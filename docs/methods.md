---
title: Methods
date: 2021-04-15
slug: methods
---

## What are you going to learn?

* Understand what is a `method`, arguments and return values 
* Group logic into methods for reutilization
* Understand when to use methods and why
* Correctly utilize the basic syntax for methods in Ruby

## What is a Method?

A `method` is a block of code to achieve `one` particular goal. We actually have been already working with them through the [dot notation](https://dwayne-phillips.medium.com/object-oriented-programming-and-dot-notation-b8cdbe0fe825), some examples are:

```ruby
"Hi, my name is Alice".upcase
#=> HI, MY NAME IS ALICE

5.to_f
#=> 5.0
```

One of the most important on why `methods` exists, is to reuse code, but also to give more meaning to a block of code, that otherwise will just be floating around. So take for example
the `upcase` method, what would you do if that does not exist?. You can also think of methods like sending messages to **instances** or **objects**. In other words, telling a string to
provide an upcased version of it.

This is how a `method` definition looks like:

```ruby
def greetings(name)
  puts "Hi, #{name}"
end

# This is how you call or invoke a method
greetings("Alice")
#=> Hi, Alice
```

We will decompuse the code above, just to clarify what is happening:

![alt text](method_description.png)

As you can see above, there a lot of things happening here on which we'll spend some time explaining it.

#### Arguments

Arguments are the input or additional information for a method.

* Each argument work as a local variable, and its scoped is limited to the code inse the method
* When calling the method, you have to provide the arguments
* When defining a method, arguments are also known as **parameters**. Placeholders for arguments
* There is no limit on how many arguments a method can have

Here is an example of a method invokation with a parameter:

```ruby
"Hello World!".include? "Hello"
#=> true

# This is also valid
"Hello World!".include?("Hello")
#=> true
```

In the code above, you are calling the `include?` method, on a string. The argument is `"Hello"`, and the return value is `true`. Note that parenthesis are optional.

Here is a method invokation with two parameters:

```ruby
"And I am Alice".gsub("Alice", "Iron Man")
#=> And I am Iron Man

# This also works, and is actually more common
"And I am Alice".gsub "Alice", "Iron Man"
```

#### Return values

In programming a **Return value** is the output or result of a method. 

* You can only return one value per method
* A return value can be anything, an integer, string, `nil`, array, etc.
* In Ruby you can explicitly return a value with the `return` keyword, but **Ruby will return the last line on the method**

```ruby
"alice".capitalize
#=> Alice
```

As you can see the `capitalize` method returns a string with the first character upcased.

This is how you define a returning value:

```ruby
# This method uses an explicit return value
def calculate_sum(a,b)
  sum = a + b
  return sum
end

# This method just returns the last line
def calculate_sum(a,b)
  sum = a + b
  sum
end
```

The preferred way in Ruby to return a value is not to include the `return` keyword, unless you explicitly need to finish a method execution:

```ruby
def calculate_sum(a,b)
  return unless a.is_a?(Integer) || b.is_a?(Integer)
  sum = a + b
  sum
end
```

The code above will terminate its execution if `a` or `b` are not numbers.

### Calling methods inside other methods

You can call multiple methods inside a definition of another one:

```ruby
def calculate_sum(a,b)
  a + b
end

def pretty_sum(a,b)
  sum = calculate_sum(a,b)

  puts "The sum between #{a} and #{b} is #{sum}"
end

# This method invokation will call the calculate_sum with 
# a and b passed as parameters to the pretty_sum method
pretty_sum(5,3)
#=> The sum between 5 and 3 is 8
```

One of the advantages of using methods is that you can work with different levels of complexity or abstraction. [Abstraction](https://en.wikipedia.org/wiki/Abstraction_(computer_science))
is a concept in computer science to model high complex problems into simple steps, and focus higher layers of functionality.

Think of this as using your computer. You don't actually need to know everything that happens inside the circuits, or when you type. All of these details are *abstracted* from you, and the
only thing you need to know is how to interact with the keyboard or interface.

Don't worry if you don't get it right away, it will eventually sink, and we will talk this topic again on the `classes` lesson.

Before jumping into the exercises, here are some good practices when creating methods:

* Provide meaningful names, even if they are long
* Methods should only take care of one task
* Try to keep them as long as 4 lines
* Even though you don't have a limit when it comes to having arguments, try to keep it simple, 3 at most

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/methods`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)