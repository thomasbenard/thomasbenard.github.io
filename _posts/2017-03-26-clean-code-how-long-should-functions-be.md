---
layout: default
title: Clean Code - how long should functions be ?
date: 2017-03-26 15:48:11.000000000 +02:00
published: true
categories: []
tags: []
excerpt: We almost all agree that functions should be small. Big functions are hard to maintain, hard to test, hard to name, hard to call. They simply are hard to work with. But how small exactly ?
---
# {{ page.title }}

> The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.
> 
> Uncle Bob

We almost all agree that functions should be small. Big functions are hard to maintain, hard to test, hard to name, hard to call. They simply are hard to work with.

But how small exactly ? What does "small" mean ? When I started programming, I was told by my fellow co-workers that functions should not be more than one screen long, so that when working on a function I could see it in its entirety. I never really understood this rule: if someone in the team suddenly chooses to use a bigger font, does that mean that we have to re-write half of our functions ?

Later, I was told that functions should not be longer than 20 lines. This rule always bugged me as well. I always felt I could see 15 lines functions that were way too big and needed to be split.

## Functions should do one thing

![](/assets/rez-817x1024.jpg)

When I first read this line in the book Clean Code, it was like a divine ray of light just shined upon me: it is not a matter of length. Instead, what matters is how much your function does. If it does only one thing, it has a proper size, whatever the number of lines.

How do you know if a function does only one thing ? The answer is simple, but you are probably not going to like it: when you can't extract subfunctions from it. This is called "extract till you drop".

> If a function does only the steps that are one level of abstraction below the stated name of the function, then the function is doing one thing.

Here is an example extracted from [this article](https://sites.google.com/site/unclebobconsultingllc/one-thing-extract-till-you-drop). What does the `replace()`  
function do ?

```java
class SymbolReplacer { 
  protected String stringToReplace;
  protected List alreadyReplaced = new ArrayList();

  SymbolReplacer(String s) {
    this.stringToReplace = s;
  }

  String replace() {
    Pattern symbolPattern = Pattern.compile("\$([a-zA-Z]\w*)");
    Matcher symbolMatcher = symbolPattern.matcher(stringToReplace);
    while (symbolMatcher.find()) {
      String symbolName = symbolMatcher.group(1);
      if (getSymbol(symbolName) != null && !alreadyReplaced.contains(symbolName)) {
        alreadyReplaced.add(symbolName);
        stringToReplace = stringToReplace.replace("$" + symbolName, translate(symbolName));
      }
    }
    return stringToReplace;
  }

  protected String translate(String symbolName) {
    return getSymbol(symbolName);
  }
}
```

Of course, you can understand what it does, but it takes you some time because it is not completly obvious. Now, take a look at the refactored version of this code:

```java
class SymbolReplacer {
  protected String stringToReplace;
  protected List alreadyReplaced = new ArrayList();
  private Matcher symbolMatcher;
  private final Pattern symbolPattern = Pattern.compile("\$([a-zA-Z]\w*)");

  SymbolReplacer(String s) {
    this.stringToReplace = s;
    symbolMatcher = symbolPattern.matcher(s);
  }

  String replace() {
    replaceAllSymbols();
    return stringToReplace;
  }

  private void replaceAllSymbols() {
    for (String symbolName = nextSymbol(); symbolName != null; symbolName = nextSymbol())
      replaceAllInstances(symbolName);
  }

  private String nextSymbol() {
    return symbolMatcher.find() ? symbolMatcher.group(1) : null;
  }

  private void replaceAllInstances(String symbolName) {
    if (shouldReplaceSymbol(symbolName))
      replaceSymbol(symbolName);
  }

  private boolean shouldReplaceSymbol(String symbolName) {
    return getSymbol(symbolName) != null && !alreadyReplaced.contains(symbolName);
  }

  private void replaceSymbol(String symbolName) {
    alreadyReplaced.add(symbolName);
    stringToReplace = stringToReplace.replace(
      symbolExpression(symbolName), 
      translate(symbolName));
  }

  private String symbolExpression(String symbolName) {
    return "$" + symbolName;
  }

  protected String translate(String symbolName) {
    return getSymbol(symbolName);
  }
}
```

(Note: you can find the whole refactoring process detailed in the linked article)

Isn't the code much easier to understand now ? Maybe you prefer the first one because everything is the same place. But think about the person who's seeing this code for the first time. There are good chances that he will prefer the second version of the code over the first one because it is so much easier to start working with it.

Let's imagine you need to change how the decision about whether we should replace a symbol is made. First the original version, it may need a little bit of inquiry if you are not familiar with the code. On the other hand, with the second version, it is very easy to see where the modification should be done: the code is gently telling you ! You simply have to change the `shouldReplaceSymbol(String symbolName)` function !

As you may have noticed, the largest function in this example is 4 lines long. And this is normal. If you start consistently writing functions that do only thing, you will soon realize that your code will be littered with functions that are 5 to 6 lines long max.

## Am I not going to be lost with all these functions ?

This may be what's going through your head right now. Well, if it is, I have bad news for you: you are simply used to work with bad code.

The reason you might think this is that you assume you can't know what a function does without reading its code. And therefore, splitting your functions into smaller functions is going to make things worse !

But here is the thing, we are not only splitting big functions, we are giving names to the newly created ones. Good, descriptive names. When you use a function from the standard library of your favorite language, does it bother you that you don't know its implementation details ? I guess it doesn't ! The name of that function describes what it does pretty clearly. What we are actually interested in is what the function does, not how it does it.

If you name your functions right, there is absolutly no reason for you to get lost within your own code.

## Functions that do one thing are easier to name

Now you might be thinking "Alright, writing functions that do only one thing will make the code clearer. But only if can name all those functions well ! Naming is hard and often, finding a descriptive name for a function is close to impossible !"

It's true that naming is one of the most complicated things to do when coding. But in this case, it is far easier than you might think.

When we have trouble to find a name that describes a function nicely, it is because we have a hard time getting our head around what it does. On the contrary, if it does only one thing, naming this function becomes much easier.

The corollary is that if you have a hard time naming a function, it may be the sign that it is doing too much !

## Classes hide in large functions

Sometimes you can come across functions that are so large and do so much stuff that there can be one or many classes hiding in there.

Let's imagine you need to change such a function. Because you are a professional and you apply the [Boy Scout Rule](http://www.informit.com/articles/article.aspx?p=1235624&seqNum=6), you start refactoring the code by extracting function after function. After some time, you realize that you class has grown quite large with a lot of functions and that some of those don't seem to belong within the same class. That's a [Single Responsibility Principle](https://drive.google.com/file/d/0ByOwmqah_nuGNHEtcU5OekdDMkk/view) violation ! By splitting the class in two, you will greatly improve the design of your application.

And as a bonus, you have made this whole code a lot easier to unit test ! Which will make your level of confidence in your code rise tremendously !

## I'm still not convinced

Well, in this case, here is my final argument: just try it. This idea has been supported for years by people like Uncle Bob and [Martin Fowler](https://martinfowler.com/bliki/FunctionLength.html), people that brought so much to the world of programmers.

Why don't you honestly give it a try for a few weeks ? If you don't like it, you can still revert back to your old ways later.

