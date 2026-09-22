---
layout: post
title: The simulation engine is now about twice as fast
date: 2026-09-21
tags:
  - post
  - Performance
---

The big news is that, building on top of some initial performance improvements released last week, now all of the ZenGM games run about twice as fast.

Specifically, on my computer, I tested the amount of time it takes to auto play 10 seasons in a new league, and here is how it changed:

- Basketball: 202 seconds → 109 seconds (1.84x as fast)
- Football: 275 seconds → 139 seconds (1.98x as fast)
- Baseball: 583 seconds → 311 seconds (1.88x as fast)
- Hockey: 355 seconds → 179 seconds (1.98x as fast)

Go check it out and see for yourself! Or read on for some technical details.

<!--more-->

If you are reading on for technical details, you may already be aware that ZenGM is not quite open source but [the source code is available for some purposes](https://github.com/zengm-games/zengm/blob/master/LICENSE.md) and although [I wrote almost all of it](https://github.com/zengm-games/zengm/graphs/contributors?all=1), sometimes other people do make notable contributions.

Over the past couple weeks I got [a few PRs from sync0516](https://github.com/zengm-games/zengm/pulls?q=is%3Apr+author%3Async0516) focused on improving performance. That is a great way to contribute to a project because if you're like adding a new feature or something there can be debate about how exactly the feature should work. But if you're just taking the existing code and making it run faster, that is an unambiguous win.

I am not surprised that there were some performance issues with my code. In the past I have put some effort into performance, but I never spent a ton of time actually profiling the game, but clearly sync0516 did. Most of the changes in his PRs are fairly common sense stuff, like I looked at a change and thought "oh of course that will be a little faster", but if you do that enough it adds up!

sync0516 also was very clearly using AI to assist with this work, based on the text in the PRs and comments, as well as the nature of the work. Scanning over tons of code is difficult for a human because it gets boring after a while, but less difficult for an AI that will just run until it's done; indeed some of the performance improvements were in parts of the codebase I hadn't looked at in years, and I might have noticed these issues myself if I had painstakingly looked at every line of code again.

These performance improvements are the largest use of AI coding in the ZenGM codebase so far, since I personally hardly use AI at all. That position is getting quite difficult to justify from a productivity standpoint, although productivity isn't the only thing that matters.

Regardless, you can't argue with the results. So now all the games are twice as fast!

The AI's code was generally good and well tested, although sometimes it was overcomplicated or unnecessary (like proposing a change intended to make it faster that does not actually make it faster). I addressed those through careful testing, but it's probably true that I could have just merged the AI PRs and saved myself those hours of hard work and the difference would not have been noticeable to any end user. This is a big debate in software engineering now, whether it's even worth it to look at code or if you should just trust the AI.

In many ways I'm sure the AI is a better programmer than me. While reviewing the AI's code, I also caught the spirit and made some additional performance improvements, which were meaningful although not as large as the AI's. I suspect that if any bugs were introduced in this release, they are probably from my code.

And while these performance improvements are great, more broadly I still am very concerned that [we are on a very dangerous path with AI](https://ifanyonebuildsit.com/).
