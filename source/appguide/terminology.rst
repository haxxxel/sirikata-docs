.. Sirikata Documentation
   Copyright 2011, Ewen Cheslack-Postava.
   CC-BY, see LICENSE file for details.

.. _terminology:

Basic Emerson Terminology
=======================================

The tutorials and documentations throw around two terms that are
unusual, and should be defined.  These are entity and presence.
Here's a quick description of the relation between entities and presences.

An *entity* has an associated Emerson script.  It has zero, one, or
many *presences* through which it connects to a virtual world.
Each presence has a physical manifestation in the virtual world, and has
properties such as velocity, mesh, bounding volume, etc. that are
managed by the virtual world.

As a concrete and visual example of entities and presences, consider
the following figure


   .. image:: images/fishEntityPresence.png
      :width: 50%

In it, Entity A has two presences associated with the world, both
fish.  Each of these fish can move independently of the other in the
virtual world.  In contrast, all the other entities only connect to
this virtual world through one presence.  (In case you're wondering
those presences are seaweed, a starfish, and another fish for Entities
B, C, and D, respectively.)
