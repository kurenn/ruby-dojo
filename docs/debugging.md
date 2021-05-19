---
title: Debugging
date: 2021-04-15
slug: debugging
---

## What are you going to learn?

* Understand how to read error messages
* How to use Pry as a debugger tool to create breakpoints

## Debug

The process of debugging code, is something that we as developers do all the time not matter how experience you are. The main reasons to debug are:

1. The program does not run, and there is an **Error**
2. The code runs, but you are not getting what is expected. **Failure** or [Bug](https://en.wikipedia.org/wiki/Software_bug#:~:text=A%20software%20bug%20is%20an,to%20behave%20in%20unintended%20ways.&text=Bugs%20can%20trigger%20errors%20that,crash%20or%20freeze%20the%20computer.)

> A software bug is an error, flaw or fault in a computer program or system that causes it to produce an incorrect or unexpected result, or to behave in unintended ways

Having a debugging process or capability allows us to identify with ease the errors or bugs we introduce to the code. A simple 4-step process would look like:

1. Read the whole error
2. Find the error, commonly Ruby gives you the file and line number causing the error
3. Establish a hypothesis
4. Try your hypothesis

## Pry

A common tool to use for debugging is a gem called [Pry](https://pry.github.io/). This gem allows us to add breakpoints in our code inspect values from variables
or the state of an object.

To start using this gem, you can simply install by:

```bash
$ gem install pry
```

And then on your script, you need to require the gem like so:

```ruby
require 'pry'
```

### Adding a breakpoint

In order to add a breakpoint anywhere in our code, just add the following line:

```ruby
binding.pry
```

Check the code below for a complete example:


```ruby
fruits = ["apple", "banana", "watermelon", "peach"]
fruits.each do |fruit|
  binding.pry
  puts fruit.upcase
end
```

The code above, will stop 1 time per fruit on the array, and on each iteration we will be able to access the `fruit` variable or any other if that would be the case.

## Stack trace

Ruby is usually friendly when it comes to output errors into the console or what is called the [stack trace](https://en.wikipedia.org/wiki/Stack_trace). This is how
it looks like when trying to run this code:

```ruby
class SuperHeroe
  def initialize(name)
    @name = name
  end

  def greetings
    "Hi, my name is #{self.name}"
  end
end

super_heroe = SuperHeroe.new("SpiderMan")
super_heroe.greetings
```

The output after running this would be:

```bash
Traceback (most recent call last):
        1: from super_heroe.rb:12:in `<main>'
super_heroe.rb:7:in `greetings': undefined method `name' for #<SuperHeroe:0x00005597eea2dab0 @name="SpiderMan"> (NoMethodError)
```

Let's break this down a little bit:

* As you can see the first line - "1: from super_heroe.rb:12:in `<main>'" is telling us the error was triggered by the line 12, in this case the invokation for the
`greetings` method on the `super_hero` variable.
* Then, if we keep reading, the actual error is happening on line `7` from the file. The line reads `super_heroe.rb:7:in 'greetings': undefined method 'name' for #<SuperHeroe:0x00005597eea2dab0 @name="SpiderMan"> (NoMethodError)`.
  * As you can see, Ruby is telling us that the `name` method does not exists for the current instance. And all of this happened while trying to run the `greetings` method.

The trace would look something like:

```
SuperHero.new -> #greetings -> #name 
```

One alternative to fix the code above, is to add the method `name`:

```ruby
class SuperHeroe
  attr_reader :name

  def initialize(name)
    @name = name
  end

  def greetings
    "Hi, my name is #{self.name}"
  end
end

super_heroe = SuperHeroe.new("SpiderMan")
super_heroe.greetings
```

## Additional Resources

+ [Debugging](http://tutorials.jumpstartlab.com/topics/debugging/debugging.html)
+ [How to Debug Ruby Applications](https://www.youtube.com/watch?v=pM1fhJp5C_g)
+ [Mastering the Ruby Debugger](https://www.youtube.com/watch?v=GwgF8GcynV0)