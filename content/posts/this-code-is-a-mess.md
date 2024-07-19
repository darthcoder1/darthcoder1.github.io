+++
title = 'This Code Is a Mess'
date = 2011-09-29T15:10:13+02:00
draft = true
+++

# This code is a mess. Let’s start from scratch again ...

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

I have heard this sentence a lot of times. And I even said it myself more than once.
It is pretty common that programmers want to have a clean and nice code base. They want
to be able to understand what is happening at the first glance and they want to have the
feeling that the code meets their quality expectations.

I also do.

But there is a serious problem which is often overlooked when we talk about ‘throwing away’ and starting from scratch.

### Where is this messy code coming from?

Code does not get ‘messy’ and war-torn by itself. It is also – normally – not the fault of some stupid programmer who has no clue what he is doing. I admit, this might happen from time to time, but I have never worked with anybody who could be attributed like that.

This are the two main reasons for code to become ‘messy’.

#### Bugfixes and the handling of corner-cases

There are lots of issues that are found and fixed during the lifetime of a code base. All the small and big fixes sum up to code, that is not really what one would call ‘clean’. But this does not mean, that the code is bad. I would even say it is exactly the opposite of bad. The functionality is tested, proven and able to handle real-world data thrown at it. This ‘messy’ code is your safe haven and you can rely on it doing what you would expect.

#### Design/Focus changes

The code was written under different base assumption, which are not valid any more. Design or focus changes forced a strong shift and required the code to adapt in ways that were not really fitting its original design. This leads pretty fast to code, which is hard to understand as a whole and therefore hard to maintain. The additional complexity introduced by this can even spread into the toolchain, which makes the also the life of the user of the system miserable.

### What to do with the mess?

The most important thing, is to realize, why the code is in the shape it is. It is crucial, that this is approached with the right mindset. You always should assume, that the implementation, the bugfixes and the extension of the code was done by someone, who had a clear picture of what is going on and a clear understanding of what needs to be done. It might sound obvious, but always assume the best knowledge and the best intent. Then you are able to judge the code objectively.

When you know in which state the code really is and when you understand all the interdependencies, you can make your decision on how to refactor it.

If there is no serious flaw in the design and it was not developed with different base assumptions and a different goal then what it has grown into, you should really think twice if you want to change it at all. Is there really a pressing reason to change it? Your decision should be not based on how much you like the code and how you judge the ‘elegance’ of the solution. The sole reason for the existence of the code is to deliver a specific functionality. And if this functionality is not suffering from the ‘ugliness’, don’t put it on stake. Accept the fact, that it might be not perfect, but it does the job. In the end, we are not writing code for the sake of writing code. We are building software. If the software functions properly, we did our job well. No one cares if there is code somewhere that does not adhere to the personal standards of a programmer, right?

Should you realize, that the code was developed with different requirements and was afterwards altered to somehow mirror the changes that happened to this requirements, the situation might be a bit more complicated. But even then, you need to keep in mind, that even this code is not necessarily shitty.

Whatever you think the right action is … throwing away the code is mostly the wrong one. We are always tempted to start from scratch, because we love to implement things and it is the most fun if you have a clear start. It is also by far easier to write new code, than to read old one.

But no matter how hard you try, you will be doomed to fix all the small bugs, issues and corner-cases later on again. All the things, that has been fixed for the existing code already need to be found again by the QA and fixed by you. There is no way you can fix all these issues on the fly, while re-implementing the functionality. Because of that, even a crappy implementation that was around for some time, has proven its right to exist and should therefore be refactored, rather than thrown away. You want to keep as much of the juice, that made the code do it’s job, as possible. And usually there is enough of it worth saving.

### Conclusion

The last motorcycle I had, was over 15 years old. It had a lot of small quirks, but I knew every single one of those. I knew how she behaved in every situation. I knew how to handle her when riding in different weather conditions. I could do the service while being drunk … with closed eyes.

The same is valid for old, ‘messy’ code. It is not beautiful, and has its scratches and it’s quirks. But you know them, and you know how to use the code to get your job done. Everything you need to be able to do, has been done already. You can rely on it, to do what you expect.

Do not throw away this intimate relationship just because of aesthetic reasons. The new one will also have it’s issues and problems, but you first need to find all of them and learn how to handle them.
