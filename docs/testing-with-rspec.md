---
title: Testing with RSpec
date: 2021-04-15
slug: testing-with-rspec
---

## What are you going to learn?

* Understand the basic implementation of a test
* Why it is important to test our code
* User RSpec to create automated tests

## Automated test

To talk about test automation, we necessarily need to understand [Test Driven Development](http://agiledata.org/essays/tdd.html) as a way to create tests. First we need
to define what test automation is:

> the use of software separate from the software being tested to control the execution of tests and the comparison of actual outcomes with predicted outcomes

Having automated tests on our code, we will:

* Be able to add or remove code with confidence
* Document our executing code through code.
* Keep happiness levels up for the developer
* Implement CI/CD strategies
* Integrate other team members with ease
* Increase the code catch up curve

## TDD

> Is an evolutionary approach to development which combines test-first development where you write a test before you write just enough production code to fulfill that test and refactoring

![TDD Cycle](/tdd.png)

This approach to create tests allow developers to keep things simple by taking small steps which lead to good code design as it enable and motivate us to refactor small portions of our code, while receiving inmediate feedback.

We have to be honest, this is not an easy practice to adopt, it may be frustating and time-consuming at first, but once you get to it, we truly believe the pay-off is worth it.
Some of the misconceptions of myths you may hear when using TDD area:

* **Test 100% of your code**. This isn't realistic for several reasons, for example, the team not having the enough skill, the user interface is not easy to test, maybe you are working with legacy code, that you are unable to add tests to. Is better to define a threshold for this, 70% of coverage for example.
* **Unit testing is enough**. Some may argue that by having all of our classes tested, it is enough. In reality this is not the case, for example, user interface testing may be a crucial component for your application to work, and it will make sense to add integration tests.
* **TDD does not scale**. As your code grows, your tests will take longer to execute, and while this is a fact, there are ways to improve the workflow, and almost remove thid concern. The fact that you have slow tests, may be an indication of code that is coupled and may be a good time to decouple.

## RSpec 

There are a bunch of tools to start testing our code, the two most common ones are [mini test](https://github.com/seattlerb/minitest) and [rspec](https://rspec.info/). In our case we are going to be using RSpec.

> RSpec is a 'Domain Specific Language' testing tool written in Ruby to test Ruby code. It is a behavior-driven development framework which is extensively used in the production applications

To install rspec, just fire up a terminal and type:

```bash
% gem install rspec
```

To start creating tests, we need to understand first the directory structure we will be creating.

### Directory structure

* All of our specs live under a `spec` directory
* The implementation code live under a `lib` directory
* The spec files should reflect the class being tested with `_spec` appended. Ex - `spec/class_to_test_spec.rb`
* Each spec file need to also require the `spec_helper.rb`. Ex. `require 'spec_helper'`
* In your test, you need to require the actual class. Ex. `require ./lib/class_to_test.rb`
* You can run your specs by executing - `rspec spec`. You can specify a single file - `rspec spec/class_to_test_spec.rb`
* You can init your project with the `rspec --init` command

This is how it looks in the wild:

```
├── lib
└── spec
    └── spec_helper.rb
```

### Writing a test

Assuming you have a directory structure, like the one describe above, this is how you would start creating a test. In this case we will be testing a sum class.

**The first step is to create the test file**

```ruby
#spec/sum_spec.rb

require 'spec_helper'
require 'sum'

describe Sum do 
  describe ".run" do
    it 'returns the 8 by sending 5 and 3' do
      result = Sum.run(5,3)

      expect(result).to eql 8
    end
  end
end
```

Here is a description of each part:

![Spec sample](/spec.png)
[Download](/spec.png)

Check more on expectations [here](https://rubydoc.info/gems/rspec-expectations). For matchers, click [here](https://relishapp.com/rspec/rspec-expectations/v/3-10/docs/built-in-matchers)

After writing our first test, **we need to see our test fail**. So by running `rspec spec/sum_spec.rb` you should see a similar output to this:

```bash
Failure/Error: require "./lib/sum"

LoadError:
  cannot load such file -- ./lib/sum
# ./spec/sum_spec.rb:2:in `<top (required)>'
No examples found.
```

This means that our test is failing, and it is because we don't have a sum file. Let's fix that, by running `touch lib/sum.rb`. Remember we are doing the bare minimum to see our test pass.

After creating the file, we run the spec again - `rspec spec/sum_spec.rb`

```bash
An error occurred while loading ./spec/sum_spec.rb.
Failure/Error:
  describe Sum do
    describe ".run" do
      it "returns 8, for 3 and 5 as paratemers" do
        result = Sum.run(3,5)

        expect(result).to eql 8
      end
    end
  end

NameError:
  uninitialized constant Sum
# ./spec/sum_spec.rb:4:in `<top (required)>'
No examples found.

Finished in 0.00002 seconds (files took 0.08941 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```

Now the error has changed, is telling us that the `Sum` constant does not exist, in this case, the class, let's fix that:

*lib/sum.rb*
```ruby
class Sum
end
```

Now, let's run the specs again - `rspec spec/sum_spec.rb` and analyze the output:

```bash
F

Failures:

  1) Sum.run returns 8, for 3 and 5 as paratemers
     Failure/Error: result = Sum.run(3,5)

     NoMethodError:
       undefined method `run' for Sum:Class
     # ./spec/sum_spec.rb:7:in `block (3 levels) in <top (required)>'

Finished in 0.00209 seconds (files took 0.07003 seconds to load)
1 example, 1 failure

Failed examples:

rspec ./spec/sum_spec.rb:6 # Sum.run returns 8, for 3 and 5 as paratemers
```

This seems a bit repetitive, but this is actually a real flow for TDD. As long as we receive different error messages, this means we are moving forward. In this case
the problem seems to be, that there is not a `run` method for the class.

*lib/sum.rb*
```ruby
class Sum
  def self.run(a, b)
  end
end
```

Now if we run the specs again - `rspec spec/sum_spec.rb`

```bash
F

Failures:

  1) Sum.run returns 8, for 3 and 5 as paratemers
     Failure/Error: expect(result).to eql 8

       expected: 8
            got: nil

       (compared using eql?)
     # ./spec/sum_spec.rb:9:in `block (3 levels) in <top (required)>'

Finished in 0.02419 seconds (files took 0.07023 seconds to load)
1 example, 1 failure

Failed examples:

rspec ./spec/sum_spec.rb:6 # Sum.run returns 8, for 3 and 5 as paratemers
```

Now we are playing, as you can see, it is actually executing the `run` method, but nothing is being performed. Let's fix that.

```ruby
class Sum
  def self.run(a, b)
    a + b
  end
end
```

Then if we run the tests again - `rspec spec/sum_spec.rb` 

```bash
.

Finished in 0.00549 seconds (files took 0.18275 seconds to load)
1 example, 0 failures
```

This is a passing test, the next step would be to refactor, but the code is already simple enough.

### Extending our test suite

Let's say there is a new requirement for our `Sum` class:

* The `run` method should be able to receive any number of arguments and the sum all of the them.

When receiving a new requirement, our suggestion would be to add one test at a time, and not a bunch of them, cause tackle them would be harder. Let's add our new test:

```ruby
#spec/sum_spec.rb

require 'spec_helper'
require 'sum'

describe Sum do 
  describe ".run" do
    it 'returns the 8 by sending 5 and 3' do
      result = Sum.run(5,3)

      expect(result).to eql 8
    end

    it 'returns 15 when sending 8, 2 and 5' do
      result = Sum.run(8, 2, 5)

      expect(result).to eql 15
    end
  end
end
```

Now, let's see how our test fail:

```bash
.F

Failures:

  1) Sum.run return 15 when sending 8, 2 and 5
     Failure/Error:
       def self.run(a, b)
         a + b
       end

     ArgumentError:
       wrong number of arguments (given 3, expected 2)
     # ./lib/sum.rb:2:in `run'
     # ./spec/sum_spec.rb:13:in `block (3 levels) in <top (required)>'

Finished in 0.00323 seconds (files took 0.06782 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/sum_spec.rb:12 # Sum.run return 15 when sending 8, 2 and 5
```

As you can see, the method is expecting 2 arguments, but we are actually sending 3. There are two ways to handle this:

1. Add a third argument with a default value of 0, so when calling with 2 arguments, the third one is not necessary. And although this may sound good
**would you handle it the same with a fourth argument?**.
2. There is a way for Ruby to call a method with any number of arguments, or even none. `*args` is an array with all the values passed to it.

An implementation for the `run` method would be:

```ruby
class Sum
  def self.run(*args)
    args.reduce(:+)
  end
end
```

Then if we run the tests - `rspec spec/sum_spec.rb`

```bash
..

Finished in 0.00324 seconds (files took 0.06835 seconds to load)
2 examples, 0 failures
```

We are done, but you may want to add some other tests, for example with negative numbers, or decimals. But we'll leave that to the exercises.

## Four-Phase Test

There is a testing pattern called [Four-Phase Test](http://xunitpatterns.com/Four%20Phase%20Test.html), it applies to all programming languages and unit tests.

This is how the four phases look in pseudo-code for each test case:

```ruby
it do
  setup
  exercise
  verify
  teardown
end
```

This four steps are executed sequentally.

### Setup

On this step, is where you create your instances, or any other task needed for the rest of the spec. For example:

```ruby
it do
  pokemon = Pokemon.new("Pikachu")
  exercise
  verify
  teardown
end
```

### Exercise

This step is where you execute the method, to get a result and use it later

```ruby
it do
  pokemon = Pokemon.new("Pikachu")

  pokemon.level_up
  teardown
end
```

### Verify

During verification, is where the expectation happen

```ruby
it do
  pokemon = Pokemon.new("Pikachu", 1)

  pokemon.level_up

  expec(pokemon.level).to eql 2
  teardown
end
```

### Teardown

This is the step where the cleanup and memory release happens. It is often handle by the same language. In Ruby you don't have to worry abnout this step.

## Before we leave

There is a lot of information out there about testing, good practices and much more. We just covered the very basics, and testing along with TDD is mastered through
practice, but first, here are some advices:

* You have to see testing as part of your development cycle, not extra steps.
* Testing is an investment on yourself and the team working on the code base. We cannot stress enough how important this is.
* Tests are a way to document your code while providing confidence to the whole team. You included.
* Each test runs independent from others. **Avoid** tests to run in order on how they were written
* Your tests will (almost)always return `E for Errors`, `F for failure` and `.` for passing.

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can find them under `/ruby-exercises/Module1/testing-with-rspec`.

## Additional Resources


+ [Four-Phase Test](https://thoughtbot.com/blog/four-phase-test)
+ [Test Driven Development](http://www.jamesshore.com/Agile-Book/test_driven_development.html)
+ [Martin Fowler TDD](http://martinfowler.com/bliki/TestDrivenDevelopment.html)
+ [Better Specs](http://betterspecs.org/)
+ [Test First](http://www.extremeprogramming.org/rules/testfirst.html)
+ [Ruby’s Powerful Method Arguments & How To Use Them Correctly](https://www.rubyguides.com/2018/06/rubys-method-arguments/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)