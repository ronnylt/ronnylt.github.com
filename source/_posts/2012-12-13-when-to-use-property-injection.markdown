---
layout: post
title: "When to use Property Injection"
date: 2012-12-13 13:35
comments: true
categories: 
---
Short answer: Try to avoid it.

There are several ways to inject dependencies when using the Symfony [Dependency Injection Component](https://github.com/symfony/DependencyInjection) (or any other DI component in general)
 
There are different ways of working and each type of injection has advantages and disadvantages to consider.

However, with the Symfony Component, one of the possibility is just setting the public properties of the class directly.
 
Although this method could be usefull in some situations, I consider Property Injection a *very bad practice* for the following reasons:

- You can not use type hinting. Therefore, you can not have control of what type of variable is being injected except by explicitly checking yourself in the class code before using the injected property.

- You can not control if the dependency is set at all, or if it is changed at any point in the object's lifetime.

- Public properties are mostly dangerous. Once you expose them, you lose any possibility of having control later. If you have gettes/setters right away you have room for adding control later.

It only may be may be useful when you are working with code that is out of your control, such as 3rd party libraries, legacy code, etc..., which uses public properties for its dependencies.

Conclusion, it´s good to know that this posiblity exists in the Service Container, but try to avoid it. Use constructor or setter injection whenever you can. Keep it simple, keep it robust ;)