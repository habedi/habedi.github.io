---
layout: post
title: Why Programming is Difficult
tags: 
year: 2014
month: 2
day: 7
published: true
---

Many years ago I used to think that programming was easy, as the years
have passed I have have realized that programming is not easy. This is
due to a slow perceptual shift in what I think programming is and what
it is that a programmer does.

At first I thought programming just involved telling a computer what to
do, this part of programming is relatively easy. After practicing for
twenty odd years I reckoned that this part of programming was pretty
easy.


<img src='/images/program.png'/>

Definition1: A program is something that transforms inputs to outputs,

A programmer is the person who writes the program, and programming
is the act of writing this program.

Now let's add a few constraints to my definition of program.

Definition2:  A program is something that transforms inputs to outputs, subject to the
following constraints:

+ The output of the program is beautiful.
+ The input to the program is beautiful.
+ The program is beautiful.
+ The input to the program is well and correctly documented.
+ The program itself is well and correctly documented.
+ The program is well tested and proved correct.
+ The problem being solved is well specified.
+ The problem is well specified.

With these added constraints programming becomes extremely difficult.

Now for a particular problem some of these constraints can be relaxed.

Several typical scenarios suggest themselves:

# Programs that do not have to be maintained

Often we write program for the output only. In this case the input and
program itself will not have to be maintained in the future and so
don't have to be particularly beautiful or well described.

My Erlang book is an example of this. Once the book is published,
maintaining the inputs and the program that produced the book is not
necessary. The result looks nice, but the input is a mess of XML files
and small test programs that will never have to be maintained.

Errata for the book, and corrections necessary made in re-printings
will only involve small changes to the inputs which are easy to make,
even if the inputs to the program are not well documented.

# Programs that must be maintained

For program that have to be maintained the opposite to the last scenario
is true. The inputs to the program and the program itself must be
beautiful and well-documented.

I was talking to a computer consultant the other day who made web
applications. He said that as soon as the output of the program
looked correct (ie the web site looked OK and the program appeared to
work) the customer assumed the project was over and the project
managers moved him on to the next project.

There was no time, and no understanding for the point of view that
not only should the web site look ok, but the code that produced it
should be cleaned up and documented before the next project was
started. And this was for projects that would have to be maintained in
the future.

# What else makes programming difficult?

There are three other things that make programming difficult:

* Fixing things that should not be broken
* No time for learning things
* Bad environment for programming

Let's look at these things - these are all ``time thieves''

# Fixing things that should not be broken

Often I have to use software that I have not written and do not really
understand in order to solve a particular problem.

In the best case, the program that I have to use has an accurate
description of how to use it. But what often happens is that the
program has either no description or an incorrect description.

So what do you do when the documentation says ``do XYZ and PQR will
happen'' and you do ``XYZ'' but ``PQR'' does not happen? If you are
lucky the person who wrote the program is nearby so you can go and
strangle them, failing this you can either try your luck with Google,
or poke around in the source code looking for the answer.

Using the Google casino for bug fixing is terribly frustrating. I
Google a bit and after a while find a posting where some poor
unfortunate soul has encountered __exactly__ the same problem that I
have. My heart leaps for joy. My trembling fingers enter the magic
spell that will remove the curse, and ... nothing. The problem
remains.

How come the fix works for some people but not for me. Is there a
malicious God that is watching over me, or am I in a local area of the
universe where the laws of physics have temporarily changed? No the
initial state of our two machines are different, and so what fixes a
bug on one machine in one state will not necessarily fix a bug on a
machine in a different state.

It is at times like this that I wish I programmed in Smalltalk and
that we all started with exactly the same program image - Smalltalk
programmers must live in a kind of heaven where this cannot happen,
but then one day even their programs might have to talk to other
programs and the fun will start.

Fixing broken stuff is doubly frustrating, since even when you've made
the bug go away, you don't really know if it was the last thing you
did that fixed the problem or the net effect of all the changes you
made.

This problem is, by the way, the thing that takes most of my time,
60-70% of my time at a guess. I once spent over a week trying to get
a broken LDAP server to work - my boss had forbidden me to implement my own
LDAP server - but after a week of struggling with a broken LDAP sever
written in C and badly documented I had a lapse of memory and forget 
that my boss said and accidentally implemented a server from scratch
in Erlang during my lunch break.

To be honest it wasn't a full LDAP sever, but I didn't want a full LDAP server.
I only wanted a couple of commands to work, and that was pretty easy to fix.

Now I find no particular joy in implementing archaic and perverse protocols
but often the quickest way to progress is to re-implement them from scratch.

# Solving problems but not learning

I'm lazy, I'm a good for nothing slacker. But when I want to put a
diagram into LaTeX I don't want to have to read a 391 page manual
first. Now I know you will accuse me of laziness and of being of unsound
moral character, and I know I should read the friendly manual first,
but I want a diagram in my document within ten
minutes reading the 391 page manual is off the radar.

When solving problems I go for the quick solution method - but in the
long term this is disastrous.

Take document production. I vacillate between TeX/LaTeX and XSLT-FO
and my own Erlguten.

About once every three years I get a strong desire to write all my
documents directly in postscript, and then the only thing to do is
take a deep breath and wait until the feeling goes away.


I guess Giambattista Bondoni when he produced his Manuale Tipografico
in 1818 wasn't particular concerned if typesetting a single page took a
few weeks but now when we have much more time because we can get
machines to do the boring and dangerous stuff we don't have time to do
things properly.

I asked my boss if he wanted ``nice slides'' for the next
presentation, he said yes, provided I got them to him before
tomorrow. This leaves no time to learn TeX properly (which I guess
could be accomplished in a couple of years or so) no time to implement
my own typesetting language (which would take five-ten years) no time
to write it directly in postscript (a week or so) - So I guess it's
PowerPoint.

# Bad environment for programming

If you've gotten this far, you'll understand that I think programming is pretty
damn difficult. That's why workplaces are designed to make programming even more
difficult. We have open plan offices which provide a noisy environment
to break our concentration, mobile phones to disturb us, and internet to distract us.

Fortunately we have place to go to where we cannot be
distracted. Sleep. A lot of programming problems get solved while you
sleep.

There are two methods. First you load your brain with
problems, then you sleep then the next day you wake up and some of
the problems are solved. Easy.

Method two - you post your problem on the internet, or tweet about it
before going to bed and the next day somebody has mailed you the solution.

Being a good programmer takes a long time, you need to learn lots of
stuff and you need to know who to ask when you get stuck.

# Amazing but true

When I'd finished this article, I wanted to spell check the content.
emacs-ispell mode decided to go on strike. It could not find aspell,
the program that I use for spelling checking.

My emacs spell checker has worked faithfully on this machine for
several years. And just when I complain that I spend half my life
fixing things that shouldn't be broken the emacs spell checker decides
to break.

I don't believe in malicious Gods, nor that the laws of physics are
different in the left-hand corner of the sofa in my front room where
I'm typing this, though there is circumstantial evidence to suggest the
contrary.

I could see no reason why my spelling checker should break - Everything
is fine, I have changed nothing. Well I have installed a new version
of Erlang and installed Julia and written some lecture notes since I
last spell checked a document.

Fortunately eleven minutes with the Google casino worked. The second
suggestion of how to fix my problem worked - and I still don't know
why emacs could not find aspell - and life is too short to find out
why.

I guess there are some things we'll just never know.

 
