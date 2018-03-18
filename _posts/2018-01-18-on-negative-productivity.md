---
layout: default
title: "Can programmers have negative productivity ?"
date: 2018-01-18 07:36:22.000000000 +01:00
published: true
categories: []
tags: []
excerpt: All to often, we think we have moved a little bit farther every time we move a sticky note to the "Done" column. And what if we had actually moved backward ?
---

# {{ page.title }}
_{{ page.date | date: "%-d %B %Y" }}_

> "Time spent on cleaning the code is wasted ! We could instead spend this time working on some user stories, let's be productive !"

{{ page.excerpt }}

> "I have one more feature, how the hell could I have moved backward ?"

Well, let's take an example. Imagine you, along with four other programmers,  are working on an amazing new project. There are 1000 hours of work left to do in order to finish it (yes, I know [it makes no sense](https://www.infoq.com/articles/kelly-beyond-projects), but bear with me for a minute). That's a total 5 of weeks of work for a team of 5 people.

You have just finished a full day of work, 8 hours ! And therefore, everyone would expect there are now only 1000 - 8 = 992 hours of work left to do.

But unfortunately, you were a little bit tired today and ended up making a little bit of mess in the code which makes it a little bit harder to understand and that will end up slowing everyone down by 1%. No one is going to notice this 1% drop in productivity. And since no one will fix it because time spent not producing anything is wasted, it will remain forever in the code base. With this in mind, amount of work that is really left is: (1000 - 8) / 0.99 = 1002 hours !

After a full day of work, you ended up with 2 more hours of work to do than what you started with ! It turns out that it would have been better if you were sick that day :D

Besides, in real life, your project will take much more than 5 weeks to complete and your code will outlast your project as it will be reused for other purposes, other projects whose productivity will also be hurt. Because of that, this little mess you made in the code ends up costing way WAY more than just 10 hours.

That's why writing clean code and continuous refactoring are such important practices. Without them, every time you make even a little bit of mess in the code, your objective moves a little bit farther away even though you deliver features.

So, the next time you have trouble finding a name for a variable, a function, a class or anything; don't give up after a few minutes to set for _data_, _manager_ or even _k_. Even spending a full day with two or more people looking for a good name is worth it.

That's long-term negative productivity: you input work on your project and end up with more. And in programming, there is a good chance that this will happen to you if you are not careful about what you do.
