---
title: "Learn a New Programming Language Every Month with Exercism's #12in23 Challenge"
datePublished: Wed Jan 18 2023 02:31:03 GMT+0000 (Coordinated Universal Time)
cuid: cld11rvxv000008l84tbl3n5h
slug: learn-a-new-programming-language-every-month-with-exercisms-12in23-challenge
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680706146728/bbe3c9ab-ec78-4541-91c0-37d74850ec68.png
tags: challenge, java, android, kotlin

---

Starting in January of 2023, I decided to take Exercism's #12in23 Challenge.

Exercism is a site for learning programming languages and exercising your skills with them. In 2023, they launched the [#12in23 Challenge](https://exercism.org/challenges/12in23). In their own words:

> The idea is simple: each month, you'll focus on learning a new programming language. We'll provide resources and support to help you along the way, and you can track your progress and connect with other participants on our online community.

Digging in a little deeper, I learned that the challenge involves solving at least 5 exercises for each language you chose. If you do that for 12 different languages during 2023, you complete the challenge.

**Note that I will not be posting my full solutions here.** I believe that goes against the spirit of the site and the challenge. Instead, I'll give you my experience and some of the techniques I used to find the solutions.

## Choosing a language

For an added bit of fun, I decided to code up a quick little [web app that chooses a language for me at random](https://codepen.io/travishorn/pen/GRBNvRQ). You can use it, too, if you'd like.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673046595842/bb46c152-9262-4d29-81ff-b43331436711.gif align="center")

For January, I did [Kotlin](https://kotlinlang.org/)!

Kotlin has some of its roots in Java. In fact, you can use Java alongside Kotlin code. JetBrains came up with the Kotlin language to modernize Android development and it has seen strong growth since its creation.

## Setup

The setup instructions on [Exercism](https://exercism.org/) are surprisingly easy to understand and follow. First, I had to download and install the Exercism CLI. Once that was installed, I downloaded the first exercise.

```bash
exercism download --exercise=hello-world --track=kotlin
```

That command downloaded all the project files for the exercise, including a tutorial document. The tutorial contained information about getting the Kotlin development environment set up, including installing Chocolatey (I'm using Windows) and the Java SDK. The whole process took less than 10 minutes.

## "Hello, World!"

With everything set up, I ran the included test.

```bash
.\gradlew test
```

I haven't coded my solution yet so obviously, the test failed.

```bash
org.junit.ComparisonFailure: expected:<[Hello, World]!> but was:<[Goodbye, Mars]!>
```

The tutorial tells us how to solve this simple exercise in `HelloWorld.kt`

```kotlin
fun hello(): String {
   return "Hello, World!"
}
```

I saved the file and ran the test again. This time everything passed! Now to submit my solution.

```bash
exercism submit src/main/kotlin/HelloWorld.kt
```

Back on the Exercism site, I marked the exercise complete and gained access to all of the "real" exercises.

## The real exercises

After getting the Kotlin development environment set up and completing the "Hello, World!" exercise, I dove into the real exercises. I have to complete at least 5 of these to check Kotlin off my list.

### Two Fer

First, I tried the exercise called **Two Fer**. Given a name, return a string with the message:

> One for **name**, one for me.

If no name is given, return:

> One for **you**, one for me.

Again, I think posting the solutions here goes against the spirit of the site and the challenge, so I won't do that. I will say that searching the internet for ["Kotlin string interpolation"](https://duckduckgo.com/?t=ffab&q=Kotlin+string+interpolation&ia=web) and ["Kotlin function parameter default"](https://duckduckgo.com/?t=ffab&q=Kotlin+function+parameter+default&ia=web) got me the solution pretty quickly!

### Hamming

The backstory of this exercise involves DNA sequencing. But it boils down to counting the number of differences in characters between two strings. So the hamming distance between these two strings:

```bash
GAGCCTACTAACGGGAT
GAGCCTACTAACGGGAT
```

is **0** because they are identical. While the hamming distance between these two strings:

```bash
GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT
^ ^ ^  ^ ^    ^^
```

is **7** because there are 7 differences when comparing each character in order.

In addition, the exercise wants us to throw an `IllegalArgumentException` when the strings are of different lengths.

I first searched ["Kotlin if else"](https://duckduckgo.com/?t=ffab&q=Kotlin+if+else) to figure out how to do conditional logic. Then ["Kotlin throw IllegalArgumentException"](https://duckduckgo.com/?t=ffab&q=Kotlin+throw+IllegalArgumentException&ia=web) to figure out throwing the exception. The first line of my solution just throws the exception if the strings are of different lengths.

Next, I added logic to return 0 immediately if the strings are identical. String comparison in Kotlin works the same as other languages I'm familiar with so that was easy.

Finally, if the strings are the same length and they are not identical, I had to do the actual comparison. My first thought was splitting the strings into lists of single characters, looping through the first string, and comparing each character to the same index in the second string. But then I figured Kotlin probably had a way to "zip" two strings (similar to other languages I've used). After searching, I found documentation on [Kotlin's `zip` method](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/zip.html).

With the strings zipped into a list of pairs of characters, all I needed to do was [count](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html) the pairs that weren't identical.

### Gigasecond

The description of this exercise was very simple: Given a date, return what the date would be 1,000,000,000 seconds have passed.

This seemed so simple once I found [`.plusSeconds()`](https://developer.android.com/reference/kotlin/java/time/LocalDateTime#plusseconds). But then I ran into trouble when the tests required that my class support both `LocalDate` **and** `LocalDateTime`.

Honestly, I was stumped. I tried interfaces, wrapper classes, and generics. I even tried defining my `Gigasecond` class twice with two different parameter options. None of it was working, until I grasped the concept of [primary and secondary constructors](https://kotlinlang.org/docs/classes.html#secondary-constructors).

In my solution, the primary constructor accepts a `LocalDateTime`. But I also added a secondary constructor that accepts a `LocalDate` and then uses [`.atStartOfDay()`](https://developer.android.com/reference/kotlin/java/time/LocalDate#atstartofday) to "convert" the value to a `LocalDateTime`.

### Scrabble Score

This one was pretty fun. Given a word, return its score in [the game of Scrabble](https://en.wikipedia.org/wiki/Scrabble). The exercise provides a mapping of letters to scores. For example A, E, I, O, U, L, N, R, S, and T all equal **1**. The letters D and G equal **2**. There is a score associated with each letter of the English alphabet, but I won't paste them all here as it's not particularly relevant.

I researched how to use [the `when` expression](https://kotlinlang.org/docs/control-flow.html#when-expression) in Kotlin to make a function that would return a specific `Int` given a specific `Char`.

Then, I used [`.fold()`](https://kotlinlang.org/docs/collection-aggregate.html#fold-and-reduce) to reduce a `String` (representing the given word) to an `Int` (representing its score). I love using aggregate operations like `.fold()` to concisely reduce data without having to code big loops.

### Difference of Square

This fifth and final exercise involved a little bit of math and not much else. I had to create:

* a function that would **square** the **sum** of a list of numbers from 1 to n
    
* a function that would **sum** the **square** of that same list
    
* a function that would return the **difference** between those first two functions
    

First, I figured out how Kotlin handles number ranges: `(1..10)` so I could use `(1..n)` where `n` was the given integer. From there, it was just researching some built-in math functions and aggregate functions like [`kotlin.math.pow()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.math/pow.html) and [`.sum()`](https://stackoverflow.com/a/52851213). The only "gotcha" that I ran into was that `kotlin.math.pow()` expects a double. So I just had to convert to double first, perform the `.pow(2)` (to square the number), then convert back to integer.

To code my solution, I simply had to put those operations in the right order to create a `squareOfSum()` and a `sumOfSquares()` function. Finally, calculating the difference was the easiest part: just use the minus operator (`-`) to subtract the return value of `sumOfSquares()` from `squareOfSum()`.

## One month down

With those five exercises in Kotlin complete, I'm finished with January! Eleven more months and eleven more languages to go.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673111871336/39573856-8d4a-4a83-a148-1ec38fc1315d.png align="center")

Kotlin was an interesting experience. I like how it's similar to Java so a lot of the knowledge I gained is applicable in that language if I ever need to use it, too. I'm also interested in how Kotlin can be used for mobile app development. While Exercism doesn't focus on higher-level concepts like deployment, I could take some of the low-level skills I learn and implement them into bigger projects that could eventually be deployed as mobile apps.