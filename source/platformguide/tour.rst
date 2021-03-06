.. Sirikata Documentation
   Copyright 2011, Ewen Cheslack-Postava.
   CC-BY, see LICENSE file for details.

.. _platform-tour:

A Tour of the Sirikata Code
===========================

In order to extend or modify the Sirikata code, you'll need a basic
understanding of the components involved and where to find them in the
code base.  If you aren't already, be sure you are familiar with the
high level components of the system described in
:ref:`general-architecture`. This tour will build on these three.

Implementation Approach
-----------------------

The Sirikata implementation is broken down into libraries, binaries,
and plugins.

Libraries hold the bulk of the common functionality and all the
exported interfaces in the system. They are organized logically based
on high level tasks. By keeping most of the code in libraries it is
reusable: they contain all the components or interfaces but don't full
connect them.

The binaries put the pieces together, making sure all the necessary
components for a service are available, instantiate them, connect
them, and manage the service. Binaries are usually pretty minimal
since they mostly connect components available in libraries or
plugins. Usually developers won't need to add new binaries unless they
are adding a truly new service to the system: the existing binaries
can be customized via plugins. However, if you needed some custom
functionality not addressed by the system, for instance to perform
additional logging for which there isn't an interface it might require
creating a custom binary.

Finally, plugins contain the specialized functionality of the
system. Plugins usually contain an implementation of an interface from
one of the libraries and add it to a factory so a binary can
instantiate it.  This is probably the most common location for code
modifications -- the libraries and binaries are setup to provide most
of the functionality expected -- allowing you to add implementations
with different properties (e.g. storage for object scripts backed by a
system with strong durability guarantees) or just to extend the
features of an existing plugin (e.g. add some new utility API for a
scripting language).


Code Layout Conventions
-----------------------

Navigating the code base is made simpler by following a small set of
conventions.

#. Each library or binary is organized into its own directory. Library
   directories are prefixed with ``lib``, e.g. ``libmesh``, and binaries
   aren't, e.g. ``space``.
#. Source within each code directory should be broken into a standard
   set of directories: ``include`` for public header files, ``src`` for
   internal headers and source files, and ``plugins`` for all code
   associated with plugins for interfaces in that library.
#. Plugins are always associated with a library, not a binary. Plugins
   implement generic interfaces and these generic interfaces should
   always be in libraries (binaries should not export any additional
   symbols).

Of course there are some additional directories that don't follow
these conventions, but they are easily identified. Examples include
the ``scripts`` and ``build`` directories.

As an example, here are is the directory layout for a few items in the
repository::

   libcore/
      include/
         sirikata/core/*.hpp
      src/
      plugins/
         local/
      test/
   space/
      src/

The library has longer include directories to make the directory
structure work for both in-tree builds and when the libraries have
been installed into system directories. Since only libraries should
have headers installed, only libraries require this structure.

In this example, libcore has a plugin ``local`` in addition to its
``include`` and ``src`` directories. It also has some unit tests
associated with it, stored in a separate ``test`` directory.  The space
binary only has a source directory.


A Tour of the Libraries and Binaries
------------------------------------

.. sidebar:: Additional Libraries and Binaries

   You'll notice that there are some additional directories for
   libraries and binaries:

   * ``libsqlite`` provides access to SQLite databases. It is a
     library since SQLite is used by plugins associated with multiple
     libraries.

   * ``analysis`` performs analysis of trace data from measurement
     experiments of the system. This reports data which helps evaluate
     the performance and correctness of the system.

   * ``bench`` performs a few low-level benchmarks.

   * ``cseg`` is a service which the CoordinateSegmentation in the
     space server communicates with. It organizes and coordinates
     space servers.

   * ``pinto`` is a service which the Proximity service in the space
     server communicates with. It aids in the query handling process
     for large deployments with many servers.

   * ``simoh`` is a stripped down simulated object host used for
     automatic evaluation of the system.  It lacks true scripting
     support, opting for simplicity and minimalism to accurately
     benchmark the system.

   These won't be discussed further in this guide as they are more
   would require more detail than this guide aims to give.

At a high level, the system is broken down into two binaries which
clearly map to the space and object host components:

   * ``space`` is the space server binary. It can be run in isolation
     or on many servers to simulate a larger world.

   * ``cppoh`` is the object host binary. It ties together the
     components for simulating objects -- storage, scripting, display,
     etc.

As mentioned earlier, these binaries should be minimal. They should
instantiate implementations (in plugins) of interfaces (in
libraries). Therefore, they depend on a number of libraries:

   * ``libcore`` provides **a lot** of core utilities across a range
     of functionality. As you're adding code, this library will be a
     powerful resource for common functionality -- options, network
     utilities, CDN access, geometric data types, threads and locking,
     platform abstractions for dealing with OS interaction like
     signals, factory and listener patterns, etc.

   * ``libmesh`` provides basic data structures for dealing with
     meshes and generic interfaces for mesh processing. Plugins for
     libmesh implement loading and saving for different file formats
     and "filters" for meshes, e.g. for simplification, computing
     statistics, or any type of content conditioning.

   * ``libproxyobject`` provides functionality organized around
     "proxies" or replicated objects. Proxies are objects which you
     have learned about and may have knowledge of their location,
     appearance, and messaging identifier, but they are not local. For
     most developers this library is most relevant when dealing with
     display since display is focused around these basic properties
     and a display plugin lives under this library.

   * ``libspace`` contains all the interfaces and generic services for
     space servers. This include the generic parts of session
     management, message forwarding, location management, and
     querying, as well as interfaces for OSeg (routing table), CSeg
     (region to space server mapping), and PIntO (object queries).

   * ``liboh`` contains all the interfaces and generic services for
     object hosts. This includes the code for managing connections to
     space servers and interacting with them, managing collections of
     objects, and the interface for object scripts.

This figure shows how the binaries depend on these libraries (and the
libraries depend on each other).

.. image:: images/lib_binaries.*

Since plugins are stored within the directories of the library they
are associated with, most code specific to a space is under
``libspace`` and most code specific to object hosts is under
``liboh``. However, code useful to both appears in the other
libraries, upon which ``libspace`` and ``liboh`` depend.  The
separation of these core libraries is simply to logically partition
the code to make it more manageable. Note that there are direct
dependencies that are only implicit transitively in the image. For
instance, both ``liboh`` and ``libspace`` depend directly on
``libcore``, but this is only implied by their dependence on
``libmesh`` and ``libproxyobject``.

You may also notice that the content distribution network is missing
from this description. Currently, the CDN uses stock web servers, so
its implementation is separated and not referenced in the remainder of
this guide. It can be found in the ``cdn`` directory. Access to the
CDN is provided in the ``transfer`` directories in ``libcore`` since
it is used throughout the system.
