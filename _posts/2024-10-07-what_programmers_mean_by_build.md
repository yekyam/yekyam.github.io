---
layout: post
title: "What do programmers mean when they say `just build anything`?"
date: 2024-10-07
---

## Intro

So, you're new to programming: you've worked through a language tutorial, followed along a project guide, 
and you're ready to take your first steps into the world. You open up your favorite terminal, and then...nothing. 
You watch the cursor blink endlessly as you ponder one of the most common questions every programmer has asked 
themselves: `What do I do now?`

The question is desceptively simple. Ask any other programmer, and they'd say `Just build something.` But what is 
something? Tic Tac Toe?
Connect four? My own operating system?

Beginners often see project ideas on a sort of axis, where simple ideas fall opposite to complex ones. 
A simple idea is seen as a waste of time, meanwhile complex ones are simply far too advanced to make any 
meaningful progress.

However, this line of thinking is two things: a) an easy path to stagnating one's learning and b) completely 
misleading.

> To address both issues, one must see projects as a jumping off point, not as a final destination.

## Example

Let's go back to the tic tac toe example. A beginner should be able to knock out a simple CLI implementation in under 
hour. But how does one move past this? The solution is to keep adding on to the idea. Try and take it 
to its extreme. In each step, we'll take stock of the technologies a beginner has worked with.

```
Stage 0:
- CLI 
```

Our beginning step. Let's try and make this more complex; one common way would be to transition 
the program over to have a graphical component.

```
Stage 1: Transition to graphics
- Use a GUI library 
```

Wonderful. Now, to add complexity, lets implement some networking.

```
Stage 2: Transition to P2P multiplayer
- Add networking
```

At this point, our tiny CLI app has now become a deliverable application. 
But let's keep going. Let's add a server, and let's implement both a leaderboard system and random matchmaking.

```
Stage 3: Transition to client-server multiplayer
- Add a server
  - Add a leaderboard through a database
    - Add a basic database
  - Add random matchmaking
```

Let's go completely rewrite this application into one that can be used on the web.

```
Stage 4: Web appliication
- Transition code to be hosted on the web and playable in browser
```

It'd be nice to have some social features, no? Add friends, customize a profile, that sort of thing.

```
Stage 4: Beef up database
- Add social features
```

And so on an so forth. 

Let's reflect on all the steps taken thus far:
```
Stage 0:
- CLI 

Stage 1: Transition to graphics
- Use a GUI library 

Stage 2: Transition to P2P multiplayer
- Add networking

Stage 3: Transition to client-server multiplayer
- Add a server
  - Add a leaderboard through a database
    - Add a basic database
  - Add random matchmaking

Stage 4: Web appliication
- Transition code to be hosted on the web and playable in browser
```

The point is, *there's so many places even a "simple" project idea can go.* We took a simple tic-tac-toe game all
the way up to a full-fledged application with multiple implementations. It sure wouldn't be easy, but even 
attempting this sort of stuff is how a person gets better at programming
