---
layout: post
title: "The Beginner's Axioms"
---

# Formalizing C Patterns: The Case for C++

Original post found [here](https://dev.to/yekyam/formalizing-c-patterns-the-case-for-c-481o/)


## Background

Online, I see a lot of discourse surrounding C and C++. It makes sense; both languages are related, and they fit very similar niches. C programmers typically describe C++ as bloated, overcomplicated, obtuse, and so on.

[Here's one of the most famous examples of a C programmer arguing against C++, by one of the most famous programmers of all time: Linus Torvalds](http://harmful.cat-v.org/software/c++/linus). As seen in Torvalds' rant, one key piece missing from these discussions are concrete, technical arguments. Although Torvalds is a far better programmer than I am, his argument features charged language with non-concrete examples.

This article serves as a way to document my own thoughts on the subject matter, explicitly states concrete arguments, and is a time capsule for future me to look back on.

## My Take 

C++ can do everything that C can, but the opposite is not true.

In online discussions, people tend to forget that C++ is the natural evolution of C. If C is bulbasaur, C++ is ivysaur. Because C++ did not come out of thin air, and because it does not aim to replace C, C++ borrows, formalizes, and improves plenty of common C techniques and paradigms.


## Formalizing?

C++ wasn’t created in a vacuum. It was created after years of C programming, meaning C++ had a lot of experience in what patterns worked and what didn’t. It moved these design patterns into formal language features instead of letting the users hand roll them.

*This means that good C code tends to be a more primitive version of good C++ code.*

## Generic Objects - C

Imagine I wanted to create a library to represent a two element vector. So, I create a struct containing two integer elements: 

```
typedef struct vec2_t
{
    int x; 
    int y;
} Vec2;
```

Awesome, I can now represent a vector in my code.

But wait, wouldn’t it be useful to define some common operations? 

For brevity, we’ll only implement addition, but you get the idea:

```
Vec2 add(Vec2 v1, Vec2 v2)
{
    Vec2 result = {0, 0};
    result.x = v1.x + v2.x;
    result.y = v1.y + v2.y;
    return result;
}
```

But wait, what if I want to support floats too? Or long ints? Or doubles? 

I could do some void* voodoo, but macros would work here too.

```
#define DECL_VEC2(typename, type) \
typedef struct typename##_t { \
    type x, y;\
} typename; \
\
typename typename##_add(typename v1, typename v2) { \
    typename result;\
    result.x = v1.x + v2.x;\
    result.y = v1.y + v2.y;\
    return result;\
}

```

Sweet, we’ve created a generic data structure to represent a two element vector. 

There's still an issue with this; what about `BigInt`s? Or custom, fixed-point types? All of a sudden, I'd need to change my add operation function to accept a third `add` function parameter.  

Let’s identify what makes this good C code:
- We have a good interface to interact with our structure
- Our structure is generic, meaning we can reuse our code without duplicating it for other types.

## Generic Objects - C++

C++ looks at what went right and formalizes it though the language and type system.

The proper way of doing what we did in the C code would look something like this:

```
template <typename T>
struct Vec2
{
	T x, y;

	static Vec2<T> add(Vec2<T> v1, Vec2<T> v2)
	{
		Vec2<T> result;
		result.x = v1.x + v2.x;
		result.y = v1.y + v2.y;
		return result;
	}
};
```

In this example, we have the help of the compiler to enforce type safety, and we don't have to deal with macros. C++ formalizes the generic object pattern into a built-in language feature.

We also don't need to worry about passing in an `add` function to our code. Why? Because operator overloading exists, and the `+` operator should be defined for the type passed in. Using traits, we can even expand the code to check if `T` implements the operator and report errors at compile time. Sweet.

## Cleanup - C

Contrary to common belief, in C, `goto` can actually be quite useful. 

When we're dealing with lots of memory allocations, often times we need a way to bail out of a function that has reached an error state:

```
void some_func(const char* filename)
{
    FILE* file = fopen(filename, "r");
    if (!file)
        goto no_cleanup;

    fseek(file, 0L, SEEK_END);
    int file_size = ftell(file);
    
    char* buffer = malloc(file_size);
    if (!buffer)
        goto cleanup_file;
    FILE* out_file = fopen("outfile", "w");
    if (!out_file)
        goto cleanup_buffer;

    // no errors, free to continue with operations

cleanup_buffer:
    free(buffer);
cleanup_file:
    fclose(file);
no_cleanup:
    return;
}
```

Here, we've organized our code in a way where we bail out of normal execution into our cleanup code if we encounter any problems. Without `goto`, our code would be a mess of nested if statements, increasing depth every time we add a new resource.

## Cleanup - C++

C++ once again formalizes a good C pattern. In this case, the formalized pattern is called RAII.

In C++, we can use RAII to cleanup objects at the end of their lifetimes.

```
void some_func(const std::string& filename)
{
    std::fstream file(filename);
    if (!file)
        return;

    std::string buffer;

    std::fstream out_file("outfile");
    if (!out_file)
        return;

    // do stuff
}
```

Notice how here we don't have any cleanup labels. This is because (well formed, memory owning) objects have destructors that automatically handle cleanup for us. If we have an error state in our `do stuff` area and need to bail out, we know our code will still cleanup our resources. 

## Data Hiding - C

When reading files, it's useful to have a dynamic array of strings, with each index representing a line of the file. Let's look at a simple C example:

```
typedef struct dynamic_string_array_t {
    int size;
    int capacity;
    char** arr;

} DynamicStringArray;

DynamicStringArray* DynamicStringArray_new(int capacity)
{
    DynamicStringArray* dyn_arr = malloc(sizeof(DynamicStringArray));
    
    dyn_arr->size = 0;
    dyn_arr->capacity = capacity;
    dyn_arr->arr = malloc(sizeof(char*) * capacity);
 
    return dyn_arr;
}

bool DynamicStringArray_push_back(DynamicStringArray* this, char* str)
{
    if (this->size == this->capacity)
    {
        int new_size = this->capacity * 2;
        this->arr = realloc(this->arr, new_size);
        this->capacity = new_size;

        if(!this->arr)
            return false;
    }
    this->arr[this->size] = str;
    this->size++;
    return true;
}

```

Of course, we'd need to define the other functions to operate on our dynamic array, but this works as a good example.

There is an issue: there's nothing stopping another programmer from going in and misusing our dynamic array by changing the fields. If they directly change any of the fields in our struct, we'd run into potential UB or an outright crash. 

The most naive way to "hide" our data is to not hide it all. Instead, we could prefix our fields with a single underscore to indicate that the field should be considered private. However, that's not effective.

The "best" way to hide our data is to make our struct an opaque struct. An opaque struct is one that completely hides all fields by only being accessible through a pointer.

To accomplish this, we `typedef` the name of the struct in our header file, and keep the implementation of the struct in the source file.

DynamicStringArray.h:
```
typedef struct dynamic_string_array_t DynamicStringArray;

DynamicStringArray* DynamicStringArray_new(int);
boolean DynamicStringArray_push_back(DynamicStringArray*, char*);
```

DynamicStringArray.c:
```
typedef struct dynamic_string_array_t {
    int size;
    int capacity;
    char** arr;

} DynamicStringArray;

// rest of our function implementations

```

## Data Hiding - C++

In C++, the default way is easy: just mark the fields as private, or in the case of a class, don't mark the fields public. This allows our code to be header only, which can be nice.

```
class DynamicStringArray
{
    int size;
    int capacity;
    char** arr;

public:
    // define methods here
};

``` 

## Did you see the error?

In the section "Data Hiding - C" I purposefully left a bug. Instead of allocating space and copying the contents of the `char*` parameter, I directly stored the pointer into the array. This is a bug; there is no guarantee that the string is properly managed. If our dynamic array outlives the string, then the pointer would point to garbage memory. If the string is not null-terminated, that would also lead to potential bugs.

In C++, we usually don't need to worry about string handling issues like this. Non-reference object parameters automatically call their respective copy constructors, meaning we wouldn't need to worry about copying our string. On top of that, `std::string` manages the contents of our string, meaning we don't need to worry if the internal array is null-terminated or not (unless constructing from a char*).  

## Wrapping Up

The list goes on, seriously. I could've talked about:

- Signals and `errno`, return codes vs Exceptions
- Custom pointer and size structs vs Spans
- Bit set macros vs std::bitset
- Compile time facilities
- **char vs std::string**
- Etc, etc, etc.

But at that point, this article would turn into a short book.

I'd like to reiterate my earlier point: C++ can do everything that C can. If you take issue with any of these features, there's nothing forcing you to use them. C++ features are *opt in*; if you don't want to use them, there's no penalty. Even if you use only 1 C++ feature, then it's worth the switch.
