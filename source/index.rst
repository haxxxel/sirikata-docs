.. Sirikata Documentation
   Copyright 2011, Ewen Cheslack-Postava.
   CC-BY, see LICENSE file for details.

.. _welcome:

Welcome
=======

What is Sirikata?
-----------------

.. sidebar:: A Sirikata Application

   .. image:: images/kataspace_screenshot.*
      :width: 100%

   KataSpace is an example application built with Sirikata. Users
   join the world as an avatar and can chat with each other.

Sirikata is a BSD-licensed platform for virtual world
applications. Virtual worlds are shared, online, 3D, interactive
spaces. They have a lot of applications -- from games and
entertainment, to education, to social applications.
Sirikata makes creating virtual worlds easy.


How do I use Sirikata?
----------------------

Sirikata provides all the tools for creating a virtual world, but it
isn't a single world that you can log into. Instead, you use Sirikata
to create your own world or an *application* in a Sirikata-based
world.

There are two types of users of Sirikata:

.. glossary::

   Application Developers
     A user that participates in a Sirikata-based world created by a
     world developer and adds content to it, for instance adding
     objects, their associated appearance, and adding behavior to them
     to bring the world to life.

   World Developers
     A user that takes components of Sirikata, configures them, and
     ties them together to create their own, unique world.

While application developers might be end-users, generally this guide
is not for end-users.  The world developer is responsible for
providing documentation for them since Sirikata is highly customizable
-- clients for different worlds may look different, respond
differently to interaction, and permit different actions in the world.

Both types of users should start out by understanding the
:ref:`general-architecture`: you need to understand the components of the
system at a high level and how they work together to create the world
before you can understand how to setup a world or build within a
world.

Application developers should proceed to the :ref:`app-guide`, and
will likely want to begin with the :ref:`emerson-tutorials`. World
developers should then read about the :ref:`ecosystem` to understand
their options when constructing a world and then proceed to the
:ref:`world-guide`.

For a complete list of available documentation, see the
:doc:`contents`. Or :ref:`search` for a particular topic.


How do I help develop the Sirikata platform?
--------------------------------------------

Found a bug and want to try fixing it? Need functionality that isn't
built into Sirikata? If you want to dig into the Sirikata platform
code (rather than building on it), check out the
:ref:`platform-guide`.
