---
layout: post
title: Minimal Viable Programs
tags: 
year: 2014
month: 2
day: 14
published: true
---

A minimal viable program is the smallest program that solves a
particular problem.  It is small and beautiful. It has no additional
features.

If you removed a single feature it would be totally useless. If you
added a new feature that feature would not be essential, you could use
the program without making use of the new feature.

Very few of the programs I use are minimal viable programs, but some
are. I'll describe one such program. This is the ticket system that
was used in the Erlang distribution.

# The Erlang Ticket System

The Erlang ticket system was designed and implemented by Peter Högfeldt
in 1986.  We needed a ticket system that was easy to use, intuitive,
reliable and we wanted it yesterday, so Peter got the job, since he
was very busy and didn't have time to take on any new jobs.

If you want a job done find the busiest person you know and give them
an extra job.  This is because the reason they are busy is that lot's
of people want them to do things because they are good at doing things
and that's why they are busy.

Peter built the ticket system in a couple of hours and we've been
using it ever since. I guess the couple of hours were divided into an
hours drinking coffee and drawing things on a white board and twenty
minutes programming.

# The Ticket System

Peter's ticket system was simple in the extreme. There was one command.
You typed **newticket** in the shell and got an integer back. Like this:

<pre>
$ new_ticket
23
</pre>

The system had made a new file in **${HOME}/tickets/23** and the content 
of the file would be:

<pre>
ticket: 23
responsible:joe@erix
status:open
title: ?
----
Describe your problem here
</pre>

This file was also checked into a global CVS archive that all project
members had access to. Today one might use GIT or SVN but any revision
control system would do.

The ticket system had a few simple rules:

* The status is open or closed
* The responsible person cannot be changed to somebody new without the permission of the new person

Project management wanted a reporting system. This was pretty easy,
this was done with a few simple shell scripts. For example to
find the number of open tickets a simple shell script does the job:

<pre>
#!/bin/sh
grep 'status:open' ${HOME}/tickets/* | wc
</pre> 

The first ticket system was operational in 1985 and we have used it ever since.

# Adding features

Do we need to add additional features? The first point to note is
there is no time or dates - but wait a moment, this file is checked into
a revision control system, so the times when the file is created and modified
are in the revision control system and do not need to be added to the ticket.

# What happened later?

Feature were added - but none that broke the original spirit of the design.

# But we can't make money from a MVP

Many companies sell "features" - so an MVP will be useless - a product
needs new features. But the MVP program will do exactly the same thing
in 100 years time as it did yesterday.

New features mean new sales opportunities, good for the company but
not good for the user.

New features mean new untested code, and backwards incompatibility
with earlier versions of the program. Things that are stable for a
long time are good.

The problem with adding features to MVP is that when we ship more
complex products like complete operating systems that are packed with
programs, the complexity of the individual programs contributes to the
complexity of the whole.

If a system shipped with one complex program it probably would not
matter - and it's difficult to imagine the idea of a MVP applying to
a complex program like photoshop.

If the individual components in a system are not MVPs we will soon be
overburdened by complexity when we start combining programs to build
larger systems.

If we to have any control over complexity then we should ensure that the
basic components are MVPs.

I really like systems that do one essential thing and do it well.
Good examples are Dropbox and Twitter. Dropbox just works. Twitter
has a no fuss 140 character tweet box. Simple, easy to understand
and minimalist.
