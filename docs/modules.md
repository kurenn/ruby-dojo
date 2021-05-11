---
title: Modules
date: 2021-04-15
slug: modules
---

## What are you going to learn?

* Understand what is a Module
* Understand the usages of a Module and when to apply it
* Refactor our code by mixin in modules

## What is a Module?

In simple words, a Module is:

* Only stores behavior, not state
* A group of methods that can be shared across multiple classes
* Cannot be instantiate it, just as classes
* Even though you cannot create instances, Modules can live and run by themselves
* Also use as a way to encapsulate and isolate classes. To give a Namespace

> Instance methods appear as methods in a class when the module is included, module methods do not. Conversely, module methods may be called without creating an encapsulating object, while instance methods may not.

This is how a [Module](https://ruby-doc.org/core-2.5.0/Module.html) looks like:

```ruby
module ModuleName
end
```

Keep in mind a couple of things here:

* All module names in Ruby are written in [camel case](https://en.wikipedia.org/wiki/Camel_case)
* The definition starts with the `module` word
* They are commonly created in separated files, with the name of the class in [snake case](https://en.wikipedia.org/wiki/Snake_case). 

Now that you have defined your module, let's start using it:

```ruby
module Area
  # This is how you define constants in Ruby
  PI = 3.1416

  def self.square(n)
    n * n
  end

  def self.rectangle(a,b)
    a * b
  end

  def self.circle(r)
    PI * r * r
  end
end

Area.square(10) #=> 100
Area.rectangle(10, 5) #=> 50
Area.circle(5) #=> 78.54
```

As you can see, this methods are defined and behave just like in class methods. But this is just the tipo of the iceberg, let's see another examples.

Let's say you have the following classes:

```ruby
class Image
  def share_on_facebook
  end
end

class Post
  def share_on_facebook
  end
end

class Tweet
  def share_on_facebook
  end
end
```

Maybe if you come from a technical background, one possible alternative that may cross your mind is [inheritance](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)), but we are taking another path, **Modules**.

In `shareable.rb`

```ruby
module Shareable
  def share_on_facebook
  end
end
```

In `image.rb`

```ruby
require 'shareable'

class Image
  include Shareable
end
```

In `post.rb`

```ruby
require 'shareable'

class Post
  include Shareable
end
```

In `tweet.rb`

```ruby
require 'shareable'

class Tweet
  include Shareable
end
```

The code above shows how to **mixin** all of the methods from the `Shareable` module into multiple classes. Take this into consideration:

* You can access properties on objects from host class. In other words, you can access *instance variables* from the module, and once they are included into
the class, it will behave as you expected.
* The `include` directive will inject the methods as instance methods.

### Extending a Class

What we mean by extending a class, is to add methods, just like above, but instead of instance methods, add them as class methods. 

```ruby
class Tweet
  extend Searchable
end

module Searchable
  def find_all_from(user)
    # some code implementation
  end
end
```

As you can see instead of using the keyword `include`, we are using `extend`, this will inject the methods to the class level so the invokation would be:

```ruby
Tweet.find_all_from("kurenn")
```

![Modules Usages](/modules.jpeg)
## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/modules`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)