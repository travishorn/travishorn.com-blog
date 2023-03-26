---
title: "#12in23 Challenge - February - Elixir"
datePublished: Wed Feb 08 2023 13:29:32 GMT+0000 (Coordinated Universal Time)
cuid: cldvpjlip000109l7bf6e8ebl
slug: 12in23-challenge-february-elixir
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1675616721449/4c74a47c-265c-46d9-a03c-6105ce4c529c.png
tags: challenge, elixir, functional-programming, 12in23

---

For the month of February 2023, I continued [Exercism's #12in23 Challenge](https://exercism.org/challenges/12in23) by trying out [the Elixir programming language](https://elixir-lang.org/).

If you're not familiar with this challenge, it's a programming-language learning challenge that tasks you with trying out 12 different languages in the year of 2023.

> The idea is simple: each month, you'll focus on learning a new programming language. We'll provide resources and support to help you along the way, and you can track your progress and connect with other participants on our online community.

Each month has a specific theme in mind. For February, the theme is Functional February. You can work toward the goal by completing exercises from Clojure, Elixir, Erlang, F#, Gleam, Haskell, OCaml, Scala, or SML.

I chose Elixir!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675610621281/5785552d-34c2-4601-b7b2-7f9fab04ac12.png align="center")

> Elixir is a dynamic, functional language for building scalable and maintainable applications.
> 
> Elixir runs on the Erlang VM, known for creating low-latency, distributed, and fault-tolerant systems. These capabilities and Elixir tooling allow developers to be productive in several domains, such as web development, embedded software, data pipelines, and multimedia processing, across a wide range of industries.

The nice part about choosing this functional language is that Exercism itself has a nice syllabus that you can follow to learn the language (they also have syllabuses for Clojure and F#). This way, I don't have to learn the language separately and then test myself on Exercism; I can do it all right from their site.

Note: In my last blog post, I mentioned that I will not be posting solutions to the exercises as I figured it goes against the spirit of the site. However, Exercism has promoted walkthroughs and live streams of the community solving the exercises. So, going forward, **I will be posting my solutions here**. If you want to avoid these "spoilers," I recommend trying the challenge yourself and then coming back here afterward.

When I did January's challenge with Kotlin, I had to set up my development environment locally. However, with some languages, including Elixir, you can code and test your solutions directly on Exercism!

I did it that way this time, but honestly, I might go ahead and set up a local development environment for each language going forward. I feel that it can give you a better idea of how each language works and what the developer experience is like. Plus, you can build your own applications if you have an idea and you like the language.

## Hello, World!

Starting at the beginning with a classic "Hello, World!" exercise. The in-browser editor gave me the following starting code:

```plaintext
defmodule HelloWorld do
  @doc """
  Simply returns "Hello, World!"
  """
  @spec hello :: String.t()
  def hello do
    "Goodbye, Mars!"
  end
end
```

Without even knowing any Elixir, I was able to solve this one simply by changing the string `"Goodbye, Mars!"` to `"Hello, World!"`.

But looking at the code closer can teach us a lot about how modules are defined, how documentation is a first-class citizen in Elixir, declaring typed function signatures/specifications, and more.

## Lasagna

Unfortunately for lazy programmers, the "Hello, World!" exercise doesn't count toward the required 5 exercises to complete the monthly challenge. So the first qualifying exercise I did was called "Lasagna."

We are given 5 tasks:

1. Define the `Lasagna.expected_minutes_in_oven/0` function that does not take any arguments and returns how many minutes the lasagna should be in the oven. According to the cooking book, the expected oven time in minutes is 40
    
2. Define the `Lasagna.remaining_minutes_in_oven/1` function that takes the actual minutes the lasagna has been in the oven as an argument and returns how many minutes the lasagna still has to remain in the oven, based on the expected oven time in minutes from the previous task
    
3. Define the `Lasagna.preparation_time_in_minutes/1` function that takes the number of layers you added to the lasagna as an argument and returns how many minutes you spent preparing the lasagna, assuming each layer takes you 2 minutes to prepare
    
4. Define the `Lasagna.total_time_in_minutes/2` function that takes two arguments: the first argument is the number of layers you added to the lasagna, and the second argument is the number of minutes the lasagna has been in the oven. The function should return how many minutes in total you've worked on cooking the lasagna, which is the sum of the preparation time in minutes, and the time in minutes the lasagna has spent in the oven at the moment
    
5. Define the `Lasagna.alarm/0` function that does not take any arguments and returns a message indicating that the lasagna is ready to eat
    

The `Lasagna.expected_minutes_in_oven/0` means that `Lasagna` is the module name, `expected_minutes_in_oven` is the function name, and `0` is the number of arguments the function accepts.

Since I had been following along with Exercism's Elixir syllabus (highly recommended), I already knew how to do function shorthand and basic math. My solution was simple.

```plaintext
defmodule Lasagna do
  def expected_minutes_in_oven, do: 40
  def remaining_minutes_in_oven(minutes_elapsed), do: expected_minutes_in_oven() - minutes_elapsed
  def preparation_time_in_minutes(layers), do: layers * 2
  def total_time_in_minutes(layers, minutes_elapsed), do: preparation_time_in_minutes(layers) + minutes_elapsed
  def alarm(), do: "Ding!"
end
```

`Lasagna.expected_minutes_in_oven/0` always returns the integer `40` .

`Lasagna.remaining_minutes_in_oven/1` takes the result from that first function and subtracts the number of minutes elapsed that was passed in as an argument.

`Lasagna.preparation_time_in_minutes/1` simply multiplies the passed-in number of layers by 2.

`Lasagna.total_time_in_minutes/2` uses that previous function to multiply the passed-in number of layers and then adds the passed-in number of minutes elapsed.

Finally, `Lasagna.alarm/0` always returns the string "Ding!"

## Pacman Rules

This exercise was all about using conditional logic. There are 4 tasks:

1. Define the `Rules.eat_ghost?/2` function that takes two arguments (if Pac-Man has a power pellet active and if Pac-Man is touching a ghost) and returns a boolean value if Pac-Man is able to eat the ghost. The function should return true only if Pac-Man has a power pellet active and is touching a ghost.
    
2. Define the `Rules.score?/2` function that takes two arguments (if Pac-Man is touching a power pellet and if Pac-Man is touching a dot) and returns a boolean value if Pac-Man scored. The function should return true if Pac-Man is touching a power pellet or a dot.
    
3. Define the `Rules.lose?/2` function that takes two arguments (if Pac-Man has a power pellet active and if Pac-Man is touching a ghost) and returns a boolean value if Pac-Man loses. The function should return true if Pac-Man is touching a ghost and does not have a power pellet active.
    
4. Define the `Rules.win?/3` function that takes three arguments (if Pac-Man has eaten all of the dots, if Pac-Man has a power pellet active, and if Pac-Man is touching a ghost) and returns a boolean value if Pac-Man wins. The function should return true if Pac-Man has eaten all of the dots and has not lost based on the arguments defined in part 3.
    

Again, since I was following the syllabus, I understood how the logic operators `and`, `or`, and `not` work. Once I understood how the Pacman rules were supposed to work, the solution was clear

```plaintext
defmodule Rules do
  def eat_ghost?(power_pellet_active, touching_ghost) do
    power_pellet_active and touching_ghost
  end

  def score?(touching_power_pellet, touching_dot) do
    touching_power_pellet or touching_dot
  end

  def lose?(power_pellet_active, touching_ghost) do
    not power_pellet_active and touching_ghost
  end

  def win?(has_eaten_all_dots, power_pellet_active, touching_ghost) do
    has_eaten_all_dots and not lose?(power_pellet_active, touching_ghost)
  end
end
```

`Rules.eat_ghost?/2` returns `true` only if there is a power pellet active and Pacman is touching a ghost.

`Rules.score?/2` returns `true` if either Pacman is touching a power pellet or he is touching a dot.

`Rules.lose?/2` returns `true` if there is **not** a power pellet active and Pacman is touch a ghost.

`Rules.win?/3` returns `true` if Pacman has eaten all the dots and the previous `lose?` condition is `false`.

## Freelancer Rates

This one was all about integers and floating point numbers (floats). We have 4 tasks:

1. Implement a function to calculate the daily rate given an hourly rate
    
2. Implement a function to calculate the price after a discount
    
3. Implement a function to calculate the monthly rate, and apply a discount
    
4. Implement a function that takes a budget, an hourly rate, and a discount, and calculates how many days of work that covers
    

There are more business logic rules and expected return types in the actual exercise, but I don't think its particularly relevant to repeat here. You can look at the full exercises yourself if you want to start Exercism's Elixir track.

```plaintext
def daily_rate(hourly_rate) do
  hourly_rate * 8.0
end
```

This was the easiest one. Just multiply the hourly rate by 8. Since they wanted the result as a float, I used `8.0` instead of `8`.

```plaintext
def apply_discount(before_discount, discount) do
  before_discount * (1 - discount / 100)
end
```

The discount is given as a fractional number representing a percentage. For example `25.0` represents 25%. So in my solution, I take `1 - discount / 100` to get `0.75`, which I then multiply by the `before_discount` rate to get the final return value.

```plaintext
def monthly_rate(hourly_rate, discount) do
  ceil(apply_discount(daily_rate(hourly_rate) * 22, discount))
end
```

For this one, they were expecting the value to be rounded up. So I used my previously writeen `daily_rate` function to get the daily rate, multiplied that by 22 to get the monthly rate, applied the discount, and then rounded everything up with `ceil`.

```plaintext
def days_in_budget(budget, hourly_rate, discount) do
  Float.floor(budget / apply_discount(hourly_rate * 8, discount), 1)
end
```

This was the most complicated function in the exercise, but it follows the same basic formula of composing functions together. Divide the supplied budget by the discounted hourly rate, then use `Float.floor` to return the expected result rounded down to one decimal place.

## Secrets

This was a fun one that used anonymous functions and bit manipulation. There are 7 tasks this time:

1. Implement `Secrets.secret_add/1`. It should return a function which takes one argument and adds to it the argument passed in to `secret_add`.
    
2. Implement `Secrets.secret_subtract/1`. It should return a function which takes one argument and subtracts the secret passed in to `secret_subtract` from that argument.
    
3. Implement `Secrets.secret_multiply/1`. It should return a function which takes one argument and multiplies it by the secret passed in to `secret_multiply`.
    
4. Implement `Secrets.secret_divide/1`. It should return a function which takes one argument and divides it by the secret passed in to `secret_divide`.
    
5. Implement `Secrets.secret_and/1`. It should return a function which takes one argument and performs a bitwise and operation on it and the secret passed in to `secret_and`.
    
6. Implement `Secrets.secret_xor/1`. It should return a function which takes one argument and performs a bitwise xor operation on it and the secret passed in to `secret_xor`.
    
7. Implement `Secrets.secret_combine/2`. It should return a function which takes one argument and applies to it the two functions passed in to `secret_combine` in order.
    

Building anonymous functions and use closures are some powerful concepts that some people find very hard to wrap their head around at first. But if you can understand them, you'll find how useful they can be for almost any programming task.

First, I'll talk about my solution to the first 6 tasks:

```plaintext
def secret_add(secret), do: &(&1 + secret)
def secret_subtract(secret), do: &(&1 - secret)
def secret_multiply(secret), do: &(&1 * secret)
def secret_divide(secret), do: &(div(&1, secret))
def secret_and(secret), do: &(Bitwise.band(&1, secret))
def secret_xor(secret), do: &(Bitwise.bxor(&1, secret))
```

Remember that each of our functions should be returning functions themselves. This is the tricky part.

In the syllabus, I learned the shorthand capture notation, which I use here. `&( ... )` represents an anonymous function. Anything inside the parentheses is what that inner function will do/return.

For example, the first function - `Secrets.secret_add/1` - accepts a number (represented by `secret`. We then return a function that **also** accepts a number (represented by `&1`) and adds `secret` to it.

If you can understand that, you can then understand the remaining functions in the solution. They all just return anonymous functions that do some math. The last two above do "bitwise math" which you might have to read up on if you're not familiar. But honestly, you don't have to understand it to see that we're just applying an operator on `&1` and `secret` just like the rest of the functions.

The final function in this exercise was much more difficult for me to code but very easy to understand looking back at it. This function must accept two of the previous functions and "combine" them. Here's the solution:

```plaintext
def secret_combine(secret_function1, secret_function2) do
  &(secret_function2.(secret_function1.(&1)))
end
```

Looking as deep into the composition as we can, you can see that we first pass `&1` to the first passed-in function, then that result gets passed into the second function, then finally the whole thing gets returned as a function itself. As I said, it's a little tricky at first, but I highly recommend learning this pattern if you want to better yourself as a programmer.

## Language List

The last exercise I did to complete this month's challenge was "Language List." The big concept to understand here is how Elixir handles Lists. If you know anything about Linked Lists, this will come natural to you.

There are 6 tasks:

1. Define the `new/0` function that takes no arguments and returns an empty list.
    
2. Define the `add/2` function that takes 2 arguments (a language list and a string literal of a language). It should return the resulting list with the new language prepended to the given list.
    
3. Define the `remove/1` function that takes 1 argument (a language list). It should return the list without the first item.
    
4. Define the `first/1` function that takes 1 argument (a language list). It should return the first language in the list.
    
5. Define the `count/1` function that takes 1 argument (a language list). It should return the number of languages in the list.
    
6. Define the `functional_list?/1` function which takes 1 argument (a language list). It should return a boolean value. It should return true if "Elixir" is one of the languages in the list.
    

Here we go.

```plaintext
def new(), do: []
```

This couldn't get must simpler. We just return an empty list `[]`.

```plaintext
def add(list, language), do: [language | list]
```

Using `[head | tail]` notation makes it easy to prepend a passed-in language to a passed-in list.

```plaintext
def remove(list) do
  [_head | tail] = list
  tail
end
```

For this one, I am simply assigning the first element in the list to `_head` and the remaining elements to `tail`. Then I return `tail` which effectively removes the first element.

```plaintext
def first(list) do
  [head | _tail] = list
  head
end
```

Notice this one is almost identical to the previous one. We're just returning the `head` (first element) this time.

Looking back, I'm sure there is a way to do both `remove` and `first` in a single line. If I continue my Elixir journey, I'm sure that would be easy information to find.

```plaintext
def count(list), do: length(list)
```

Easy. Use `length` to return the length of a list.

```plaintext
def functional_list?(list), do: "Elixir" in list
```

In the syllabus, I learned about the `in` operator which made this one easy, too.

## Two months down

With those five exercises in Elixir complete, I'm finished with Functional February! Ten more months and ten more languages to go.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675616112170/19ef504a-9408-45c7-a1f4-0be83c6ecc88.png align="center")

Elixir was a great experience. I enjoy functional programming. I tend to write my apps functionally if at all possible. I use this paradigm for both work and fun projects. Elixir was a joy to use, but I feel like I was missing something. Since I only used the in-browser code editor on Exercism's site, I don't feel like I got the full experience. If I decide to continue trying Elixir, I will definitely set up a local development environment on my PC. In fact, I will probably attempt to develop locally for all future languages as I progress through the #12in23 challenge.