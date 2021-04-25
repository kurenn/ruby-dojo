---
title: Hashes
date: 2021-04-15
slug: hashes
---

## What are you going to learn?

* Understand the most used data collection in Ruby 
* Perform operations on data collections(hashes in this case)
* Be able to understand how Ruby Docs is organized and to search for methods 
* Correctly utilize the basic syntax for hashes in Ruby

## What is a Hash?

In simple words, a hash is a collection of key-value pairs:

* Represents a collection of named values
* Separated by commas and enclosed by curly braces
* Can store big amounts of data or `elements`, or can be empty 
* Can contain any type of data type (strings, integers, floats), even other hashes.
* Has no particular order
* The keys from the hash are almost always known

As you can see it has some similarities with an Array, but:

* An Array index is always an Integer.
* A Hash key can be (almost) any object.

This is another data type in Ruby, and as mentioned in the last lesson, it is also an `instance` of the class 
[Hash](https://ruby-doc.org/core-3.0.1/Hash.html). If you come from other programming language, this is also known as dictionaries of data.

Here is how hashes look in Ruby:

```ruby
{} #empty hash

{
  name: "Abraham",
  last_name: "Kuri"
} #a hash with data

{
  "one": 1,
  "two": 2
}

{
  address: {
    name: "Elm street",
    number: "101"
  }
}
```

As you can see, we are using strings and [symbols](https://ruby-doc.org/core-2.5.0/Symbol.html). The last ones are the most
common use to define key names on a hash, but in some cases you will see strings

> Symbol objects represent names and some strings inside the Ruby interpreter.

Remember that you can test the examples above with the interpreter, just open a `terminal` and type in `irb`. You will see something like:

```bash
irb(main):003:0> { name: "Alice" }
=> {:name=>"Alice"}
```

Another way to create a Hash, is by simply using the Class itself:

```ruby
Hash.new
```

And when using this syntax, you are able pass a value, for the value of any key is that:

```ruby
Hash.new(0)
```


## Reading a Hash

Now that you can create hashes, you probably wonder how to read the data from it. Take the next example for
getting the name of the `heroe`:

```ruby
heroe = { 
  name: "Iron Man", 
  powers: ["Millionare", "Weapons", "Smart"]
}

heroe[:name]
#=> "Iron Man"

heroe[:powers]
#=> ["Millionare", "Weapons", "Smart"]
```

As you can see we are reading the variable `heroe`, and we added this `[:name]`. So to access the value `"Iron Man"` you need to use the [symbol](https://ruby-doc.org/core-2.5.0/Symbol.html) `:name` as the index.

Hash are a great way to provide a bit more context to the code, as provides a more descriptive alternative to store data.

*What is returned if you try to access a key that does not exist on the hash?. Go ahead, try it.*

## Modify a value fro the Hash

You can also change the value of value in the hash:

```ruby
heroe = { 
  name: "Captain America", 
  powers: ["Shield", "Super strong", "Super fast"]
}

heroe[:name] = "Spider Man"
#=> {:name=>"Spider Man", powers: ["Shield", "Super strong", "Super fast"]}
```

As you can see, just by assigning the value directly into the hash key, you changed that `value` from "Captain America" to "Spider Man". Just be aware that any changes you perform, will persist and unable to be recovered.

## Add a key-value pair

Another way to modify a hash is to add new keys-value pairs:

```ruby
heroe = { 
  name: "Captain America", 
  powers: ["Shield", "Super strong", "Super fast"]
}

heroe[:real_name] = "Steve Rogers"
heroe[:height] = 182.4
#=> {:name=>"Captain America", powers: ["Shield", "Super strong", "Super fast"], real_name: "Steve Rogers", height: 182.4}
```

As you can see it is really easy to add new key-value pairs into the hash. Notice that you can save any data type as the value.

## Basic operations with Hashes

Even though you can perform basic operations with arrays, this is not the case for a hash. For example to join two hash instances, there is method called [merge](https://ruby-doc.org/core-3.0.1/Hash.html#method-i-merge):

* Returns the new Hash formed by merging each of other_hashes into a copy of self.
* Each argument in other_hashes must be a Hash.
* Each new-key entry is added at the end.
* Each duplicate-key entry’s value overwrites the previous value.

```ruby
basic_heroe_description = { 
  name: "Captain America", 
  powers: ["Shield", "Super strong", "Super fast"]
}

extra_heroe_details = {
  real_name: "Steve Rogers",
  height: 182.4
}

basic_heroe_description.merge(extra_heroe_details)
#=> {:name=>"Captain America", powers: ["Shield", "Super strong", "Super fast"], real_name: "Steve Rogers", height: 182.4}
```

*The `merge` method can receive as many other hash objects as you need.*

### Deleting a member from the Hash

It is not as common as you may thinkg, but you can also delete a key-value or member of the hash by using the [delete]https://ruby-doc.org/core-3.0.1/Hash.html#method-i-delete) method:

```ruby
heroe = {
  name: "Captain America", 
  powers: ["Shield", "Super strong", "Super fast"]
}

heroe.delete(:powers)
#=> ["Shield", "Super strong", "Super fast"]

heroe
#=> {name: "Captain America"}
```

We highly recommend you have a read on the official [documentation on hash](https://ruby-doc.org/core-3.0.1/Hash.html), try some of the methods Ruby already offers

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/hash`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)