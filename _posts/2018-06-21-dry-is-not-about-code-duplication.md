---
layout: default
title: The DRY principle is NOT about code duplication
date: 2018-06-21 18:21:25.000000000 +01:00
published: true
categories: []
tags: []
excerpt: DRY is one of the most famous principles in the field of software programming. And yet, it is heavily misunderstood.
---

# {{ page.title }}
_{{ page.date | date: "%-d %B %Y" }}_

{{ page.excerpt }} We often think it is about removing code duplication.

![](/assets/you-are-wrong.jpg)

Let's take a look at what it actually means. 

## The Don't Repeat Yourself (DRY) principle

Enunciated by Andy Hunt and Dave Thomas in their oh-my-you-should-definitely-read-it book The Pragmatic Programmer, the DRY principle states that:
> Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

The DRY principle is actually about knowledge duplication. Not code duplication. It doesn't even mention code.

For example, it may mean that: 
- you can't have a specification document as well as acceptance tests.
They both serve the same purpose, they both hold the same information, they both say how the system should behave (and by the way, tests are doing a much better job at that).
- you should stop typing all those git commands when trying to commit something. Write a script for that.

It does not mean you should blindly remove all code duplication in your system.
Remove code duplication only when those bits of code actually represent the same thing, the same knowledge, the same rules.
Let's take an example. Consider the two following functions:

```java
public class UserDescription {
    // ...
    public bool isLongEnough() {
        String words[] = description.split(' ');
        int numberOfWords = words.length;
        return numberOfWords > 10;
    }
    // ...
}
```
Here the function `isLongEnough` checks that this UserDescription has at least 10 words.
```java
public class RemoteApiResponse {
    // ...
    public bool containsEnoughElements() {
        String elements[] = description.split(' ');
        int numberOfElements = elements.length;
        return numberOfElements > 10;
    }
    // ...
}
```  
While here, the function `containsEnoughElements` checks that the response received from a remote API has the minimum number of elements (10) to be considered viable.

Although these two functions have exactly the same code, it would probably be a terrible idea to remove the duplication.

If you did so, what would happen if the minimum number of words in the description of a user changes ? 
Or, if the remote API programmers decide to change how the response is formatted ? 
Then, you can't change one without changing the other.
Even if you can work around it, you still have to modify, recompile and redeploy the module containing the `UserDescription` class whenever the format of the response sent by the remote API changes.   

Indeed the code is duplicated, but both functions actually represent very different knowledge: in one case, it represents the validation rules of a user description while in the other case it holds the validation rules for the response of an API.

## Don't make your system more complex by removing too much duplication

Code duplication is bad. Well, most of the time. 
It can make your system hard to change.

But, as we just saw, removing too much duplication can make your system harder to change in the same way !
Let's not blindly remove code duplication anymore. 
We have to think before doing so.

We have to ask ourselves if those bits of code actually represent the same knowledge, if they implement the same rule, or if they are the same by pure coincidence.
