+++
title = 'Who Killed Anti Portals'
date = 2011-09-12T15:06:00+02:00
draft = true
+++

# Who killed Anti-Portals

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

Yesterday, I had a small chat with a former coworker that threw me back in time. It was about Anti-Portals.
Yeah, I know, you heard that term back in the days, but as it is so long ago ….

<!--more-->

### ... a small reminder what the hell an Anti-Portal is

An Anti-Portal is just a plane placed in the world, which shows you that everything behind this plane is not visible. To make use of it, you need to generate a plane that goes through the player’s point of view for every edge of the portal. You end up with a frustum that allows you to easily check if an object or scene-partitioning node is occluded or not. Normal Portals are working exactly the same, but they define the visible area instead of the occluded. They were used for doorways and such.

### What the hell has happened to Anti-Portals?

After that chat yesterday, I realized for the first time, that this technique disappeared silently and is dead by now. I haven’t heard of anyone using anti-portals since around 2004 or something. Why? Although we have other ways to handle occlusion nowadays, I cannot see what a few well placed anti-portals could harm. But I can see that they could be perfectly used to reject a bunch of objects with almost no cost. This does of course only make sense for large occluders, but hey, why not? You will almost always be able to find some of these.

### But now, let’s ask the most important question … who’s fault was it?

Who killed Portals / Anti-Portals? 🙂
If you have any clues that might help to find the murderer, please share them with us. Maybe we can catch that bastard before occlusion queries silently disappear as well.

### Why the hell am I writing this?

First and foremost, to commemorate the fallen ones and to find that reckless criminal. And second, because this topic remembered me instantly of a long forgotten ( at least by me ) internet site. [Flipcode](http://www.flipcode.com/). Although the site is down since 2005, they still have the archives there and it is still a lot of fun to read that stuff again.
