---
title: Nested Collections
date: 2021-04-15
slug: nested-collections
---

## What are you going to learn?

* Understand how to read and manipulate nested collections
* Perform operations on nested data collections

Before jumping into the lesson, you may want to read the [arrays](/arrays) and [hash](/hashes) content.
## Arrays in Arrays

As we move forward with software, you may find that data collections start to become a little bit more complex, whether the ones we
create or third-party.

Take this simple example, and try to answer the following:

```ruby
nested_collection = [["burguer", "pizza", "hot dog"], ["coke", "water", "beer"]]
```

* What would the method `count` on `nested_collection` return?
* How would you access the `pizza` element?
* How would you add another set of data?

## Hash in Arrays

```ruby
heroes = [{name: "Iron Man"}, {name: "Hulk"}]
```

* What would the method `count` on `heroes` return?
* How would you access the `Hulk` element?
* How would you change `"Iron Man"` to `"Black Widow"`?

## Hash with Arrays

```ruby
chinese_food_box = {
  base: ["rice", "wheat noodles", "egg noodles"],
  protein: ["fish", "shrimp", "beef", "chicken"],
  toppings: ["onion", "brocoli", "peanuts", "carrot", "potato"],
  sauce: ["teriyaki", "spicy", "cilantro", "soy sauce"]
}
```

* What would the method `count` on `chinese_food_box` return?
* How would you add a `extras` key-pair to the `chinese_food_box` collection?
* How would you access the `peanuts` element?
* How would you add another topping?

## Hash with Hash

```ruby
pokemons = {
  pikachu: { level: 32, attacks: ["thundershock", "thunder punch", "surf"] },
  charizard: { level: 54, attacks: ["dragon claw", "blast burn", "firespin"] },
  gengar: { level: 42, attacks: ["shadow claw", "sludge bomb", "shadow ball"] },
}
```

* What would the method `count` on `pokemons` return?
* How would you add an `attribute` key-pair to each pokemon element?
* How would you access the `charizard` element attacks?
* How would you add another another attack for any pokemon?

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/nested-collections`.

## Additional Resources

+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)