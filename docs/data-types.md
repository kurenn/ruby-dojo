---
title: Data Types
date: 2021-04-15
slug: data-types
---

## What are you going to learn?

* Understand the basic data types Ruby offers
* Explain the difference between some of the most important built-in data types
* Be able to understand how Ruby Docs is organized and to search for methods 
* Correctly utilize the basic syntax for data types in Ruby
* Execute simple Ruby scripts in their own operating system's command line

## What are data types?

Interacting with data is all about programming, and this comes in different forms, such as numbers, text or collections of data. The most basic Ruby data types we will cover here are:

* Integers
* Floats
* Strings

Before jumping into how this works, you have to be aware that every single one of the data types described above are called [classes](https://en.wikipedia.org/wiki/Class_(computer_programming)). 
In short terms, a **Class** is how behavior is described in object-oriented programming(OOP), and the data for each of these classes are called [instances](https://en.wikipedia.org/wiki/Instance_(computer_science)) or objects. Here we provide a bunch of examples:

* `2` is an instance of the `Integer` class
* `3.14` is an instance of the `Float` class
* `'Hello World'` is an instance of the `String` class
* `['apple', 'orange', 'banana']` is an instance of the `Array` class 
* `{ name: 'Darth Vader' }` is an instance of the `Hash` class

Don't worry if you don't get it right away, we will talk more about this topic later in the course. For now, just make sure you feel comfortable by saying *instance of the _____ class* for each of the basic data types discussed above.

As you can imagine, you can perform operations on such instances such as `sum`, `multiplication`, `concatenate words`, and some others:

```
1 + 2
'hola' + 'mundo'
10 / 2
5 * 8
```

Ruby comes with an interactive interpreter on which you can perform many of these operations, just open a `terminal` and type in `irb`. You will see something like:

```bash
irb(main):001:0> 1 + 2
=> 3
```

In there you can copy and paste some of the examples provided above. **Make sure you press the *Enter* key to execute**.

## Variables

In programming **variables** are used to store data. You can interact with the data and perform operations just like shown above. 

```ruby
x = 1
```

Just like in math, while working with equations, Ruby evaluates assignment from right to left. This means it will evaluate whatever expression on the right of the `=` and storing
the result into that variable.

To break it into steps:

1. All of the variables need a name, which can be almost anything, in this case `x`. 
2. Next to the variable name, there is an [Assignment Operator](https://en.wikipedia.org/wiki/Assignment_(computer_science)), `=`
3. After the assignment operator, comes an expression, in this case `1`

Variables can store any data type, from integers to hashes. In Ruby unlike other programming languages, you can also switch the variable to a completely different datatype. This is 
called [Dynamic Typing](https://en.wikipedia.org/wiki/Type_system#Type_checking). Check it out:

```ruby
x = 1
x = 'Luke, I am your father'
x = ['orange', 1, 3.14]
```

It is very important that you keep track of the value a variable holds, as you may encounter with unexpected behavior or errors when executing your program, such as trying to sum a integer and a string.

```ruby
a = 1
b = 2
a + b
b = '2'
a + b
```

If you try to run the code above on the ruby interpreter, the ouput of the execution would be what is called an [exception](https://en.wikipedia.org/wiki/Exception_handling). 
This terminates the execution of the program, while providing meaninful information on what happened:

```bash
Traceback (most recent call last):
   5: from /home/kurenn/.rbenv/versions/2.7.2/bin/irb:23:in '<main>'
   4: from /home/kurenn/.rbenv/versions/2.7.2/bin/irb:23:in 'load'
   3: from /home/kurenn/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/irb-1.2.6/exe/irb:11:in '<top (required)>'
   2: from (irb):8 
   1: from (irb):8:in '+'
   TypeError (String can't be coerced into Integer)
```

Don't worry if this looks a bit scary, we will talk more about that as we move forward on the course. For now, just focus on tracking the variables final values.

## Naming variables

Giving meaninful names to your variables is really important, because as your code grows, it will become easier to understand the intention of that variable as a whole.
There are some guidelines to follow when declaring a variable:

* Must start with lowercase
* Can only contain numbers or letters and underscores `_`
* Ruby uses the *Snake Case* to name variables, which means all letters are lowercase with words separated by `_`.

Here are some examples of good variable naming:

```ruby
greetings = 'Hello'
number_of_pizzas = 4
sith_lords = ['Darth Vader', 'Darth Sidius']
```

And here are some bad examples even though they work:

```ruby
greeTings = 'Hello'
numberOfPizzas = 4
sith_Lords = ['Darth Vader', 'Darth Sidius']
```

We cannot stress enough on how important variable naming is, for example if you are running a `sum` or calculating an `average`, you can actually use those names as variables instead of `x` or `num`.

## Integers

Integers are simple numbers, without decimals:

* 1
* -456
* 1_535_465

Make sure you have a ruby interpreter open, so you can test some of the examples we are going to provide.

### Basic Operations

```ruby
 3 + 5
 #=> 8

 9 - 4
 #=> 5

 5 * 2
 #=> 10

 10 % 3
 #=> 1 (this is the remainder of the division between 10 and 3)

 8 / 6
 #=> 1
```

### Built-in methods

Ruby comes with a bunch of functions that act on instances of classes, such as [Integer](https://ruby-doc.org/core-2.5.0/Integer.html), and you can use them as such:

```ruby
1.odd?
#=> true

1.even?
#=> false

-34.abs
#=> 34
```

We highly recommend you have a read on the official [documentation on integers](https://ruby-doc.org/core-2.5.0/Integer.html), try some of the methods Ruby already offers.

## Floats

Floats are numbers with decimals:

* 3.1416
* -456.23

### Basic Operations

```ruby
 3.5 + 5.5
 #=> 9

 9.1 - 4.7
 #=> 4.3999999999999995

 5.5 * 2.4
 #=> 13.2

 8.0 / 6.0
 #=> 1.333333333333
```

### Built-in methods

```ruby
1.4.round
#=> 1

5.6.ceil
#=> 6

-34.45.abs
#=> 34.45

(1.34).floor
#=> 1
```

We highly recommend you have a read on the official [documentation on floats](https://ruby-doc.org/core-2.5.0/Float.html), try some of the methods Ruby already offers.

## Strings

Text between single quotes `'` or double quotes `"` 

### Built-in methods

```ruby
'Hello World'.length
#=> 11

'hello'.upcase
#=> "Hello"

'hello'.chars
#=> ["h", "e", "l", "l", "o"]
```

We highly recommend you have a read on the official [documentation on strings](https://ruby-doc.org/core-2.5.0/String.html), try some of the methods Ruby already offers.

### String concatenation

In simple words, is the act of joining two or more strings into a new one. This is achievable through the `+` operator. For example:

```ruby
hello = 'Hello'
world = 'World'

hello + world 
#=> 'HelloWorld'

hello + ' ' + world + '!'
#=> 'Hello World!'
```

### String Interpolation

Out of the surface, it works similar to concatenation by creating a new String and evaluating whatever is inside the #{}. **The String must be in double quotes to use this technique**. For example:

```ruby
hello = 'Hello'
world = 'World'

"#{hello}#{world}"
#=> 'HelloWorld'

"#{hello} #{world}!"
#=> 'Hello World!'

"The sum between 1 and 2 is #{1 + 2}"
#=> "The sum between 1 and 2 is 3"
```

As you can see this is a much cleaner and more readable way to join strings into one. But remember to keep if sane while interpolating your strings.

## Change Data Types

You can change the data type from one to another in order to have another behavior. This action is also known as [type cast]https://en.wikipedia.org/wiki/Type_conversion). For example:

```ruby
"1".to_i
#=> 1 (from string to integer)

"3.14".to_f
#=> 3.14 (from string to float)

"1 ring to rule them all".to_i
#=> 1
```

When performing this type of operations, unexpected behavior may occur which Ruby may not warn you about:

```ruby
"_5".to_f
#=> 0.0

"The result of 1 + 2 is 3".to_i
#=> 0
```

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/data-types`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)