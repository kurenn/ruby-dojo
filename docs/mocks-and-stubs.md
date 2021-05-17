---
title: Mocks & Stubs
date: 2021-04-15
slug: mocks-and-stubs
---

## What are you going to learn?

* Usage of `doubles` while testing with RSpec
* Understand what mock and stubbing is
* When to use mocks and stubs and why

## Test Doubles

Mocks and stubs are both types of test doubles. But wait, what is a test double?

> Test Double is a generic term for any case where you replace a production object for testing purposes.

What [Martin Fowler](https://martinfowler.com/bliki/TestDouble.html) meant by that is that sometimes while writing tests, we don't actually want to 
call for example a Payment Gateway API method, such as [Stripe](https://www.stripe.com), for three simple reasons:

* It will slow down the test suite quite a lot
* We are depending on the service to be up and ready to accept requests
* The third-party service may block our requests, as they would consider it an attack, due to the fcat that each time you run your tests, you are hitting the server.

This also applies when working with a database, and other layers of abstraction such as [Active Record](https://guides.rubyonrails.org/active_record_basics.html). How?, you may be
wondering. Well sometimes loading the whole class that works with ActiveRecord, for example a `User` class, is a bit overload for certain test cases, as you just need something
that behaves the same, but don't actually run all of the baggage.

Just for clarity, here is a possible implementation of double with RSpec:

```ruby
it 'returns the first_name and last_name from the user' do
  user = instance_double(User, first_name: "Luke", last_name: "Skywalker")

  user_full_name = user.full_name

  expect(user_full_name).to eql "Luke Skywalker"
end
```

This a super simple example on how this works, but as you can see we are using `instance_double` from RSpec - https://relishapp.com/rspec/rspec-mocks/docs - to mimic the behavior
from the user class, but not actually loading. This has huge performance improvements if you have a big test suite.

Don't worry if you don't get it right away, we will try to provide a bit more examples later on the lesson.

## Stubs

By the definition from Marin Fowler:

> Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.

Or in our own words, is an object that holds in most cases hardcoded data, and it is use to get the program behave the way we want in order to test it.

Take the next code for example:

```ruby
#payment.rb
class Payment
  def initialize(amount, gateway)
    @amount = amount
    @gateway = gateway
  end

  def perform
    @gateway.process_payment(@amount)
  end
end

#payment_gateway.rb
class PaymentGateway
  def process_payment(amount)
    # THIS IS THE API CALL, OR SOMETHING LIKE THAT
    response = true

    response && { id: rand(1000) }
  end
end

#payment_spec.rb
describe Payment do
  it "process the payment" do
    payment_gateway = instance_double(PaymentGateway)
    allow(payment_gateway).to receive(:process_payment).with(1000).and_return(id: 1234) # THIS IS WHERE WE ARE STUBBING THE OBJECT WITH SOME DATA AND HARDCODING THE OUTPUT
    payment = Payment.new(1000, payment_gateway)

    response = payment.perform

    expect(response).to eql id: 1234
  end
end
```

As you can see from the code above we are using `doubles` with:

```ruby
payment_gateway = instance_double(PaymentGateway)
```

and the actual stub is the next line:

```ruby
allow(payment_gateway).to receive(:process_payment).and_return(id: 1234)
```

This line tells RSpec to return `id: 1234` whenever the instance tries to call the `process_payment` it will be stubbed by this, and return what we want for us
to complete the test.

Here you can read more on method stubs - https://relishapp.com/rspec/rspec-mocks/v/2-99/docs/method-stubs

## Mocks

By the definition from Marin Fowler:

> Mocks are pre-programmed with expectations which form a specification of the calls they are expected to receive.

In simpler words, a Mock is a fake object in charge of listening to the methods called on the mock object or to **ensure that the right methods are being called.**

A difference between this and stubs, is that mocks helps you work with outputs and stubs with inputs.

```ruby
#order.rb
class Order
  def save(gateway_payment_id:)
    @gateway_payment_id = gateway_payment_id
  end
end

#payment.rb
class Payment
  attr_reader :order

  def initialize(amount, gateway)
    @amount = amount
    @gateway = gateway
    @order = Order.new
  end

  def perform
    response = @gateway.process_payment(@amount)
    @order.save(gateway_payment_id: response[:id])
  end
end

#payment_gateway.rb
class PaymentGateway
  def process_payment(amount)
    # THIS IS THE API CALL, OR SOMETHING LIKE THAT
    response = true

    response && { id: rand(1000) }
  end
end

#payment_spec.rb
describe Payment do
  it "ensures the order method is being called" do
    payment_gateway = instance_double(PaymentGateway)
    allow(payment_gateway).to receive(:process_payment).with(1000).and_return(id: 1234) # THIS IS WHERE WE ARE STUBBING THE OBJECT WITH SOME DATA AND HARDCODING THE OUTPUT
    payment = Payment.new(1000, payment_gateway)

    # We need to call this in here to make sure the save method is being called and mock when the payment.perform method is run.
    expect(payment.order).to receive(:save).with(gateway_payment_id: 1234)

    payment.perform
  end
end
```

As you can see from the code above, the expectation for this test is to ensure a method is being called, in this case the `save` method, when the `perform` is invoked. Our tests
should be passing by now.

```bash
..

Finished in 0.01542 seconds (files took 0.20936 seconds to load)
2 examples, 0 failures
```

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can find them under `/ruby-exercises/Module1/stubs-and-mocks`.

## Additional Resources

+ [RSpec Mocks](https://relishapp.com/rspec/rspec-mocks/docs)
+ [TestDouble](https://martinfowler.com/bliki/TestDouble.html)
+ [Test Doubles](https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da)
+ [Fundamentals of TDD](https://thoughtbot.com/upcase/fundamentals-of-tdd)
+ [Mocks & Stubs Plain English](https://www.codewithjason.com/rspec-mocks-stubs-plain-english/)
+ [Four-Phase Test](https://thoughtbot.com/blog/four-phase-test)
+ [Test Driven Development](http://www.jamesshore.com/Agile-Book/test_driven_development.html)
+ [Martin Fowler TDD](http://martinfowler.com/bliki/TestDrivenDevelopment.html)
+ [Better Specs](http://betterspecs.org/)
+ [Test First](http://www.extremeprogramming.org/rules/testfirst.html)
+ [Ruby’s Powerful Method Arguments & How To Use Them Correctly](https://www.rubyguides.com/2018/06/rubys-method-arguments/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)