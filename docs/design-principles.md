---
title: Design Principles
date: 2021-04-15
slug: design-principles
---

## What are you going to learn?

* Understand what and when to apply the SOLID principles
* Understand and apply the Law of Demeter
* Advantages of having clean code and keep development happiness 

![Development Happiness](/dev-happiness.jpeg)

## SOLID Principles

When it comes to build high quality code under the object oriented paradigm, the [SOLID principles](https://en.wikipedia.org/wiki/SOLID) are the way to go. The intention behind this
is to make software designs more readable, understandable and maintainable.

It stands for:

* **S**: Single-Responsibility Principle
* **O**: Open-Close Principle
* **L**: Liskov Substitution Principle
* **I**: Interface Segregation Principle
* **D**: Dependency Inversion Principle

### Single Responsibility Principle

> There should never be more than one reason for a class to change. In other words, every class should have only one responsibility.

Take the code below for example:

```ruby
class ZipCodeService
  def initialize(environment = 'development')
    @env = environment
  end

  def zip_codes
    url = 'http://sepomex.icalialabs.com/api/v1/zip_codes'
    url = 'https://myreal.server.com' if env == 'production'

    puts "[ZipCodeCollection] GET #{url}"
    response = Net::HTTP.get_response(URI(url))

    return [] if response.code != '200'

    zip_codes = JSON.parse(response.body)['zip_codes']
    zip_codes.map do |params|
      ZipCode.new(
        id: params['id'],
        code: params['d_codigo'],
        city: params['d_ciudad'],
        state: params['d_estado']
      )
    end
  end

  private

  attr_reader :env
end

class ZipCode
  attr_reader :id, :code, :city, :state

  def initialize(id:, code:, city:, state:)
    @id = id
    @code = code 
    @city = city
    @state = state
  end

  def to_s
    "#{id} #{city}"
  end
end

zip_code_service = ZipCodeService.new
puts zip_code_service.zip_codes
```

The class above is in charge of:

1. Configuring an environment
2. Logging the data to the console
3. Handling the request
4. Serializing the actual response

By following this principle, we probably need to create 4 abstractions or more for this.

We put together a video which you can find [here](https://youtu.be/f7t4yUE9fFI) on how to refactor this using the Single Responsibility Principle. And the final code is [here](https://github.com/kurenn/SOLID_examples/blob/refactor/single_responsibility/single_responsibility_principle.rb)

## Open-Close Principle

> Software entities ... should be open for extension, but closed for modification.

Take the code below for example:

```ruby
class Payment
  def initialize(payment_gateway)
    @payment_gateway = payment_gateway
  end

  def perform
    case @payment_gateway
    when 'conekta'
      gateway = PaymentGateway::Conekta.new
      gateway.prepare
      gateway.pay
    when 'paypal'
      gateway = PaymentGateway::Paypal.new
      gateway.prepare
      gateway.pay
    when 'mercado_pago'
      gateway = PaymentGateway::MercadoPago.new
      gateway.prepare
      gateway.pay
    end
  end
end
```

The class above is in charge of performing the charge using a particular gateway. Even this only has a single responsibility, the open close principle is being broken. Let's say
we need to add another gateway, `Stripe` for example, we will need to modify the behavior from the `perform` method in order to add the new class. 

We put together a video which you can find [here](https://youtu.be/L0dOQ0ik3_U) on how to refactor this using the Open-Close Principle. And the final code is [here](https://github.com/kurenn/SOLID_examples/blob/refactor/open_close/open_close_principle.rb)

## Liskov Substitution Principle

> If S is a subtype of T, then objects of type T may be replaced with objects of type S 
> (i.e. an object of type T may be substituted with any object of a subtype S) without
> altering any of the desirable properties of the program 

Take the code below for example:

```ruby
class Mammal
  def eat
  end

  def walk
  end
end

class Lion < Mammal
end

class Monkey < Mammal
end

def migrate
  mammals.each do |mammal|
    mammal.walk
  end
end
```

We put together a video which you can find [here]() on how to refactor this using the Liskovb Substitution Principle. And the final code is [here](https://github.com/kurenn/SOLID_examples/blob/refactor/liskov_substitution_principle/liskov_substitution_principle.rb)

## Interface Segregation Principle

> Many client-specific interfaces are better than one general-purpose interface

Take the code below for example:

```ruby
# No client should be forced to depend on methods it does not use.

class Motorcycle
  def change_oil
  end

  def charge_battery
  end

  def drive
  end

  def adjust_mirror
  end
end

class Rider
end

class Mechanic
end
```

We are including a video that explains this better in [here](https://www.youtube.com/watch?v=Ye1h3zKl1lg). And the final code is [here](https://github.com/kurenn/SOLID_examples/blob/interface-segregation-modules/interface_segregation.rb)

## Dependency Inversion Principle

> High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).

Take the code below for example:

```ruby
class Server
  def create
    Heroku.create
  end
end
```

We put together a video which you can find [here](https://youtu.be/8fFhdZHryb0) on how to refactor this using the Interface Segregation Principle. And the final code is [here](https://github.com/kurenn/SOLID_examples/blob/interface-segregation-modules/interface_segregation.rb)

## Law of Demeter

The Law of Demeter (LoD) or principle of least knowledge is a design guideline for developing software, particularly object-oriented programs. In its general form, the LoD is a specific case of loose coupling. The guideline was proposed at Northeastern University towards the end of 1987, and can be succinctly summarized in each of the following ways:[1]

* Each unit should have only limited knowledge about other units: only units "closely" related to the current unit.
* Each unit should only talk to its friends; don't talk to strangers.
* Only talk to your immediate friends.

The fundamental notion is that a given object should assume as little as possible about the structure or properties of anything else (including its subcomponents), in accordance with the principle of "information hiding".

Take the example below:

```ruby
class Article
  def initialize(author)
    @author = author
  end
end

class Author
  def initialize(name)
    @name = name
  end
end

author = Author.new("Richard Feynman")
article = Article.new(author)

# To get the author name, we would do something like:
article.author.name
```

The code above is breaking the `Law of Demeter` as the article instance is accessing the author name to get the name, which in this case could and should be delegated.

```ruby
class Article
  def initialize(author)
    @author = author
  end

  def author_name
    @author.name
  end
end

class Author
  def initialize(name)
    @name = name
  end
end

author = Author.new("Richard Feynman")
article = Article.new(author)

# To get the author name, we would do something like:
article.author_name
```

The article is no longer assuming a property from the author, it is encapsulated on a method. Here is a more complete example on this - https://github.com/hackerschoolmty/rails-patterns#law-of-demeter

## Full examples

We put together a repository with all of the code above, where you can find the examples and their solutions.

The repo is this -> https://github.com/kurenn/SOLID_examples

## Additional Resources

+ [Ruby Midwest 2011 - Keynote: Architecture the Lost Years by Robert Martin](https://www.youtube.com/watch?v=WpkDN78P884)
+ [Rails Conf 2013 The Magic Tricks of Testing by Sandi Metz](https://www.youtube.com/watch?v=URSWYvyc42M)
+ [SOLID Principles](https://rubygarage.org/blog/solid-principles-of-ood)
+ [Ruby Docs](https://www.ruby-doc.org/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)