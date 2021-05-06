---
title: Variables Scope
date: 2021-04-15
slug: variables-scope
---

## What are you going to learn?

* Understand the scope of variables in Ruby
* Perform operations with the different scopes combined
* Track how variable changes behave on the program cycle

## Scope

So far we have been working with single files, methods on a ruby file, and classes, but we have not clarify a very important part of this,
the life-cycle of a variable when we execute our code, track the values, and how it can deliver unexpected behaviors if we don't track them accordingly.

We are going to tackle 4 types of scopes:

* **Global** - Any variable within this scope is accessible and modifiable everywhere. This are the most dangerous and least recommended.
* **Local** - Any variable within this scope is attached to the method or a block, such as `#each`
* **Instance Variable** - This variables are the ones used on a class, and they are only accessible on any instance method for the class
* **Class Variable** - This variables are accessible everywhere on a class

### Global Scope

Take this for example:

```ruby
name = "Alice"
puts name
```

As you can imagine, the output for this program is going to be "Alice", and although it does not seem quite obvious, the variable `name` is under the global scope, as it is not
within a method or a class, just floating around in the script.

* What would the next code output?
* Why do you think that is?

```ruby
name = "Alice"

def print_name(name)
  puts name
end

puts name
```


### Local Scope

Take this for example:

```ruby
def print_name
  name = "Alice"
  puts name
end

print_name
puts name
```

Remember local variables are only accessible for the method or block, and because the `name` variable was defined inside the method, we can manipulate as much as we want, but always
inside that method.

* What was the output of the code above?
* Is it what you expected?

### Instance Scope

Take this for example:

```ruby
class Person
  def initialize(name)
    @name = name
  end

  def greetings
    "The variable @name is accessible here, because it is an instance method"
  end

  def self.greetings
    "The variable @name is not accessible here, because it is an class method"
  end
end

```

This may not be so obvious, but as you can see the `self.greetings` method is a class method, therefore, any *instance variable* is not accessible within this methods. Remember that
*instance variables* can be accessed inside all the class, as long as they are instance methods.

### Class Scope

Check the following code:

```ruby
class Person
  @@name = "Person"

  def initiliaze(name)
    @name = name
  end

  def greetings
    puts @name
    puts @@name
  end

  def self.greetings
    puts @@name
  end
end

Person.new("Alice").greetings
Person.greetings
```

As you may imagine, both `greetings` method work, as the `@@name` variable can be accessed anywhere in the class, whether is an instance or class methods. Class variables are not
that common to use, but it is important for you to know.

## Exercises

For this lesson there are no exercises, it was more about showing you how variable scoping works in Ruby.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)