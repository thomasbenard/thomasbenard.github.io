---
layout: default
title: A simple dependency breaking technique
date: 2017-05-27 10:33:47.000000000 +02:00
published: true
categories: []
tags: []
excerpt: Unit testing is hard. Particularly when working with legacy code. Here is one simple refactoring technique for breaking dependencies.
---

# {{ page.title }}

Unit testing is hard. Particularly when working with legacy code.  
Very often, the code we need to work on is highly coupled with libraries, frameworks or even other parts of our own code, which makes it virtually untestable without first refactoring it.  
Here is one simple refactoring technique for breaking such dependencies.

## What's the issue ?

Congratulations ! You just started working at SomeCompany™ and your first task is to make some modification in SomeClass, in the method computeSomeValue. Naturally, because you are a conscientious professional, you want to make sure you don't break anything. So you decide to write a couple of unit tests for this function.  
The only problem is: this function needs to call some external resource in order to fetch some value (maybe it retrieves it from a database, from a call to a webservice or even from some hardware, it doesn't really matter).  
The function looks like this:

```java
public int computeSomeValue(int foo) { 
  int someValue = SomeAPI.fetchSomeValue();
  if(someValue >= foo)
    return someValue * foo;
  return someValue;
}
```

Now, if you try to write a unit test for this function, you will soon realize that you can't call it without making this webservice call as well ! We say that the function is tightly coupled with SomeAPI.

## The solution: break dependencies

So, how do we get rid of this dependency towards SomeAPI ? Mmmmmh... Easier said than done !

Let's think for a minute. What do we want to do ?  
This call to `SomeAPI.fetchSomeValue()` is problematic. We don't want to do it in the unit tests but we still want to call it when we run this code in production.  
To sum it up, we want our code to behave in a certain way in production and in another way during the tests. Well, there's a name just for that: Pophynormism ? No, that's not it. Polyphormism ? No, neither is that. Polymorphism ! Yes, that's it !

Now, in a language like Java, how can we have polymorphism ? We need to use inheritance ! So what do we need to do...

We need this function to polymorphically (I hope I spelled that right !) call SomeAPI in production and some fake -just for the tests- code during the tests.  
For that, we first need to encapsulate the call to SomeAPI into a class:

```java
public class SomeApiAdapter {
  public int fetchSomeValue() {
    return SomeAPI.fetchSomeValue();
  }
}
```

Now, the code becomes like this:

```java
public int computeSomeValue(int foo) {
  SomeApiAdapter someApi = new SomeApiAdapter();
  int someValue = someApi.fetchSomeValue();
  if(someValue >= foo)
    return someValue * foo;
  return someValue;
}
```

We are not done yet, but let's take a minute to think about this step. Have you noticed the name of the class we just created ? We called it `Adapter`. Does it ring a bell ? It should. This is the [Adapter Design Pattern](https://en.wikipedia.org/wiki/Adapter_pattern) ! If you don't know what it is, I advice you take a few minutes off this wonderful article you currently have in front of you and go check what this is about.

Now that you know what an Adapter is, let's put our focus back onto polymorphism. What do we actually want in practice ? We need our `someApi` object to be an instance of `SomeApiAdapter` in production and something else in test. So, we need to introduce an interface:

```java
public interface ISomeApi {
  public int fetchSomeValue();
}

public class SomeApiAdapter implements ISomeApi {
  public int fetchSomeValue() {
    return SomeAPI.fetchSomeValue();
  }
}
```

```java
public int computeSomeValue(int foo) {
  SomeApi someApi = new SomeApiAdapter();
  int someValue = someApi.fetchSomeValue();
  if(someValue >= foo)
    return someValue * foo;
  return someValue;
}
```

We're getting close ! We have inheritance, but still no polymorphism because the instanciation of our `someApi` object is still in the `computeSomeValue` function. We need to move it away, like for example at the instanciation of the class:

```java
public class SomeClass {

  public SomeClass(SomeApi theApiWeWantToUse) {
    someApi = theApiWeWantToUse;
  }

  public int computeSomeValue(int foo) {
    int someValue = someApi.fetchSomeValue();
    if(someValue >= foo)
      return someValue * foo;
    return someValue;
  }

  SomeApi someApi;
}
```

Aaaaand... We're done ! We now have the power to have a different behavior in production than in the tests.  
We just have to write a specific `FakeSomeApi` that we can use during our tests. It could look something like that:

```java
public class FakeSomeApiThatAlwaysReturnsTheSameValue implements SomeApi {

   public FakeSomeApiThatAlwaysReturnsTheSameValue(int value) {
      this.value = value;
   }

   @Override
   public int fetchSomeValue() {
      return value;
   }

   private int value;
}

public class FakeSomeApiThatAlwaysThrowsAnException implements SomeApi {
   @Override
   public int fetchSomeValue() {
      throw new SomeApiException();
   }
}
```

With this, we can now write unit tests like so:

```java
@Test
public void should_multiply_when_someValue_is_greater_than_input() {
   SomeClass someObject = new SomeClass(
         new FakeSomeApiThatAlwaysReturnsTheSameValue(3));
   assertEquals(6, someObject.computeSomeValue(2)); //SUCCESS !
}

@Test
public void should_return_0_when_exception_is_thrown_by_SomeApi{
   SomeClass someObject = new SomeClass(
         new FakeSomeApiThatAlwaysThrowsAnException());
   assertEquals(0, someObject.computeSomeValue(42)); //FAIL !
}
```

## The Dependency Inversion Principle (DIP)

Indeed, this technique is the application of the [DIP](https://drive.google.com/file/d/0BwhCYaYDn8EgMjdlMWIzNGUtZTQ0NC00ZjQ5LTkwYzQtZjRhMDRlNTQ3ZGMz/view), the last (but not least !) of the SOLID principles. It states that:

> A. High-level modules should not depend on low-level modules. Both should depend on abstractions.  
> B. Abstractions should not depend on details. Details should depend on abstractions.

What this technique enables us to do is to remove the dependency between the code we are working on, in this case SomeClass, and some other code that we need to call (SomeApi) by simply introducing an interface between the two. And by doing so, it enables us to finally test our code.

But remember, this technique is NOT a silver bullet. It is based on a principle, NOT a golden rule. There are a lot of cases, actually most of the time, where it would be a mistake to apply it because [it doesn't come for free...](http://sam-koblenski.blogspot.fr/2014/07/the-cost-of-abstraction.html)
