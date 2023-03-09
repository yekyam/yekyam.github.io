---
layout: post
title: "The Key to C"
---

Original post found [here](https://dev.to/yekyam/the-key-to-c-n6i)

## C for Beginners?
Let's be clear here: no matter the popularity, C is a difficult language for beginners. To truly understand C, beginners have to learn both about programming and hardware, two very deep and complex topics. 

In this article, I'll attempt to bridge the two.

### Some background: Types? What are they?

A type is simply a description of a variable. More precisely, a type describes what operations can be performed with the given data.

## The Key to C: Types are an Illusion

To truly understand C, one must view an entire program in terms of operations on interpretations of raw data. 

*Operations on interpretations of raw data? What does that mean?*

Let's break that down with an example: 

A `char` isn't just a single character, it's an interpretation of the data one byte holds. For example, this block of code is perfectly valid:

```
char x = '2';
char y = '2';

char z = x + y;
printf("%d\n", (int)z); // Outputs '100'
```

Again, `char` is just an interpretation of the data. Sure, most of the time, a `char` refers to an actual character, but the only guarantee C gives us is that a char is exactly one byte. 

In the example code, I performed an operation (addition) on raw data, even though the data is interpreted as characters. In ASCII, '2' is represented by the decimal number 50. 


*Viewing types as illusions simplifies all of the more difficult topics in C.*

Sure, you can view an `int` is a number, but then what is an `int*`? It points to a number? What does that even mean?

Or, you can view an `int` is a (typically) 4 byte block of memory on the stack, and an `int*` holds a number that is usually the address of a (typically) 4 byte block. 

Yes, it's a mouthful, but until this line of thinking becomes intuitive, it's important to break down components of a program in that manner to truly understand C.


If C hasn't clicked for you yet, try viewing your programs in the way I've described. You'll surprise yourself! 
