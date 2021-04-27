---
title: "Introduction to #each"
date: 2021-04-15
slug: introduction-to-each
---

## What are you going to learn?

* Understand the #each operator to navigate collections
* Modify collection data through an #each iterator
* Be able to understand how Ruby Docs is organized and to search for methods 
* Correctly utilize the basic syntax for #each in Ruby

## each

`#each` is an iterator:

* Supported by all collections in Ruby.
* It returns all the elements of an `array` or `hash`.
* Will allow us to perform anything for every element of the collection

As you may recall from the [arrays](arrays) and (hash)[hashes] lessons, these are collections
that can be accessed through an index value:

```ruby
fruits = ["Apple", "Banana", "Kiwi"]
fruits[0] #=> "Apple"
fruits[1] #=> "Banana"
fruits[2] #=> "Kiwi"

heroe = {
  name: "Batman",
  real_name: "Bruce Wayne",
  powers: ["Cool gadgets", "shadows", "ninja skills"]
}

heroe[:name] #=> "Batman"
heroe[:powers] #=> ["Cool gadgets", "shadows", "ninja skills"]
```

A simple implementation for an `array` to print every element would be:

```ruby
fruits = ["Apple", "Banana", "Kiwi"]

fruits.each do |fruit|
  puts fruit
end
```

As you can see `.each` is a method called on the `fruits` array.

Everything we do inside the `do` and `end` is called **Block**. Whatever is insde the block, will execute the number of elements on the `array`. `fruit` is the **block** variable
that will store the current `element` we are iterating over.

A simple implementation for a `hash` to print every pair would be:

```ruby
heroe = {
  name: "Batman",
  real_name: "Bruce Wayne",
  powers: ["Cool gadgets", "shadows", "ninja skills"]
}

heroe.each do |key, value|
  puts "#{key}: #{value}"
end
```

This time `each` method is called on the `heroe` hash. 

A subtle difference with the array, is that instead of having one **block variable** you have two, in this case one for the `key` and one for the `value`.

**Rmember that #each will always return the whole collection by the end of the execution**

The usage of `#each` is on of the most common ways to iterate a collection, and certainly the preferred one by the Ruby community. It is used to
modify elements from the collection, locate them, remove them, or even create new stuff. Take in account that there are many other methods to manipulate 
collections, such as `find`, `select`, `keep_if`, `map`, but don't worry for now, we will cover them in later lessons.
### Modify every element

There will be times when you want to modify each element from a collection, and `each` is certainly a way to do it. Take the next code for example:

```ruby
fruits = ["apPle", "BanAna", "KiwI"]

fruits.each do |fruit|
  fruit.downcase
end
```

* What would be the expected output?
* Is what you expected?

A common mistake when working with `#each` is that while iterating and having access to the current value, you may think that by applying some method or operation
on the element, it will persist the changes, but this is not the case. Why?, well because the **block variables** are that, variables that are storing the current value,
but not actually performing it on the element, therefor any modification will happen to the variable, not the element. For example:

```ruby
fruits = ["apPle", "BanAna", "KiwI"]
fruit = fruits[0]
fruit.downcase
# The fruit variable stores what is on the 0 position from the array
# in this case "apPle", and anything you perform on it will only
# affect the fruit variable
```

In order to get the output you expected, you may want to do something like:

```ruby
fruits = ["apPle", "BanAna", "KiwI"]
downcased_fruits = []

fruits.each do |fruit|
  downcased_fruits << fruit.downcase
end

puts downcase_fruits
#=> ["apple", "banana", "kiwi"]
```

### Create a new collection

Maybe you wan to create a more readable type of collection, such as a `hash` from a given `array`:

```ruby
fruits = ["apple", "banana", "kiwi"]
fruits_collection = []

fruits.each do |fruit|
  fruits_collection << {
    name: fruit
    quantity: rand(10)
  }
end

puts fruits_collection
```

In the example above we created a new `array` full of `hash` objects, to have a more descriptive data set, and we achieve this
by iterating the `fruits` array and even complementing with a bit more data, in this case the `quantity`.

Here is another example with a conditional:

```ruby
numbers = [1,2,3,4,5,6,7,8,9,10]
even_numbers = []

numbers.each do |number|
  if number.even?
    even_numbers << number
  end
end

puts even_numbers
```

**Make sure you test the examples above in your terminal**

### Calculate a result

Instead of creating new collection, sometimes you may want to perform a calculation using a collection and `#each`:

```ruby
numbers = [1,2,3,4,5,6,7,8,9,10]

sum = 0

numbers.each do |number|
  sum = sum + number
  #sum += number - this also works
end

puts sum
```

As you can see, we take an `array` of numbers, iterate and sum each `element` to get a whole new value. Obviously this is no the only way to achieve this, and certainly
the examples presented are not the only usages of `#each`. You can read more [here](https://mixandgo.com/learn/how-to-use-ruby-each).

### Sugar syntax

As we progress and you become more expert on Ruby, you will find that it offers many handy short versions when calling a method, specially the ones with blocks. Take for example:

```ruby
array.each do |element|
  puts element
end
```

You could rewrite this to:

```ruby
array.each { |element| puts element }
```

This is highly use in Ruby, but be aware that this is only recommended when the block of code is very short or that can be executed with one line. Otherwise **avoid** using this
if you are performing more than one action on the block.

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/each`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)