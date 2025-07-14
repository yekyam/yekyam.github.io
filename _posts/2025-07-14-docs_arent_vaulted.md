---
layout: post
title: "Is Documentation Worth the Effort?"
date: 2025-07-14
---

<!--toc:start-->
- [Intro](#intro)
- [A Short Answer](#a-short-answer)
- [A Longer Answer](#a-longer-answer)
- [What is Documentation??](#what-is-documentation)
- [What are the advantages of creating documentation?](#what-are-the-advantages-of-creating-documentation)
- [What are the disadvantages?](#what-are-the-disadvantages)
  - [So, is documentation worth writing?](#so-is-documentation-worth-writing)
- [A and the Super-Awesome-MVP](#a-and-the-super-awesome-mvp)
- [Conclusion](#conclusion)
<!--toc:end-->

## Intro

The debate around documentation has always been a heated one. One group of 
people would try to stone you if you didn't document every line, shouting that one should be able to understand an 
entire codebase without reading a single line of code. Another group of people would stone you if
you wrote *any* comments, shouting that code should be self-documenting, and that all external documentation 
(includling comments) were bound to be out-of-date. They claimed that at best, documentation was a waste of time,
and at worst, it was an active detriment to a codebase. Who's right??

This document (;)) serves to show my take on the whole "Is documentation worth writing?" debate.

## A Short Answer

Like most questions, the answer lies somewhere between "it depends" and "maybe". But, if I absolutely positively 
had to give a short answer, I would say yes, documentation is worth writing.

## A Longer Answer

But, to really, properly, answer the question, we need to understand a few things:

- What *is* documentation?
- What are the advantages of creating documentation?
- What are the disadvantages?
- What happens if a project doesn't have documentation?

## What is Documentation??

Documentation is simply any "\*text" that accompanies a piece of software, that describes the usage or 
workings of that software. 

\*text in this case can mean written words, diagrams, pictures, videos, etc. Anything with meaning, basically.

Some common examples of documentation include:
- A README file describing installation and usage
- A PDF detailing the workings of a software
- A website which hosts information that details the workings of a software

## What are the advantages of creating documentation?

There are lots of good reasons to actually create documentation. From an external point of view,
documentation is important to anyone who wants to know how to use a piece of software. No matter how intuitive a design 
may seem, nothing beats having an actual instruction manual detailing what does what. 

From an internal point of view, documentation is important for both new and old contributers alike. For an old contributor,
they may not have worked on a particular section in a while, and may need to refresh their memory on certain implemenation
details. Nobody can remember every single detail of every single project they've worked/are working on. Meanwhile, a new
contributor will also value documentation to get a clear, easy overview of the working of a software. Instead of having to 
reverse-engineer a codebase themselves, or bug an old contributor, they can get a pretty decent overview from the docs.

Additionally, documentation can also serve as a planning tool. Instead of having an amorphous mental image of what a codebase
might look like, documentation can serve as a kind of roadmap, or sketch, of what a codebase will look like. All team members 
can refer to it!

## What are the disadvantages?

The main disadvantage I hear is that writing documentation takes time. Not only does it take time out of the MVP stage, 
but it also requires time in the future to update documentation as a codebase gains new features or gets reworked. 
If documentation isn't updated, it can become useless or even a detriment as it may confuse contributors. 

### So, is documentation worth writing?

Absolutely. In the short-term, documentation may seem like a waste of time, especially in the MVP phase. However,
any time saved *actually* results in a time deficit down the line. How? Buckle up for some story time.

## A and the Super-Awesome-MVP

Imagine we have contributor "A" who is a super-dev. 
In fact, A is so good, they knock out a 10k line MVP in a weekend, and to do so, they don't write any documentation.

A's project is awesome, and their boss greenlights A getting two new devs on the project, B and C. 
Now, B and C have to spend some time digesting the codebase, but because they're both new 
on the project, and were added at the same time, A is able to help B and C with their questions.
So, our super-team continues, and the MVP blossoms to 50k lines, still without any documentation.

Then, A goes on a paid vacation, and to make up for it, management adds D to the team. 
D tries their best to learn about the project, but is lost due to the lack of documentation.
Of course, B and C try their best, but turns out that they didn't understand the codebase as
well as they thought they did; it was fine when A was around to answer questions, but now that 
A is gone for a while, things are in disarray. B, C, and D spend their time trying to 
reverse engineer a codebase, constantly questioning why certain things were done one way,
other things were done another. It's all confusing!

See, if A took the extra time to document their design decisions, and the codebase itself,
B, C, and D would all have a nice reference in A's absence. So, even though A was able to save a lot of time
on a personal level, the entire team needed to spend extra time wading through the codebase, causing a time deficit.

## Conclusion

Unfortunately, A and the Super-Awesome-MVP is not an uncommon story. In fact, I'd say most developers have experienced
each of those roles at some point in their careers. The best way to save time is to simply dedicate time to
writing good documentation.
