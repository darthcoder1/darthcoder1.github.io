+++
title = 'Stupid Quoting Is the Root of All Evil'
date = 2011-07-15T13:25:51+02:00
draft = false
+++

# Stupid Quoting is the root of all evil

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

If I would have received a beer for every time I hear (or read) someone quoting Knuth’s “Premature optimization is the root of all evil”, I would have long ago died with cirrhosis of the liver … twice.

<!--more-->

### Why does this quote drive me so mad?

First of all, no one I have ever met, who was using this quote to back up his stupid point has even read the paper this quote originates from.

[Structured Programming with Goto Statements](https://pic.plover.com/knuth-GOTO.pdf)

Yeah, really, that was the title. And the entire article was about optimizing the shit out of stuff.
Why is no one quoting the title of this paper? Maybe I should do this whenever someone claims how evil ‘goto’ is.

Sorry, that you are not able to use the available tools without screwing your code base and falling into spaghetti-mode, moron! Did you ever heard a carpenter saying: “Dude, I don’t use saws. That is fricking dangerous. I could hurt myself.”?

Goto is as evil as virtual when you give it into the wrong hands.

<!--more-->

But that is not the point … I’m getting sidetracked 🙂

### Never ignore performance considerations

To be clear: I know the importance of profiling to identify your bottlenecks and your critical path. I would never argue against that. Optimize only where your profiler tells you, it makes sense.

But that does not mean that you can give a crap about the rest of the code. Keep one thing in mind: There is no non-performance critical code in a game, ever. None. You don’t need to optimize the hell out of everything, but you need to think about performance implications of your code in every single case. There should never be an exception to this.

## When you start to don’t care, Baby Jesus will hate you.

What this gives you in the end, is a bit too much cost for almost everything that is going on. You are wasting your time in trivial things all around your code base, but you are not able to nail it down and to optimize it properly, as it is spread everywhere. And every single optimization will give you almost non-measurable improvements. But the sheer amount of small inefficiencies sums up and costs you and considerable amount of execution time.

Unfortunately, when you are at this point, there is no chance of improving this ‘Death by a thousand papercuts’ situation anymore. You will not have the resources to spent precious programmer time on such minor improvements. It is just not enough bang for the buck.

Do not ignore performance considerations ever! This will bite you in the ass in the long run and you will have to suffer in other areas. In the worst case you will even be forced to scale down some features to meet the performance criteria. But for what?
Just for the fact, that you followed a totally outdated quote, that is used out of context and interpreted wrongly.

And stop quoting stuff you have no clue about.

Ironic as I am, I will finish this post with another quote from another awesome programmer 🙂

```
My point is, that you should fire anyone quoting anything from this paper without pointing out, that all this is obsolete, because compilers changed a lot since the age of dinosaurs ;-)
```
