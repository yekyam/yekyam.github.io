---
layout: post
title: "Hot Take: Use Golang Over Python in New Project at Work"
date: 2025-07-14
---

<!--toc:start-->
- [Intro](#intro)
- [Dependency Management](#dependency-management)
  - [Python](#python)
  - [Golang](#golang)
- [Type Systems](#type-systems)
- [Speed](#speed)
<!--toc:end-->

## Intro

This'll be a shorter article detailing why greenfield projects should be written 
in Golang instead of Python.

## Dependency Management

### Python

(Honestly, I could say those two words and end the post right there.)

Dependency management in Python is what I believe to be a secret government experiment on turning all
python devs insane. 

Here are some of the dependency management tools in Python:
- pip
- pipenv
- virtualenv
- venv (yes, different from virtualenv)
- pyenv
- poetry
- conda
- pdm
- mamba
- uv

What do they do? What's the differences? Which one is better? Hint-hint: nobody knows. All of these tools
have so much overlap and cruft and annoyances and pain points and positives and negatives that it's 
almost impossible to know which one is best. (If I had to pick, I do think uv + pyenv is the best).

So yes, dependency management in Python is super annoying to deal with.

### Golang

Go on the other hand?? It's official tool manages project-level dependencies for you. It's awesome. Try it!

## Type Systems

I'm just going to be upfront about this: nobody can convince me that dynamic typing is as good as static
typing. Yes, sometimes it's nice to have generics by default, but that's only sometimes. It's nice to 
(almost) eliminate a whole swath of runtime errors just by have static typing.

I'm also a strong believer that a static type system is frankly just better in larger projects. It's
nice to have good auto-complete, and the ability to click into a type and see the methhods available. 

It's funny; because Python is dynamically typed, but so many people depend on type checking tools,
you'll see people write *more* type hints in python than they write *actual* types in Golang!

## Speed

Yes, so many people will say that speed is largely irrelavent for most projects, especially those 
communicating with an API over the internet. 

However, having that speed built-in, just available by default, is a godsend for whenever a project reaches
that "oh, we actually did need to make things fast all along" moment. 


