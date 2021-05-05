---
title: Classes & Instances
date: 2021-04-15
slug: classes-and-instances
---

## What are you going to learn?

* Define classes
* Understand the difference between a class and instances
* Add methods to classes to provide behavior
* Understand how to create `set` and `get` methods in classes

## What is a Class?

A class, just like any other object-oriented programming language is used to model to things,**state** and **behavior**.

* State is what a something is. If for example we were to describe a person, we can model it with some attributes, like `name`, `birthdate`, `last_name` .
* Behavior is what the person is able to do, such as `run`, `eat`, `swim`. These would be translated as methods.

If you recall from other lessons, an instance is a representation of a class. We have been working with this since the beginning, for example when we
create an empty array, what is actually happening behind scenes is `Array.new`. 

Another way to think of a class is like a blueprint of a plane. This describes how the plane is supposed to be build. An instance would be the actual plane,
and you can have many planes based on that blueprint.

### Defining a class

This is how you define an empty class in Ruby:

```ruby
class YouClassName
end
```

Keep in mind a couple of things here:

* All class names in Ruby are written in [camel case](https://en.wikipedia.org/wiki/Camel_case)
* The definition starts with the `class` word
* They are commonly created in separated files, with the name of the class in [snake case](https://en.wikipedia.org/wiki/Snake_case). 

Now that you have defined your class, you can strat using it, even though nothing else has been added. For example:

```ruby
class Pokemon
end

Pokemon.new
#=> #<Pokemon:0x0000555fff229f98>
```

As you can see, anytime you want to create an instance of a pokemon, just use the class name, with the `new` method.

### Class Constructor

Now that you know how to define a new class, it is time to model a state through an `initialize` method. The `initialize` method is part of the object creation cycle
and it allows you to set initial values for the class. In other programming languages, this is called a "constructor".

Through out this lesson, we will be working with a `Pokemon` class, so buckle up trainers. To define a class with a constructor

```ruby
class Pokemon
  def initialize(name, trainer)
    @name = name
    @trainer = trainer
  end
end

Pokemon.new("Pikachu", "Ash Ketchum")
#=> #<Pokemon:0x00005560ecb20a00 @name="Pikachu", @trainer="Ash Ketchum">
```

There are a couple of thins happening here, first we added the `initialize` method along with two arguments, `name` and `trainer`. When working with classes, this are also
known as attributes, which will set an initial state for any instance.

The `@` before each variable indicates that is an attribute or *instance variable*. You tipically want to add this on the constructor, as we want those attributes to persist
throughout the object's lifetime.

Remember that the order of the arguments is important, so if we pass an empty string as the `name`, the pokemon name would be that. 

Passing arguments to the `initialize` method is a very common pattern, but this is not the only way to go, sometimes you may want to set default values to the class, let's
say every pokemon starts at `level` 1, so we may need to add that instance variable with a default value.

```ruby
class Pokemon
  def initialize(name, trainer)
    @name = name
    @trainer = trainer
    @level = 1
  end
end

Pokemon.new("Pikachu", "Ash Ketchum")
#=> #<Pokemon:0x00005560ecb20a00 @name="Pikachu", @trainer="Ash Ketchum", @level=1>
```

### Accessing Attributes

Now that we have a pokemon instance, you may want to access its name. We certainly can do that through the `dot-notation` syntax.

```ruby
pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.name
```

If you ran the code above, you probably encounter this error:

```bash
NoMethodError (undefined method `name' for #<Pokemon:0x00005560ecb9fa80>)
```

This happens because the *instance variables* are not accessible by the instance, in other words, they are **private and only accessibble from within the class**. This is
really easy to achieve:

```ruby
class Pokemon
  def initialize(name, trainer)
    @name = name
    @trainer = trainer
    @level = 1
  end

  def name
    @name
  end
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.name
#=> "Pikachu"
```

As you can see, we enable the access for the `@name` *instance variable* through a method called `name`, but it can actually be anything we want. This way of reading 
*instance variables* follows the same pattern across class definitions. Ruby hates to repeat code, and we can clean our class like so:

```ruby
class Pokemon
  attr_reader :name, :trainer, :level

  def initialize(name, trainer)
    @name = name
    @trainer = trainer
    @level = 1
  end
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.name
#=> "Pikachu"
```

`attr_reader` is a class method provided by Ruby, and it helps you create those access methods for the instance variables. Take this into consideration:

* There is no limit on how many arguments the `attr_reader` can receive, in this case, `name`, `trainer`, `level`
* If you plan to access the *instance variables*, the method names, should match them.

There is also other ways to access or even modify *instance variables* from within the class, for example to increase the pokemon level:

```ruby
class Pokemon
#...

  def increase_level
    @level += 1
  end
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.level
#=> 1
pokemon.increase_level
pokemon.level
#=> 2
```

But what if you want to change the trainer name for another one, let's say from `"Ash Ketchum"` to `"Gary Oak"`. One way to do it, would be:

```ruby
class Pokemon
#...

  def change_trainer(trainer)
    @trainer = trainer
  end
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.change_trainer("Gary Oak")
pokemon.trainer
#=> "Gary Oak"
```

Even though this would make sense and it actually works, when you want to change an *instance variable* value, the pattern is called a *setter*:

```ruby
class Pokemon
#...

  def trainer=(trainer)
    @trainer = trainer
  end
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.trainer = "Gary Oak"
```

As you may already been thinking, Ruby offers a solution for this, the same as `readers`:

```ruby
class Pokemon
  attr_reader :name, :trainer, :level
  attr_writer :level
  #...
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.trainer = "Gary Oak"
```

There is also another way to add both of type of patterns with a single method, such as `attr_writer` and `attr_reader`:

```ruby
class Pokemon
  attr_accessor :name, :trainer, :level
  #...
end

pokemon = Pokemon.new("Pikachu", "Ash Ketchum")
pokemon.trainer = "Gary Oak"
```

The `attr_accessor` would create both methods, the *reader* and *writer*. This is the preferred way in Ruby to create both patterns. You can read a bit
more on this [here](https://stackoverflow.com/questions/4370960/what-is-attr-accessor-in-ruby).

### Adding behavior & self

In order to add behavior to a class, as we stated before, is achievable through methods. In Ruby we have two types of methods that can be defined within a class. 

* **Instance methods** are the ones that work on instances of the class
* **Class methods** are the ones, that don't need an instance to work, such as `attr_reader` for example. This is not a very common pattern.

```ruby
class Pokemon
  attr_reader :name

  def initialize(name)
    @name = name
  end

  # This is an instance method
  def description
    "#{Pokemon.description} named #{@name}"
  end

  # The keyword 'self' refers to the class, in this case Pokemon
  def self.description
    "A Pokemon"
  end
end

pokemon = Pokemon.new("Pikachu")
pokemon.description
#=> "A Pokemon named Pikachu"

Pokemon.description
"A Pokemon"
```

As you can see, we have two methods named the "same", in this case `self.description` and `description`. But the difference here is the `self` keyword prefixed on the method. This
tells Ruby that this method acts at the class level, not te instance. Do not worry to much about this, it is just important you identify this.

`self` can also be used at the instance level:

```ruby
class Pokemon
  attr_reader :name, :level

  def initialize(name)
    @name = name
    @level = 1
  end

  def who_am_i
    puts self
  end

  def move
    if self.legs?
      walk
    else
      self.fly
    end
  end

  def walk
  end

  def fly
  end

  def legs?
    true
  end
end
```

You can use the keyword `self` to refer to the instance, so for example if you instantiated a Pokemon, you can either use self or not inside the instance
method, and it will work. In other programming languages, this is known as `this`.

You can actually use the methods created by the `attr_reader` inside any other method by using the `self` keyword or the *instance variable*. If you want to read more this,
[here](https://www.rubyguides.com/2020/04/self-in-ruby/) is a good article.

### Working with multiple classes

As our code grows, it is inevitable to interact with multiple classes, as well as the communication between them. Maybe is the need to have more layers of abstraction,
better test our code, follow a design principle or any othe reason.

Take for example the idea of having a `Trainer` class, because maybe just working with a name is just not enough:

```ruby
class Trainer
  attr_reader :pokemons, :name

  def initialize(name)
    @name = name
    @pokemons = []
  end

  def catch(pokemon)
    @pokemons << pokemon
  end
end
```

This way we are not longer working with a bare string, but a custom made `Trainer` class:

```ruby
trainer = Trainer.new("Ash Ketchum")
pokemon = Pokemon.new("Pikachu", trainer)

#read the trainer name
pokemon.trainer.name
#=> "Ash Ketchum"

#to catch a pokemon
trainer.catch(pokemon)
trainer.pokemons
#=> [#<Pokemon:0x00005560ecb20a00 @name="Pikachu", @trainer="Ash Ketchum", @level=1>]

```

This is a super simple example on how to work with two classes, but there are some techniques to identify layers of abtractions or responsibilities. For now
you don't have to worry, we will catch up in further lessons.

### Class Overwrite

There is a powerful feature in Ruby, that allows you to overwrite methods from an existing class. **This has to handle with care, as it may have unexpected side effects.**

```ruby
class Pokemon
  attr_reader :name, :level
  attr_accessor :trainer

  def initialize(name, trainer)
    @name = name
    @trainer = trainer
    @level = 1
  end

  def name
    "This overrides the method name created by attr_accessor or any other"
  end
end

pokemon = Pokemon.new("Pikachu", trainer) 
pokemon.name
#=> "This overrides the method name created by attr_accessor or any other"
```

The ability to do this is really powerful, as you can actually open standard classes and override a particular method, for example:

```ruby
class String

  def upcase
    "No Upcase support!"
  end
end

puts "Hello World".upcase
#=> "No Upcase suppoort!"
```

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/classes-and-instances`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)