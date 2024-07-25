+++
title = 'What You Need to Give Up When Going Data Oriented'
date = 2011-08-10T14:56:58+02:00
draft = false
+++

# 'What You Need to Give Up When Going Data Oriented

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

This post is not about the performance advantages of data oriented design, as this has been covered already pretty extensively by much smarter guys. ( see links below )

What I want to talk about, are the prejudices that I always hear when people start to defend their holy objects.
Everyone and his mother is constantly reiterating the advantages of OOP – productivity increase, maintainability and code-reuseability. Should we really sacrifice all this good things just in favor of faster execution times?

What are the advantages of OOP everyone is so keen to protect?

I think this list should cover most of the claimed benefits:

- Encapsulation
- Inheritance
- Polymorphism
- Modularity
- Code-Reusability
- Elegance
- Extensibility

Let’s go through this list and take a look what we really need to sacrifice.

<!--more-->

### Encapsulation:

Hiding of implementation specific data and functionality. This is something you should aim for in any programming paradigm, no matter if you call it OOP, DOD or ADHS. This is definitely nothing you should give up. And you don’t need to. To claim that encapsulation is only working with OOP is just plainly wrong, as this has been done in C since the dawn of time. So, apparently this still applies to DOD and is nothing you need to give up.

### Inheritance:

The ability to inherit a class’ data and functionality to extend/alter it’s behavior..
Inheriting data is really not fitting well into the DOD concept. It obfuscates the way your data is organized and forces you to group the data by object, not by usage pattern. This is definitely a thing you would need to give up, at least to some extent.

### Polymorphism:

That is a really nice concept of OOP. It releases you from thinking about what happens, when you call a method on any given object. If you want to update your 10000 entities, you just iterate over a list and call update() each time. Everything is handled for you. Yeah, it is the most inefficient way you could handle this … but it works. It is convenient and it safes you a lot of headaches. Unfortunately, it is highly overused.
You do not really want that much of polymorphism if you design your code around your data. This does not mean there is no space left for it, but you have to decide whether is makes sense or not instead of using it as the default way of programming.

### Modularity:

You put all data and functionality into one parent entity … a class for example. The ‘class’ in this case is an arbitrary defined scope for a ‘module’. A module could as well be a library, a subfolder in your source-tree, a matching pair of header and implementation-files or even a block of code marked by some fancy comment ( maybe even including ASCII-Art ). So, why is modularity attributed to OOP. You do not have to give up any modularity if going away from an OOP model. In the end, a module is defined by a description of the data and the transforms that can be applied to it. Modularity is also perfectly possible and encouraged in data oriented programming.

### Code Reusability:

This should never be anything that drives your implementation decision. But apart from this, code-reuse is achieved by calling a function, right? Is it ‘better’ code-reuse if a class provides you the ’reusable’ functions to call? “But you can reuse entire objects, you $%*§$!”, I hear you saying. Can you? How often – in a real world application – have you reused an object to do something you haven’t had already in mind when writing this class originally. I think there aren’t that much occurrences, apart from the obvious cases. There is nothing that stops you from reusing your non-OOP code. Just because it is not modeled after an object does not mean that it cannot be used elsewhere. You can reuse as much code as it makes sense, so nothing to give up here.

### Elegance:

What the hell is this supposed to mean? What is elegance in code? Is it achieved by modelling your code base after small chunks of code Erich G. & Friends have taught you?. Is it elegant, if you are layering one abstraction over another? The definition of elegant code is a bit subjective, so I find it hard to use this to support any programming model.
My personal definition: “Elegant code does the job it is supposed to do ( and nothing more ) in an efficient way and can be understood by you or some other coder 6 month later without jumping through 37 files.”
So, to reverse this argument – and to clarify how subjective ‘elegance’ is – object-oriented design encourages programmers to write non-elegant code according to my definition ( which for sure is the only correct one 😉 ).

### Extensibility:

I prefer to see extensibility of code pretty tight to my definition of elegance. If you are able to modify any given code and can understand it and the possible side effects, without the need to understand the 33 abstraction-layers underneath you, you have pretty extensible code.
The requirement is not to have a system in place that gives you the possibility to derive some classes and re-implement some functions. The requirement is, that you are able – in a short timeframe – to extend the code by the needed functionality without causing hazard some thousand lines away.

### Conclusion:

If you look back – at my highly biased post – there is not really a lot you are giving up in favor of faster execution times. But the most important advantage of data oriented design is the fact, that you are discouraged to over-engineer. You are not writing code for the sake of creating a code-temple for your ego, but you are writing code to perform an operation on your data-set. So, by starting being awesome and concentrating on your data, you are elevating your code to new levels of readability, maintainability and extensibility … and you get faster execution time as a nice side-effect. Do not forget the street cred you get from doing the right thing …

### Further reading:

- [Pitfalls of Object Oriented Programming](https://harmful.cat-v.org/software/OO_programming/_pdf/Pitfalls_of_Object_Oriented_Programming_GCAP_09.pdf)
- [Typical C++ Bullshit](https://macton.smugmug.com/Other/2008-07-15-by-Eye-Fi/n-xmKDH#593426709_ZX4pZ)
- [Practical Examples in Data Oriented Design](https://www.gamedevs.org/uploads/practical-examples-in-data-oriented-design.pdf)
