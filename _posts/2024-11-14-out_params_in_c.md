---
layout: post
title: "Let's All Agree to Stop Using Out Params In C"
date: 2024-11-14
---

<!--toc:start-->
- [Background](#background)
- [What are Out Params?](#what-are-out-params)
- [Why not use out params?](#why-not-use-out-params)
- [What's the alternative?](#whats-the-alternative)
- [When is it okay to use out params?](#when-is-it-okay-to-use-out-params)
- [How to make out params less ambiguous?](#how-to-make-out-params-less-ambiguous)
  - [Mark non-out params as const](#mark-non-out-params-as-const)
  - [Name parameters with an out prefix or suffix](#name-parameters-with-an-out-prefix-or-suffix)
- [Conclusion](#conclusion)
<!--toc:end-->

## Background

Hi, my name is Manuel, thank you for welcoming me to your therapy group.

I'm guilty of...using out params in C. 

\*colective gasps from the group\*

Look, let's be frank: the "out param" pattern is extremely common in C. It's everywhere in the standard library,
it's everywhere in system libraries, it's everywhere in probably every known C library. But folks, it's not the '70s
anymore. \*Personal computers are *fast* and have *tons of memory*. We don't need to keep using out params.

(\* Mini disclaimer here: this advice mainly applies to programs in modern environments. )
## What are Out Params?

If you're new to C, you're probably confused on what an "out param" is. To understand what they are, one must know
a few things:
- A parameter describes an input to a function
- C is "pass by value", which means that the value of arguments are passed as parameters to a function
- A pointer can be used to edit any place in memory, including the memory used by a variable

Let's put it all together:

```
#include <stdio.h>

void increment(int* x) {
    *x = *x + 1;
}

int main() {
    int magic_num = 255;
    
    increment(&magic_num);
    
    printf("%d\n", magic_num);
}
```

Here `increment` is a function that takes a pointer to an integer, and then adds one to that integer.
The parameter `x` is called an "out param"; an out param is any parameter that is edited by a function.

## Why not use out params?

Here are my main issues with out params:
1. Out params make the usage of function arguments ambiguous
2. Out params disrupt the "rhythm" of a codebase 

Let's look at my first point; here's a fictional function signature:
```
int create_file(struct file_format*, FILE*);
```

So this function signature is actually somewhat clear. An experienced C programmer might think that the
first parameter is a read-only pointer, and the second parameter is the actual out param, with the return value being
used to hold an error code. And this is correct, but it's still ambiguous; what *actually* seperates the first two 
parameters from each other?

Let's look at a different function signature:

```
int create_files(int, char*, struct file_format*, FILE**);
```

Uhhhh... 

So the first parameter is definitely not an out param, since it's not a pointer. So what's the second? Maybe the 
last two are out params? Are there actually any out params?

Here, because we're dealing with four params, things get complicated. We'd need to jump into the documentation,
or even worse, the body of the function, to really see how the parameters are being used. *This is bad.* Although 
I do believe that documentation is important, code should aim to be readable without it. 

Let's now talk about how out parameters disrupt the "rhythm" of a codebase.

If I had to give a definition of what "rhythm" is in a codebases, I'd say its the many patterns and styles 
which a codebase employs, and how those patterns fit together. C codebases employ a multitude of patterns and 
styles, ranging from declarative to object oriented. Having a codebase with a good "rhythm" helps readability and 
extensibility. Once a programmer understands the rhythm of a codebase, they can often guess how functions should be
used, how new functions will be expected to use, and where exactly things should be added. 

Back to out params: they break rhythm. If a codebase  all of a sudden throws out params everywhere left and 
right, a developer can't build up a sense of rhythm. This slows down development time, and also introduces more 
bugs, since functions won't be used properly. 


## What's the alternative?

An easy alternative is to not use out parameters at all. Going back to our function signatures above, we might 
rewrite them as:

```
struct create_file_result {
  int rc;
  FILE* file;  
};

struct create_files_result {
  int rc;
  int num_files
  FILE** files;  
};

struct create_file_result* create_file(struct file_format*);

struct create_files_result* create_files(int, char*, struct file_format*);
```

Here, we're assured that none of the params are out params. And by making sure that all of our return variables
are in one place, things are definitely more organized. If we keep the pattern of "each return struct has at least 
a return code", then the rhythm becomes very predictable: 
1. Call function
2. if `struct some_result.rc != 0`, handle errors
3. Use variables in the given result

One common criticism of this is that too many results would lead to a polluted namespace, and that a programmer 
would have to constantly a) create return structs and b) check what the variables in the many structs are. I'd argue
that I'd *much* rather check the members of a return struct over having to read through the body of a function 
*every time* I want to check which ones are return params. Also, reiterating what I said above, the pattern becomes
almost second nature. 

## When is it okay to use out params?

I'll get off my soap box and step back in to reality. The truth is, there's a time and a place for almost every 
programming pattern, and that includes out params. 

In general, out params are okay in these scenarios:
1. Recreating common standard library utilites
2. When using a function almost like a method

The first scenario doesn't need much explaining. Of course, if you're making your own `sprintf`, just use a similar
function signature. No need to complicate anything there, the damage was already done.

Let's look at scenario two:

```
struct Point {
    int x, y;
};

int point_init(Point*, int, int);

int point_add_x(Point*, int);
int point_add_y(Point*, int);
// other functions here
```

Lots of C codebases follow an object-oriented style. Here, it's totally okay (and in fact, recommended) to use
an out param, since it takes the place of a typical `self` parameter. This pattern is not only common in C,
but it's also common in other object-oriented languages (Python, C++, Rust (sort of)).

## How to make out params less ambiguous?

If you plan on using out params, here are some recommendations to make them less ambiguous.

### Mark non-out params as const 
```
int create_files(int, const char*, const struct file_format*, FILE**);
```

Here, it's clear that the only paramter that should be modified is the last one. The rest of the paramters 
should remain untouched. Of course, `const` is a very loose contract, it's fairly easy to get around if needed,
but getting around `const` is not the norm.

### Name parameters with an out prefix or suffix
```
int create_files(int num_files, char* dir_path, const struct file_format* format, FILE** files_out);
```

Here, the out param is explicitly named with the suffix. There aren't any semantic guarantees that the other 
pointers will remain untouched, but it can be assumed that the only out param is `files_out`.

I don't think this solution is good by itself, but it works pretty well in conjuction with marking non-out params as
const. 

## Conclusion

Overall, it's been over 50 years since C has been around (more than double my age!). Let's all agree to make our
code just slightly more readable by using less out params.
