.. Sirikata Documentation
   Copyright 2011, Ewen Cheslack-Postava.
   CC-BY, see LICENSE file for details.

.. _helloworld:

Hello World -- Your First Script
================================

Hello World!
------------

Let's put together a quick hello world script.  One can do this in one
of two ways: by printing from a running entity's presence or by creating a new
entity, connecting that entity to the world, and then telling that
entity to print your message.

We'll talk about each separately below.


Print from running object
+++++++++++++++++++++++++

Presumably, you're connected to a world with already-existing entities
connected through presences.  Move close towards one (you can use the
arrow keys to navigate in flat space and 'q' and 'z' to adjust your
height).  Click on it.  A white cube should appear around the
presence's mesh as seen in the following figures:


Before clicking on object:
   .. image:: images/clickPics/sirikata0.*
      :width: 50%

After clicking on object:      
   .. image:: images/clickPics/sirikata1.*
      :width: 50%


      
This white cube means that the presence is "selected".  Type ``alt``+``s`` to
bring up a scripting window for the entity connected.  Note that although
the presence is highlighted, you will be scripting the **entity**
associated with the presence, **not** the presence itself.  The
scripting window appears as the white rectangle in the following
example image:
      
   .. image:: images/clickPics/sirikata2.*
      :width: 50%


In the window, type ``system.print("\n\nHello, World!\n\n");``.  Hit
``ctrl``+``enter`` to execute this script (or, hit the button marked
"run").  You should see "Hello, World!" appear in the scripting
window.  That should be it.  You've created your first program!  If
you want, you can enter other commands into the scripting window to do
basic math, :ref:`movepresence`, print more messages, etc.
      

Typing in command at prompt.
   .. image:: images/clickPics/sirikata3.*
      :width: 50%

Result of running hello, world command.      
   .. image:: images/clickPics/sirikata4.*
      :width: 50%

      
      
Bring up entity and print when first presence is connected
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
There are a variety of ways to create new entities in the world.
The simplest is to approach an object in the world, and ask it to
create a new entity for you.  The tutorial on :ref:`createent`
explains how a scripter can create a new entity.  This subsection
focuses on writing a script that a newly created entity would use to
print "Hello, World!" after it has connected to a new space.

Conceptually, there are several ways to approach such a task.  For
instance, one could send a request to connect to the space and
frequently check an indicator to see when that request has been
accepted, performing some action on acceptance.  Such an approach is
called "polling".  As an alternate strategy, one could instead request
a connection to the space and register a *callback* with the
underlying run-time environment so that the script is notified when
the connection request is successful.  This is called an "event-based
approach".  Emerson, for the most part, encourages event-based
scripting, and therefore uses the second strategy.

Consider the simple following script::

        function printHelloWorld(newPres)
        {
            system.print("\n\nHello, World!\n\n");
        }

        system.onPresenceConnected(printHelloWorld);

Let's break it into pieces::

        function printHelloWorld(newPres)
        {
            system.print("\n\nHello, World!\n\n");
        }

As one might guess from the print from running object section of this
tutorial, when the above function is called, it simply prints "Hello,
World!".  No surprises here.  Let's look at the next bit:

system.onPresenceConnected(printHelloWorld);

The above ``system.onPresenceConnected`` function takes in a single
argument: a function that specifies what to do when a presence is
newly connected.  When a new presence is connected, the Emerson
run-time automatically executes this function for the scripter.

In general, a scripter might write presence initialization code in
such a function, but, for our purposes, printing "Hello, World!"
suffices.
