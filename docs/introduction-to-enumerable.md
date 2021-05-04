---
title: Introduction to Enumerable
date: 2021-04-15
slug: introduction-to-enumerable
---

## What are you going to learn?

* Understand the most common enumerable methods for collections - `map`, `reduce`, `select`, `find`
* Be able to understand how Ruby Docs is organized and to search for methods 
* Correctly utilize the basic syntax for enumerables in Ruby

## What is Enumerable?

Back in the [Introduction to Each](/introduction-to-each) lesson, we learn how to iterate over collections(arrays or hash) and also perform some operations on them, such as
value sum, filtering elements and so on. Due to this to the fact that this operations are very common when coding, Ruby comes with a built-in methods that help us to work
with the most common iteration patterns. This methods come from the [enumerable](https://ruby-doc.org/core-3.0.1/Enumerable.html) module. 

> "The Enumerable mixin provides collection classes with several traversal and searching methods, and with the ability to sort."

Before jumping into the enumerable methods, here is a reminder on how the `each` iterator looks:

```ruby
[1,2,3,4].each do |number|
 puts number
end
#=> [1,2,3,4]
```

*Note: Remember that the `each` iterator returns the original collection, not the actual process inside of it*

### map

By the official definition:

> Returns a new array with the results of running block once for every element in enum.

In other words, the difference between [map](https://ruby-doc.org/core-3.0.1/Enumerable.html#method-i-map) and [each](/introduction-to-each), is that the first one returns
whatever we do in the block.

Take the following code for example:

```ruby
numbers = [1,2,3,4,5]
squared_numbers = []

numbers.each do |number|
  squared_numbers << number**2
end

puts squared_numbers
#=> [1,4,9,16,25]
```

This is simpler with `map`:

```ruby
numbers = [1,2,3,4,5]

squared_numbers = numbers.map do |number|
  number**2
end

puts squared_numbers
#=> [1,4,9,16,25]
```

As you can see, we are no longer appending elements to the `squared_numbers`, instead of that, we are directly assigning it. So whenever you have this situation with `each`
where you are saving values into another empty collection, it's probably better if you go with a `map` enumerable.

### reduce

[Reduce](https://ruby-doc.org/core-3.0.1/Enumerable.html#method-i-reduce) will combine all of the elements on the collection and reduce it to a single result. A clear example
of this is to `reduce` the elements from an array by summing them all.

Take for example the following code with `each`:

```ruby
numbers = [1,2,3,4,5]
result = 0

numbers.each do |number|
  result = result + number
end

puts result
#=> 15
```

Now, with reduce:

```ruby
numbers = [1,2,3,4,5]
result = numbers.reduce(:+)

puts result
#=> 15
```

As you can see we are introducing what is called, **sugar syntax**, but you can also use the full version:

```ruby
numbers = [1,2,3,4,5]
result = numbers.reduce do |sum, number|
  sum + number
end

# This is the short version for the block, works exactly the same. This is preferred when only one operation in being performed
#result = numbers.reduce { |sum, number| sum + number }

puts result
#=> 15
```

We know, there could be a bit of confusion around this, but here are some other examples:

```ruby
# Multiply some numbers
(5..10).reduce(1, :*)                          #=> 151200

# find the longest word
longest = %w{ cat sheep bear }.reduce do |memo, word|
   memo.length > word.length ? memo : word
end

longest      
```

### find

As you can infer by the name, the [find](https://ruby-doc.org/core-3.0.1/Enumerable.html#method-i-find) method, helps you locate the first element which meet the search criteria.

Take for example the following code with `each`:

```ruby
numbers = [1,2,3,4,5]
even = nil

numbers.each do |number|
  return even = number if number.even?
end

puts even
#=> 2
```

Now, with `find`:

```ruby
numbers = [1,2,3,4,5]
even = numbers.find do |number|
  number.even?
end

puts even
#=> 2
```

As you can see, even though there are two numbers that meet the search criteria, `even?`, the method `find` will always return the first match.

### select

What if instead of just returning the first matching element you want all?, this is where [select](https://ruby-doc.org/core-3.0.1/Enumerable.html#method-i-select) comes into play. By the official
definition:

> Returns an array containing all elements of enum for which the given block returns a true value.

Take for example the following code with `each`:

```ruby
numbers = [1,2,3,4,5]
even_numbers = []

numbers.each do |number|
  even_numbers << number if number.even?
end

puts even_numbers
#=> [2,4]
```

This is how it looks with `select`:

```ruby
numbers = [1,2,3,4,5]

even_numbers = numbers.select do |number|
  number.even?
end

# This are two alternatives which works the same
# even_numbers = numbers.select { |number| number.even? }
# even_numbers = numbers.select(&:even?) What is this? => https://blog.pjam.me/posts/ruby-symbol-to-proc-the-short-version/

puts even_numbers
#=> [2,4]
```

### More enumerable methods

There are a lot more enumerables out there to achieve many tasks in simple steps. A good place to start searching are the [Ruby Docs](https://ruby-doc.org/core-3.0.1/Enumerable.html#method-i-select), but here is a list of what we think may come in handy someday:

* #reject
* #grep
* #include?
* #none?
* #one?
* #max
* #min
* #group_by

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/enumerables`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)