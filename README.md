# Swarm Demo
Copyright © 2012 Bart Massey

This program is licensed under the "MIT License".  Please
see the file COPYING in the source distribution of this
software for license terms.

This is a simple Java 2D graphical physics simulation built
around "bugs", little motile thingies with a tail that
points away from their facing. Build it with just "`javac
*.java`". You can invoke it with "`java Swarm 10`" to get a
swarm of 10 bugs. If you are going to use more than about
100 bugs, you should probably reduce their size
substantially or the simulator will never place them all and
they won't be able to move very well.

The idea is to subclass the `AI` class of the bugs to provide
an `AI` for a particular `Agent` in the simulation. The `AI` gets
sensor data and can exert control over the `Agent`'s
motion. The default `AI` just tries to drive the bug to the
center of the screen. More interesting `AI`'s are possible. It
is also relatively straightforward to add new Agents or even
passive `Things` to the simulation, and to give the new stuff
useful behavior.  In short, this is just a framework, not a
final answer.

The sensor and actuator data is mediated by the `View`
classes. Take a look at how `Agent` sets up views to get some
idea of what's happening here.

The physics of the simulation is controlled by a number of
per-entity constants; minimum and maximum reverse and
forward acceleration `AMIN` and `AMAX`, minimum and maximum
right and left turn velocity `VTMIN` and `VTMAX`, and a
coefficient of drag `CDRAG` for a velocity-dependent,
size-squared-dependent drag term.

The whole simulation takes place in a `Double` coordinate
space ranging from 0..1. The physics simulation timestep is
governed by `Playfield.DT`, which is echoed as "`dt`" in various
classes. Some effort has been expended to make sure that the
whole thing runs in roughly "real-time"; one simulated
second per wall-clock second. The frame rate of the display
is normally less than the timestep rate, for efficiency. The
display frame rate is normally 200ms or faster, which gives
an adequate animation.

The `Agent` physics are currently built to model `x` and `y`
position, facing angle `t`, velocity `v` in the facing
direction, angular velocity `vt`, and acceleration `a` in the
facing direction. The only currently controllable parameters
are `a` and `vt`, but of course this can easily be changed.

Remember that an object with acceleration `a` and velocity `v`
will move forward at a rate

        v + a * dt

Thus, the object will move a total
distance of

        v * t + 0.5 * a * t * t

in time `t`. The damping
term mentioned above will squash this a bit, though.

For dealing with angles, it is useful to recall that

        dx = dr * Math.cos(t)
        dy = -dr * Math.sin(t)
        t = Math.atan2(dy, dx)

It is also good to remember that angles are typically
measured in radians, `2 * Math.PI` of them around the unit
circle. The class method `AI.angleDiff()` will compute the
difference between two angles "the short way around" so that
you don't make extra-long left turns.

Currently, when bugs collide with each other or a wall, all
bugs involved in the collision instantly lose all their
acceleration and velocity. They still turn at the normal
rate, but they typically get "stuck" and can't move until
they either back up or turn around.

There is a Makefile for building this. It's kind of kludgy,
but it works. Say "`make`" to build the demo. Say
"`make javadoc`" to build javadocs in the "doc" subdirectory.

Mostly, this code is supposed to be fun to play with. Enjoy!
