---
layout: post
title:  "Keep your privates internal"
excerpt: "How being a little explicit about package visibility can be helpful"
---
`Java` default visibility modifier for its elements is `protected`. `Protected` means that those elements have package level visibility, which in turn means only elements in the same package are allowed to use the element marked with `protected` visibility modifier. This made sure that your `classes` present in a different package cannot accidently use `class` from a different package. But here walks in `Kotlin`, making the elements default visibility modifier `public`. Sounds like a small change, but its not. It’s a major change. A change which can cause havoc if you are not cautious with the way you are exposing your elements.First let me explain why `kotlin` did what `kotlin` did.

You see back in the days when `kotlin` was a new release, their default visibility modifier was indeed `protected`. Well it is actually called `internal`, but it does the same thing. Marking a element `internal` means only files from the same module can access this element marked with `internal`. Now that was a good java-to-kotlin mapping, as `kotlin` always try to present itself as “the shorthand Java”. But then something happened. `Kotlin` team at JetBrains started noticing a trend. They noticed that most of the users were ditching the `internal` modifier and switching it to `public` modifier. This trend has to be a major spike because later they decided that they are making a change where the default visibility modifier is now going to be `public`. Their reasoning behind this change was that they want `kotlin` to be a language which makes user type less for more. In order to do that and to not cause their customers this annoying switch from `internal` to `public`, they made `public` the new default. Now lets see this from the perspective of developers who are actually the customers and see how it harms us.

The biggest mistake that was ever made in the IT industry was the invention of `NULL`. Even the inventor of `NULL` has said on multiple occasions that it was his biggest mistake. On a side note, I personally do not let `NULL` creep into my projects. I rather make an "Empty Object”, which can be used instead of marking anything as `null`. Let’s get back on track though, that mistake was made by a person. Second biggest mistake in IT industry is unseen dependencies. These dependencies creep up in your code and makes change difficult. So it is the job of a developer to manage these dependencies and make sure only certain classes are exposed as part of a module’s public API as dependencies. This will at least make sure that there are certain number of exposures and make debugging faster. Now imagine if by default, you were given the feature where a class cannot be exposed by a different module, or the dependency cannot creep out of your module and then taken away. That has what happened in the case of `kotlin`. For example, let us say I make a `Validator` class in my Domain layer. Technically, this `Validator` class should only be used by the UseCase. But because `Kotlin` is `public` by default, I would have to remember explicitly to mark this `Validator` class `internal`. This explicit marking of class as `internal` is actually more annoying because you know that is how it should be by default. Before extending a class you have to explicitly mark it `open` in `kotlin`, so why not to use it in a different module as well. I have my doubts about why at one side we are enforcing something and on another we are easing things. Developers should be made aware of all their decisions, so if they mark a class `public` they know the dangers that come with it.