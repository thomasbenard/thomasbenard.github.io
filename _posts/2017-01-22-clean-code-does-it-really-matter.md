---
layout: default
title: Clean Code - Does it really matter ?
date: 2017-01-22 15:30:25.000000000 +01:00
published: true
status: publish
categories: []
tags: []
excerpt: Have you ever met code that was so opaque that it took you hours to understand what a single function was doing ? If you have any kind of experience programming, chances are that you did.
---

# {{ page.title }}
_{{ page.date | date: "%-d %B %Y" }}_

{{ page.excerpt }}

I don't even need to give you an example. You know exactly what I'm talking about. In fact, you even probably have the example of the last time you met such a bad code crossing your mind right now. You might even have started to crind your teeth.

Such code not only is hard to understand, but it also makes us waste a lot of time because of that, it slows down the development of the project. In fact, when too much bad code is written, the survival of whole project, or even the company, is endangered.

In other words, that code is crap. It is dirty.

## What is Clean Code ?

Clean code is just the opposite of that.

Clean code is code that focuses on readability and maintability. It is easy to understand, unsurprising. Grady Booch, author of many programming books including the famous and highly recommended [Object-Oriented Analysis and Design with Applications](https://www.amazon.com/Object-Oriented-Analysis-Design-Applications-3rd/dp/020189551X) even said "Clean code reads like well-written prose".

Now, if this wasn't enough to convince you of the importance of Clean Code, or if you need more arguments to convince people in your workplace, it's time to get the big guns.

### This code is so John-like !

What do you do when you see clearly bad code ? We almost all do the same thing: we hit the blame button. We want to know who the hell wrote that kind of crap.

Have you ever been in a team where someone's reputation for putting together crappy code was so bad that their name was used to describe bad code (John is this case) ?

Think about it, even years after you leave a project, your code is still in the codebase. People that you never met, people that don't know the reasons you might have had at the moment (whatever they may be), will see you as a bad, unprofessional,  programmer. And you know, yourself carry in your head a list of such people: "If I'm asked to work with this John guy, it's time to run away."

So, don't be like John, don't write poor code. Write good, clean code. Take care of your reputation and work with pride.

## How much time do you spend reading code over writing it ?

This is a tough question. Try to estimate the ratio before reading further.

Ready ?

"_Indeed, the ratio of time spent reading versus writing is well over 10 to 1\. We are constantly reading old code as part of the effort to write new code._"

Robert C. Martin (aka Uncle Bob) in [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

This means that we actually spend more than 90% of our time coding reading code rather that writing. And because we spend so much time doing it, writing readable code is one of the biggest (if not the biggest) time savers you can focus on.

## Code is for humans

Have you ever heard something along those lines : "Spending time writing clean code is a waste because it makes no difference to the computer" ?

Right. Then why do we even bother writing code in Java, C#, C or whatever... Why do we not write our code in assembly or even binary code directly ? If it makes no difference whether we write good or bad code, why do we never write in the language that is directly understandable the machine itself ?

Remember, indeed, the quality of your code does not make any difference for the machine. But code is not for the machine. Code is for the people who will work on your program. Code is for humans, not machines.

## Do you prefer working on greenfield projects or doing maintenance ?

Don't say it ! I'm going to read your mind.

Huuuuuuuuuum...

I see the letter 'G'...

Yes ! That's it ! You prefer working on greenfield projects. Not a big trick however, most of us do. Why ? You probably guessed it: maintenance implies working with someone else's crappy code.

There are a lot of projects out there that are so poorly written, that are so painful to work on, that programmers have been pushing for years for the code to be completly re-written from scratch. This is however rarely a good idea because the underlying issue, programmers writing bad code, has not been solved. And a few years after starting to work on this new project, we start begging for a complete re-write again...

Save your future self some troubles and don't write bad code, write clean code.

## Writing code is easy, reading code is hard

Almost anyone can write code that works. By simply testing and fiddling with the code until you get there, you can get your code to work almost everytime.

Reading code however is much more complicated. It means entering into someone else's head, figuring out how the person who wrote the code thought about the problem he was trying to solve.

This can be much MUCH harder to do than simply writing code. Particularly if the code is messy. And for every person that will have to work on such code, more time will be lost. On the long run, the difference in output productivity is enormous.

In order to save time, we need to write code that can be easily understood by our coworkers (end even our future selves), that is clean code. As Donald Knuth put it : "_Programming is the art of telling another human what one wants the computer to do._" This implies that to write code that is easily understood by others, you need to get your ideas very clear in your head first.

We have barely scratched the subject of clean code here. In a future article, I will try to give you some tips about what to do in order to make you code cleaner and easier to work with.

Clean Code is also a [book](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) written by Robert C. Martin. I can't urge you enough to go read it (you can even find a pdf version for free if you google it).

