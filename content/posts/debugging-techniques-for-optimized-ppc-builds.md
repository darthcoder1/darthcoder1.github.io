+++
title = 'Debugging Techniques for Optimized PPC Builds'
date = 2011-07-26T13:36:15+02:00
draft = true
+++

# Debugging techniques for optimized PPC builds

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

In the last years I have given up the usage of debug builds completely. The performance was usually so bad, that it induced physical pain to play the game. Also the build and especially link-times for a debug build are just annoying on large projects. And not ignore the fact, that QA was testing the optimized builds, so remote-debugging or debugging of crash-dumps had to be done in this build either way.

But it is not that bad as some people might think. In the beginning it takes some time to get used to it, but after a few sessions, this works as good as a debug build.

This article is mostly aimed at programmers not that familiar with the lower level concepts and should help them to get the most information without the need of reading assembly.

<!--more-->

### Problems of optimized builds

1. The source code does not represent exactly the instructions that are executed
2. You will have to search for most of the variables yourself, as the resolution that is done by the debugger is mostly wrong
3. You can find everything in memory you might possibly need, you just need to find it

I will describe some techniques to get as much information about the current state as possible, without the need of reading assembly code.

### Variables

First thing you need to realize is, that no local variables, parameters and return values can be watched and interpreted directly from source-code. If you hover over some variable or type them into the watch-window, you will get random information. There are of course some cases, where the value is correct, but this is nothing you should ever rely on.

The only trustworthy types of variables are global variables and static class members. These are always correct. If they contain garbage, than it is most probably, because they are screwed for real, were overwritten or not initialized at all.

### Objects

The debugger can determine the “real” type of an object by resolving the vtable-entries, so use this to your advantage.

If you know, that there must be some kind of object at address 0xB00B5000, you can just
cast this address to any polymorphic type ( it doesn’t matter which one, it should just have a vtable). If you expand this object in the watch window, the first entry will hold the resolved vpointer and will contain a human readable name of the runtime type of this instance.

Here is an example. The address points to an instance of the class ‘UWorld’ and the debugger can determine this, no matter into which type you cast the pointer.

{{< figure src="/images/dbgoptppc_1.png" width="500">}}

{{< figure src="/images/dbgoptppc_2.png" width="500">}}

### Register Usage

The PPC ABI defines a specified register usage. This allows you to get a lot of information just by looking at the registers. Note, that these are callstack dependent.

This means, a function-call overwrites some of the registers and restores them after returning. Therefore, you cannot rely on every register if you are not at the top of the callstack. But the debugger aids you here also. Every register that was invalidated by a function-call above in the callstack is displayed without a value in the register window.

{{< figure src="/images/dbgoptppc_3.png" width="600">}}

In this picture you can see, that r0 and r3 - r12 were overwritten by another function-call. All registers that are containing values can be considered as valid.

The registers are used for clearly specified data.

```
r1         This is always the pointer to the current stack-frame.
r3  - r10  first 8 input arguments
r3  - r4   return values
r14 - r31  non-volatile registers
```

There are more register-types ( FPU registers, VMX registers ) but you should get away most of the time just with r0 - r31.

### Address Ranges

Let’s assume you are in some method-call and would like to inspect the current state of the this-pointer and it’s members.

First, check r3, which usually contains the this-pointer. As this is the first parameter register, this makes kind of sense, right? If you have no valid r3, the first thing to do, is to search the r14 – r31 for sane object addresses.

What a sane address is, is completely platform and implementation dependent. The Xbox360 for example maps 64kb memory pages to the address-range 0x40000000 – 0x7fffffff. When you know the platform and the implementation internals of your memory allocator, you can easily find out which addressrange contains which data.

So, for the sake of an example, just assume you are debugging on a Xbox360 and your general purpose allocator uses 64kb memory pages internally.

Heap-Allocations will therefore almost always reside in a 0x4xxxxxxx address range. They could also go to 0x5xxxxxxx addresses, but only if you are using more than 256MB for your general purpose heap.

As the stack is also allocated from 64kb pages and grows downwards, you will find the stack in the 0x7xxxxxxx area.

Last but not least, the PE loader uploads code to the 0x80000000 – 0xA0000000 area.

So, now you already have a pretty clear picture of what is going on by just looking at the addresses.

```
0x4xxxxxxx - 0x5xxxxxxx	    heap objects
0x7xxxxxxx                  stack
0x8xxxxxxx - 0xAxxxxxxx     code
```

Normally, your allocator aligns the heap allocations to 8 or 16 byte boundaries. So, another criteria if you are looking for objects on the heap: Ignore unaligned addresses.

So, with this information in mind, let’s take a look on the register window from the last page.

{{< figure src="/images/dbgoptppc_4.png" width="600">}}

You can clearly see, that r14 and r23 are most probably candidates for heap-allocated objects, while r13 points to the area where the code resides.

If you can expect that the heap-objects you are looking for are having a virtual function table, just cast the address from r14 and r23 to any polymorphic type. That’s what the debugger would show you:

{{< figure src="/images/dbgoptppc_5.png" width="500">}}
{{< figure src="/images/dbgoptppc_6.png" width="500">}}

Now, you can use these objects to find out further information about their state at the moment of the crash.

### Stack

The same works for the stack-frame of course. You can open up a memory window and display the memory at r1. This gives you the data, that is stored on the stack.

If you work with the Memory-Window, make sure you change the view to “4-byte integer”
and “hexadecimal” display. Than you can just apply your knowledge of sane addresses, and look
there for helpful objects.

{{< figure src="/images/dbgoptppc_7.png" width="600">}}

As you can see, there are some candidates in this stack-frame. Of course not every address that fits this pattern will contain a valid object, but most of the time you will find something that brings you a step further to the reason of your crash.

### Conclusion

It is not really hard to get some decent information without a debug build at hand. These are just a collection of simple tricks to get some data without the need to read assembly. If you do not have problems with this, you will have a lot of easier and more reliable ways to get the information you need.
