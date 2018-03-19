---
layout: default
title: "A few words about performance"
date: 2018-03-19 13:20:45.000000000 +01:00
published: false 
categories: []
tags: []
excerpt: Premature optimization is the root of all evil
---

# {{ page.title }}

> We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.
>
> Donald Knuth

Very often, we tend to put too much focus on the performance aspect of our code.
> If I remove this temporary variable, I can save a few cycles 

## Your biggest issue is maintanability, not speed

## Modern compilers

Modern compilers are very good at optimizing simple code, but not so much about complex code. Therefore, code that looks like it should be faster will often actually be slower than the simpler version.

## Mental masturbation

You should serve the customer, not yourself. If the customer wants speed, he will ask for it. What he won't ask for is for the system to be maintanable. Our focus on speed is for our own ego, not for the customer. We want to show off our brain power. 

## Use a profiler

We work with complex systems all day long, which means that we are terrible at predicting how they will behave. That means we should use tools like profilers 

