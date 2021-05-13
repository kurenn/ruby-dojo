---
title: Inheritance
date: 2021-04-15
slug: inheritance
---

## What are you going to learn?

* Understand what it is inheritance
* Identify when to use inheritance
* Understand how inheritance works in Ruby

## What is Inheritance?

Inheritance is a way to define classes that will share their behavior with multiple classes. You can easily reduce the code duplication by defining
what is called a **Parent class** that can be used across multiple subclasses or **child-classes**.

> Inheritance is, at its core, a mechanism for automatic message delegation. It defines a forwarding path for not-understood messages.

In simple words:

* The class being inherited from is called the parent or superclass
* A class can only inherit from one parent. This is not the same to other programming languages.
* Any number of classes can inherit from a single superclass
* When a class inherits from another class, it has access to all of the methods and instance variables from the superclass
* The inheriting class is called the child or subclass
* A subclass parent's parent and so on, is called **ancestor**

This is how it looks with Ruby code:

```ruby
# This is the superclass or parent class
class Animal
end

# This is a child class
class Cow < Animal
end

# This is a child class
class Duck < Animal
end
```

In the code shown above, `< Animal` tells Ruby that any class with this snippet will inherit behavior from the `Animal` superclass. This is not the same as a module, 
even though it kind of seems the same. Remember a module also defines namespaces or context.

Whena applying inheritance take this into consideration:

* Inheritance is for specialization, NOT for sharing code.
* You should never inherit from a concrete Class. Always inherit from abstract Classes.

Here is a practical application from Sandi Metz:

> Some of [a] bicycle’s behavior applies to all bicycles, some only to road bikes, and some only to mountain bikes. This single class contains several different, but related, types. This is the exact problem that inheritance solves; that of highly related types that share common behavior but differ along some dimension.

### Subclasses

We already saw how a subclass looks like, by just adding the `< ParentClass` to the class definition, and we are ready to go. But's let's build a more comprehensive
example on how this works.

```ruby
class Pokemon
  attr_reader :name

  def initialize(name = "")
    @name = name
  end
end
```

We inherit the class `Pokemon` to the `Pikachu` subclass

```ruby
class Pikachu < Pokemon
end

pikachu = Pikachu.new("Sparky")
pikachu.name
```

As you can see, we did not define the **constructor** method, nor the `name` attribute. The `pikachu` object responded to that because is inheriting all that from `Pokemon`. The advantage
here is that we can still add methods to the `Pikcahu` class.

Unlike modules, the methods being inherited are not passed down to the subclass, but are looked up or delegated to the parent class.

### Super

There will be a time when you want to extend parts of the behavior you are inheriting, this is when `super` comes into play. For example if you call the `super` method inside a subclass `initialize`
method, it will call the `superclass`'s  initialize.

For example:

```ruby
# pokemon.rb
class Pokemon
  attr_reader :name

  def initialize(name = "")
    @name = name
  end
end

# pikachu.rb
class Pikachu < Pokemon
  def initialize(name, sound)
    super(name)
    @sound = "pika pika"
  end
end
```

The call `super` will invoke the `initialize` method from the `Pokemon` class, which will set the `@name` variable. You need to respect the `super` arguments when it applies, this means that
if the parent class expect to receive 1 parameter, you must add it.

### Overrding Methods

As stated in the [class](/classes-and-instances) lesson, we saw how we can override methods from built-in Ruby classes. This works the same while inheriting from a class. Let's say we have a `make_sound`
method on the `Pokemon` class that the `Pikachu` class will change:

```ruby
# pokemon.rb
class Pokemon
  attr_reader :name

  def initialize(name = "")
    @name = name
  end

  def make_sound
    ""
  end
end

# pikachu.rb
class Pikachu < Pokemon
  def initialize(name, sound)
    super(name)
    @sound = "pika pika"
  end

  def make_sound
    @sound
  end
end
```

This will totally override the method, without using any of the superclass implementation. You could use the `super` clause if you need it.

### Properly Applying Inheritance

> Subclasses are everything their Superclasses are, plus more. Any object that expect Bicycle should be able to interact with a Mountain Bike in blissful ignorance of its actual Class.

Two things are required for inheritance to work:

* There is a generalization-specialization relationship in the objects you are modeling.
* Correct coding techniques are used.

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/inheritance`.

## Additional Resources

+ [Acquiring Behavior Through Inheritance](https://github.com/serodriguez68/poodr-notes#chapter-6---acquiring-behavior-through-inheritance)
+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)