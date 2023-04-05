---
title: "Analytical April: Learning Python for Data Science"
datePublished: Wed Apr 05 2023 12:00:38 GMT+0000 (Coordinated Universal Time)
cuid: clg3n0zkm01lu1unv4v7od89r
slug: analytical-april-learning-python-for-data-science
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680638789639/94ddfe13-b3dc-48b8-9ba7-0b63d8c4e00f.png
tags: python, data-science, programming-languages, exercism, 12in23

---

Python is a powerful programming language that has become increasingly popular over the years. It's a great language for beginners and experienced programmers alike. Its vast array of libraries and frameworks makes it an excellent choice for data science and data analysis. That's why I decided to try it out in this month's #12in23 challenge for Analytical April.

If you're not familiar with the #12in23 Challenge, it's a programming-language learning challenge that tasks you with trying out 12 different languages in the year of 2023. It's coordinated by [Exercism.org](http://Exercism.org).

> The idea is simple: each month, you'll focus on learning a new programming language. We'll provide resources and support to help you along the way, and you can track your progress and connect with other participants on our online community.

I'm documenting my experience with this challenge throughout the year.

Each month has a specific theme in mind. For April, the theme is Analytical April. The focus is on languages that excel at data science. You can work toward the goal by completing exercises from Julia, Python, or R.

I have used a bit of Python and R, so the natural choice would've been to go with Julia. However, due to the fact that I haven't used Python in a long time or very extensively, and that it is one of the most popular programming languages out there, especially for data science...

I chose Python!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680625747960/37d091fc-13da-4d6b-869a-51cf661022d1.png align="center")

Python is popular. It is consistently one of the most used languages in the world. Plus, the people who use Python love it. It has a simple syntax which makes it easy to learn and use. Features like high-level data structures, garbage collection, and dynamic typing make it even easier. It is a general programming language with support for many systems. Couple that with its impressive standard library, and you can see why Python can be found everywhere: it is extremely versatile. Its large and friendly community helps Python stand out amongst other programming languages. If you decide to learn Python, you'll be embarking on an endeavor with a delightful language backed by a very active community for support.

Of course, there are other reasons to learn Python, like its interoperability with other languages (like C and Java) and its scalability (as proven by its use in big tech firms like Google and Meta). I could go on and on about its virtues, but I think its best to continue with the point of this article.

This time again, I'll be installing and setting up a local Python development environment for this month. The main reason for doing this is so I can have access to more powerful developer tooling. I want it to be as easy as possible to write code, debug it, run tests faster, and just get a better sense of the real-world experience developers might have with the language in general.

![Hello World](https://dg8krxphbh767.cloudfront.net/exercises/hello-world.svg align="center")

# Hello, World!

Exercism tracks always start with a required "Hello, World!" exercise just to make sure you can complete and submit everything okay. They gave me this code to start with.

```python
def hello():
    return 'Goodbye, Mars!'
```

This one was incredibly simple. I just had to make the program return "Hello, World!" instead of "Goodbye, Mars!" so I just had to change that one string.

```python
def hello():
    return 'Hello, World!'
```

With Hello, World! out of the way, let's get started solving 5 exercises so we can complete this month's challenge.

I'll be completed each of the 5 "featured exercises." These exercises are chosen by Exercism because they particularly highlight a scenario where this month's language theme is beneficial. While you can complete the monthly challenge by completing any 5 exercises, you will get an extra-special "year-long" #12in23 badge for completing the featured exercises.

The 5 exercises that exemplify Analytical languages are ETL, Largest Series Product, Saddle Points, Sum Of Multiples, and Word Count. Let's start working on those.

![ETL](https://dg8krxphbh767.cloudfront.net/exercises/etl.svg align="center")

# ETL

ETL stands for Extract-Transform-Load. It's a common pattern when working with data. Say you have some data in an old system and you need to move it to a new system. First, you extract the data from the old system, then you transform it in some useful way - at least to make it compatible with the new system - and then you load it into the new system.

In this case, we are given a set of data that stores a list of scores and the letters that correspond to them (think "Scrabble").

* 1 point: "A", "E", "I", etc.
    
* 2 points: "D", "G"
    
* 3 points: "B", "C", "M", etc.
    
* and so on
    

We need to transform this data into a new format. One that uses a list of letters with their corresponding score.

* A: 1 point
    
* B: 3 points
    
* C: 3 points
    
* and so on
    

To get us started, the exercise gives us a function that we have to fill in.

```python
def transform(legacy_data):
    pass
```

My solution uses nested `for` loops to pull the letters from each score. It places the letters in an accumulating dictionary that we eventually return.

```python
def transform(legacy_data):
    # Create an empty dictionary
    result = {}

    # Loop through all of the keys in `legacy_data`. Each key is a point
    # value
    for points in legacy_data:
        # Each key (point value) corresponds to an array of letters. Loop
        # through the array of letters
        for letter in legacy_data[points]:
            # Assign the point value we're current in to the key
            # `letter.lower()` in our `result` dictionary. `.lower()` ensures
            # that all letters in the result are lowercase
            result[letter.lower()] = points

    # Return the dictionary filled with letters and corresponding point
    # values
    return result;
```

![Largest Series Product](https://dg8krxphbh767.cloudfront.net/exercises/largest-series-product.svg align="center")

# Largest Series Product

For this exercise, you must write a function that takes two arguments:

1. A string of numbers such as `1027839564` called the `series`
    
2. A number such as `3` called the `size`
    

The function should return the largest product for a contiguous substring of `series` of length `size`.

Using the same numbers as above for example, the substring of size `3` which produces the largest product is `956`. 9 x 5 x 6 = `270`, which is the number that would be returned in this case.

In my solution, I first check for a few error conditions and `raise` exceptions for each one.

Then I start at index `0` and find the product for all digits after it up to the given `size`. If that product is larger than the current`largest_product`, I save it as the new `largest_product`. Then I start another iteration starting at index `1`. I keep looping through until all substrings in the `series` are checked. At the end, I return the `largest_product`.

```python
def largest_product(series, size):
    # If the size is negative, raise an error
    if size < 0:
        raise ValueError("span must not be negative")
        
    # If the series is empty and the size is positive, raise an error
    if series == "" and size > 0:
        raise ValueError("span must be smaller than string length")

    # If the series is not empty but cannot be converted to an integer, raise 
    # an error
    try:
        series != "" and int(series)
    except ValueError:
        raise ValueError("digits input must only contain digits")

    # If the size is bigger than the series, raise an error
    if size > len(series):
        raise ValueError("span must be smaller than string length")

    # Special case: If the series is empty and the size is zero, the largest
    # product is 1
    if series == "" and size == 0:
        return 1

    # Before any series are calculated, 0 is the largest product
    largest_product = 0

    # Loop through a range starting at 0 and ending at the end of the series,
    # minus the size
    for i in range(len(series) - size + 1):
        # Get a slice of the series that starts at the current position and 
        # is the length of the given size
        subseries = series[i:i + size]

        # The product starts at 1
        product = 1

        # For each digit in the slice...
        for digit in subseries:

            # Multiply the current product by the digit
            product *= int(digit)

        # If the calculation resulted in a product larger than the current
        # largest, set the new product as the largest
        if product > largest_product:
            largest_product = product

    # Return the largest product
    return largest_product
```

![Saddle Points](https://dg8krxphbh767.cloudfront.net/exercises/saddle-points.svg align="center")

# Saddle Points

I had never heard of a "saddle" point before, but this exercise gave a strict definition of how to determine one in this case.

Given a matrix like this:

```python
9 8 7
5 3 2
6 6 7
```

Find any position where the number in that position is the largest of any other in its row *and* it is the smallest of any other in its column. In the example matrix above, the `5` that is at row 2, column 1 meets this criteria, so it is a saddle point.

For my solution, I first saved the largest number for each row in a list. Then I stored the smallest number for each column in a list. With that information ready, I used nested `for` loops to access each number, checking if it was the largest in its row and smallest in its column. If so, I appended the `row` and `column` indexes to a list called `saddle_points`. When the loops finish, the `saddle_points` list is complete and I return it.

```python
def saddle_points(matrix):
    # If the given matrix is empty, return an empty list
    if len(matrix) == 0:
        return [];

    # Use `*` to unpack the list of lists, then `zip` them together. This
    # transposes (or "rotates") the matrix. Now each "row" is a "column".
    cols = zip(*matrix)

    # Map over each row and get the largest number
    row_maxs = list(map(max, matrix))

    # Map over each col and get the smallest number
    col_mins = list(map(min, cols))

    # Create an empty list that we can append to
    saddle_points = []

    # Loop over each row
    for row_index, row in enumerate(matrix):
        # If the current row isn't the same length as the first row, the 
        # matrix doesn't have an equal length in each row. It is irregular; 
        # raise an exception.
        if len(row) != len(matrix[0]):
            raise ValueError("irregular matrix")
        
        # Loop over each number in the current row
        for col_index, n in enumerate(row):

            # If the current number is greater than or equal to the largest
            # number in the row AND it is less than or equal to the smallest
            # number in the column...
            if n >= row_maxs[row_index] and n <= col_mins[col_index]:
                # ...a saddle point was found. Append the row and column 
                # index to the list
                saddle_points.append({ "row": row_index + 1, "column": col_index + 1})
    
    # Return the list of saddle points
    return saddle_points;
```

![Sum of Multiples](https://dg8krxphbh767.cloudfront.net/exercises/sum-of-multiples.svg align="center")

# Sum Of Multiples

In this exercise, you are given a list of `factors` and a `limit`. You must add up all the unique multiples of the `factors`, up to the `limit`. So if the factors are `[3, 5]` and the limit is `20`...

1. We find the multiples of 3 that are less than 20: 3, 6, 9, 12, 15, 18
    
2. Find the multiples of 5 that are less than 20: 5, 10, 15
    
3. Compile a list of unique multiples: 3, 5, 6, 9, 10, 12, 15, 18
    
4. Sum them: 78
    

My solution uses a `for` loop to work over all given `factors`, it then starts a `while` loop that runs while the current `multiple` is under the given `limit`. It appends that `multiple` to a list, then moves on to the next multiple.

After the loops are complete, I remove duplicates by converting the list to a `set` and then back to a `list`. Finally, I return the `sum` of the list.

```python
def sum_of_multiples(limit, factors):
    # Create an emply list that will be used to contain multiples
    multiples = []

    # For each factor given...
    for factor in factors:
        # If the factor is 0, go to the next factor
        if factor == 0:
            continue

        # Set/reset `multiples` to 0
        multiple = 0

        # While the current multiple is less than the given limit...
        while multiple < limit:
            # Append that multiple to the list of multiples
            multiples.append(multiple)

            # Increase the multiple by the current factor and then loop again
            multiple += factor
            
    
    # Remove duplicates by converting the list to a set and then back to a 
    # list
    unique_multiples = list(set(multiples))

    # Return the sum of the list of unique multiples
    return sum(unique_multiples)
```

![Word Count](https://dg8krxphbh767.cloudfront.net/exercises/word-count.svg align="center")

# Word Count

The last exercise was to take a sentence and return a list of all the words in that sentence, along with how many times the word appears.

For this one, I...

1. Converted the sentence to all lowercase letters
    
2. Used a regular expression (regex) to remove any non-alphanumeric or apostrophe characters
    
3. Split the sentence on whitespace to get a list of words
    
4. Create a new empty dictionary
    
5. Use a `for` loop over each word
    
    1. If the word already exists in the dictionary, increment its number by 1
        
    2. If it doesn't, add it to the dictionary with a value of 1
        
6. Return the dictionary
    

```python
# Import `re` to enable the use of `re.sub()`
import re

def count_words(sentence):
    # Convert the entire sentence to lowercase
    lower_sentence = sentence.lower()

    # Use `re.sub()` and a regular expression to remove apostrophes that 
    # appear at the beginning or end of any word, and also remove any 
    # character that isn't a lowercase letter, a number, whitespace, or 
    # apostrophe. The removed characters are replace by a space
    clean_sentence = re.sub(r"(?<!\w)'|'(?!\w)|[^a-z0-9\s']+", " ", lower_sentence)

    # Split the sentence on whitespace
    split_sentence = clean_sentence.split()

    # Create an empty dictionary of words
    words = {}

    # Loop through each word in the sentence
    for word in split_sentence:
        # If the current word is already in the dictionary...
        if word in words:
            # Increment that word's count by 1
            words[word] += 1

        # If it's not in the dictionary yet...
        else:
            # Add it to the dictionary with a count of 1
            words[word] = 1

    # Return the dictionary of words
    return words
```

# Four months down

With those five exercises in Python complete, I'm finished with Analytical April! Eight more months and eight more languages to go.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680626870633/d3c3d09a-abea-489f-aaf3-f7280245c3ec.png align="center")

In addition, I've published solutions to all 15 of the [**featured exercises**](https://forum.exercism.org/t/new-12in23-badge-for-completing-all-the-things/4183) so far. This progress brings me closer to the exclusive year-long #12in23 badge.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680626910005/7522a1da-c039-4414-a0a9-1e7a545aaf40.png align="center")

Overall, Python was a great experience. What strikes me most about the language is that the syntax is easy to use. While reading the problems for each exercise, solutions in Python seemed to naturally write themselves in my mind. A little bit of tweaking and debugging later, and I've got a working solution that passes all the tests. I can see myself using this language again when the need arises. There are many times I reach for JavaScript when working with data, if only for the reason that my data is already accessible by whatever JavaScript-based system I'm using. However, there are times when bringing in Python could be very beneficial. I'll have to keep this language in mind for times like those.

I've posted the solutions in this article on GitHub with much more detailed comments in the code.

%[https://github.com/travishorn/python-exercism] 

But I highly suggest you try to solve some of the exercises on [**Exercism.org**](http://Exercism.org) on your own. Won't you join me in trying 12 new programming languages in 2023? I'm curious to hear about your experience with this challenge. Leave a comment and let me know what strategies have worked for you or which languages you're most excited to learn.