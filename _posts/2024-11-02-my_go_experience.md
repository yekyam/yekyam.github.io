---
layout: post
title: "My Experience with Go as a Beginner: Building a CLI Music Player"
date: 2024-11-02
---

## Go?

Yes! Go (or golang, Google's pet project, python-replacer-inator 5000, whatever you'd like to call it), has 
been sitting in my "Languages I should at least build one project in" list for quite some time. I'm not even 
sure why the language has been on my radar; from what I remember I just liked the idea of a python-like language
with the performance of C. Also, I read about a bunch of Go CLI apps, and that niche just clicked a spot in my brain that
somehow conviced me to give it a try.

## Why a Music Player?

Although I love Apple Music's iPhone app, I find that it's quite a pain to use on my Mac. Its slow to open, and
I don't like how it adds another symbol to my CMD+TAB list (yes, I'm nitpicky). I've toyed with the idea
of writing my own CLI music player in the past, but I finally decided I'd go ahead and actually do it.

For this project, I decided to keep the feature set very small:
- Be able to play music from a library
    - The player should have basic controls, like skip forward/back, pause/play, loop
    - Ideally some sort of progress bar
- Be able to add and delete songs to a library
    - Should be able to add songs given a youtube link or a file path

## Pros of Go

### Weirdly productive language

There's lots to talk about, but Go has on feature that really made it stick for me: **the language makes you
weirdly productive**.

By "weirdly productive", I meant that I felt comfortable reading and writing Go code within like...two hours of 
working through the Tour of Go website. Even before working through the Tour of Go, I found myself being able to
follow a long with the Go snippets I had come across before. Now, this could be a function of my growth as a 
programmer, but I think that's a cop-out (and also a slightly self-aggrandizing) statement.

In reality, Go is so productive because it takes the whole "only one right way to do something" sentiment almost to
an extreme. It does so by having the bare minimum language tools one would need, and forcing the user to follow that
paradigm. Object oriented programming? Let's just only take the basics of encapsulation and traits, leave the rest. 
Memory management? You can, but the GC is there and should do most of the work for you. Error Handling? Just make
functions return an error, everything else is bike shedding.  

Related to this whole "Keep the featureset small" idea, I've seen this quote thrown around quite a lot:

> The key point here is our programmers are Googlers, they’re not researchers. 
They’re typically, fairly young, fresh out of school, probably learned Java, maybe learned C or C++, 
probably learned Python. They’re not capable of understanding a brilliant language but we want to use 
them to build good software. So, the language that we give them has to be easy for them to understand and 
easy to adopt. - Rob Pike

[Source](https://www.youtube.com/watch?v=uwajp0g-bY4)

Some people absolutely hate this quote. They find that it's an affront to their intellect, and for that reason 
alone, Go should be avoided.

Other people love this quote. They find that the majority of programmers are bad, and having a language that forces
an "easy to write and easy to read" paradigm, especially for beginners, is a great thing. 

I find myself somewhere in the middle. There are *absolutely* bad programmers; by definition, half of all programmers
are below average (and frankly, I'm probably in that bottom half since I'm still a student). However, I can't help
but look at the limited feature set and think to myself: "Man, if only I had x feature, this code would probably look a 
little cleaner". For a small script like the one I wrote, a small feature set isn't a bad thing. But it doesn't take
an amazing programmer to see when this can be an issue (especially the whole `if err != nil` sprinkled everywhere).


### Build System

I like that Go has a build system. It's easy to add packages, it's easy to build projects, it's easy to run programs,
etc etc. I did see some older comments about how Go doesn't play well with private repositories, but that seems like
an older criticism. 

### Speed

Despite being a GC and compiled language, Go is both fast to run and fast to build. This is something that I didn't
notice until I went back and used some older python projects, and realized the half-second startup time was something
Go actually de-accustomed me to. 

### Extensive Standard Library

I haven't played around too much, but the standard library seems to be pretty nice to use. It's got lots of 
functionality, and unlike other languages, I didn't feel the need to reach for a 3rd party library every other 
second (I'm looking at you, C++).

### Defer
No spiel here, I just love the keyword, and I desperately with C had something like it.

### Static Typing
Also no spiel here, I'm much more of a fan of static and strongly typed languages. I find that having the compiler
catch bugs for me instead of having them crop up randomly during runtime is always better. 

### Last, but certainly not least, Goroutines

Holy cow. Why is this something that Go doesn't advertise and shout from the heavens about?? Go actually makes
concurrency--dare I say--easy and fun. The fact that I can spawn a (pseudo-)thread with one keyword is actually
quite ridiculous, in a good way. Go definitely has the easiest thread model I've come across by far.

## Cons

### Error Handling

As simple as `if err != nil` is, I think it's still worse than dealing with `Optional` types or even `Result` 
types. At the very least, it'd be nice to have some shorthand for the `if err != nil {return T{}, err}`, maybe 
something like Rust's `?` operator.

## Closing Thoughts

As you can see, I definitiely liked Go, and I've found that its pros out-weighs its cons (in this case, 7-2). I'm
not 100% sure where this will fit in terms of my language toolbox, but I think I'll start reaching for it before 
Python whenever I'm making smaller, script-like programs. Overall, I'd say I'm pretty satisfied with the language, 
and it seems like it fits my needs quite well. If you've yet to try it, don't be afraid to visit the Tour of Go
and get to working on some cool stuff.
