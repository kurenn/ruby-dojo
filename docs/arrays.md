---
title: Arrays
date: 2021-04-15
slug: arrays
---

## What are you going to learn?

* Understand the most basic data collection in Ruby 
* Perform operations on data collections(arrays in this case)
* Be able to understand how Ruby Docs is organized and to search for methods 
* Correctly utilize the basic syntax for arrays in Ruby

## What are Arrays?

In simple words, an array is a collection of data:

* Separated by commas and enclosed by square brackets 
* Can store big amounts of data or `elements`, or can be empty 
* Can contain any type of data type (strings, integers, floats), even other arrays.
* Has no particular order

This is another data type in Ruby, and as mentioned in the last lesson, it is also an `instance` of the class 
[Array](https://ruby-doc.org/core-3.0.1/Array.html). We will keep reviewing these concepts as we move forward, just remember that everything in Ruby is an `instance` or `object` of a particular class.

Here is how arrays look in Ruby:

```ruby
[] #empty array
[1, 2, 3] #an array of numbers
["hello", "world"] #an array of strings
[3.14, 1.5] #an array of floats
[[], [1, 2], ["Hello"]] # an array of arrays
[1, "hello", [3.14]] #an array with multiple data types
```

Remember that Ruby comes with an interactive interpreter on which you can perform many of these operations, just open a `terminal` and type in `irb`. You will see something like:

```bash
irb(main):001:0> [1]
=> [1]
```

In there you can copy and paste some of the examples provided above. **Make sure you press the *Enter* key to execute**.

## Reading an Array

Now that you can create arrays, you probably wonder how to read an `element` from it. Take the next example for
getting "Yoda":

```ruby
jedis = ["Luke", "Yoda", "Obi-Wan"]
yoda = jedis[1]
```

As you can see we are reading the variable `jedis`, and we added this `[1]`, which means to access the `element` on position number 1. But wait, isn't it `Luke` the first Jedi?. The answer is not quite, as
something important about arrays is that **the index on which an array begins, is 0**, so:

```ruby
jedis = ["Luke", "Yoda", "Obi-Wan"]
jedis[0]
#=> "Luke"

jedis[1]
#=> "Yoda"

jedis[02]
#=> "Obi-Wan"
```

It may be akward at first, but you will get use to it, in no time. You can also use the [#at](https://ruby-doc.org/core-2.7.0/Array.html#method-i-at) method to get the same behavior, but it is not commonly use. We prefer the old good way.

*You can actually use negative values as indexes, go ahead, try it and see what happens*

## Modify an Element

You can also change the value of an element in the array:

```ruby
jedis = ["Luke", "Yoda", "Obi-Wan"]
jedis[0] = "Anakin"

jedis
#=> ["Anakin", "Yoda", "Obi-Wan"]
```

As you can see, just by assigning the value directly into the array index, you changed that `element` from "Luke" to "Anakin". Just be aware that any changes you perform, will persist and unable to be recovered.

## Add elements to an Array

Another way to modify an array is to adding or appending new elements:

```ruby
jedis = ["Luke", "Yoda", "Obi-Wan"]
jedis << "Windu"
#=> ["Luke", "Yoda", "Obi-Wan", "Windu"]

jedis.append("Rey")
#=> ["Luke", "Yoda", "Obi-Wan", "Windu", "Rey"]
```

We presented two ways to append elements to an array, the first is the prefered by the community, so from now on, we will go with that. Notice that you can append any other data type to the array, such as integers, arrays, floats, etc.

## Basic operations with Arrays

Just like performing sums and substractions, you can run some of these to arrays:

* The `+` operator will join two or more arrays
* The `-` will remove all elements that match from the array on the right

```ruby
["Luke", "Yoda"] + ["Obi-Wan"]
#=> ["Luke", "Yoda", "Obi-Wan"]

["Luke"] + ["Yoda"] + ["Obi-Wan"]
#=> ["Luke", "Yoda", "Obi-Wan"]

["Luke", "Yoda"] - ["Yoda"]
#=> ["Luke"]
```

We highly recommend you have a read on the official [documentation on arrays](https://ruby-doc.org/core-2.7.0/Array.html), try some of the methods Ruby already offers

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/arrays`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)