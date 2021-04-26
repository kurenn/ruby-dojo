---
title: Flow Control
date: 2021-04-15
slug: flow-control
---

When programming you will find that you can achieve the same results by taking different paths, whether for performance purposes or readability. 

In this lesson we will try to make a piece of code to behave on different ways, depending on a or multiple **conditions**

## What are you going to learn?

* Understand how to define multiple flows within a piece of code
* Usage of `if` and `unless` to control flow
* Handle multiple decision branches with `else` and `elsif`
* Use `while`, `times` and `until` to repeat steps
* Understand what an infinite loop
* Correctly utilize the basic syntax for flow controle mechanisms in Ruby

Remember that the alternatives we publish here, are just that, you may find better or worse paths to get the same result. We will try to show you
which tools are commonly better for the job, or at least are more commonly used.

## What is a condition?

A condition is a statement that may be `true` or `false`, and in programming this is called a [Boolean](https://en.wikipedia.org/wiki/Boolean_data_type).

A boolean is another data type inside Ruby, therefore you can assign variables to `true` of `false`.

```ruby
play_music = false
# => false

grill_chicken = true
```

By their own, there is nothing much you can do, but we will use the condition evaluation by comparing booleans.

There are a bunch of comparison operators you can work with:

* `==` is used to check if two statements are the same
* `>` as in math, is used to check if the left value is greater than the one on the right
* `<` is used to check if the left value is less than the one on the right
* `>=` and `<=` if to check for values to be greater/less or equal to the second one
* `!=` is used to check if two statements are different

Here are a bunch examples:

```ruby
sun == hot
#=> true

3 > 5
#=> false

3.14 <= 144
#=> true

hot != cold
#=> true
```

There is also a way to inverse a boolean, just preppend the `!` operator to the statement. By doing this you will always get the opposite boolean
value. There is an `alias` for this by using the word `not` instead of the `!`, but this is only for Ruby. We prefer the `!` operator, as it is more
widely use across other languages.

```ruby
!true
#=> false

hot_outside = true
!hot_outside
#=> false

not hot_outside
#=> false
```

There is also a bunch of built-in methods in Ruby that evaluate into booleans:

```ruby
["Tarzan", "Hercules", "Frozen"].empty?
#=> false

1.odd?
#=> true

["Tarzan", "Hercules", "Frozen"].include? "Moana"
#=> false

{
  name: "Tarzan",
  release_date: Date.new(1999, 7, 16)
}.has_key? :name
#=> true
```

As you may already notice, all of this methods end with a `?` sign, which it is just a standard un Ruby to visually define the methods return a boolean, and even though
we highly recommend this, you are not obligated to do it.

### If statement

Let's start playing with some decisions in our code. A simple `if` statement will create two possible ways the code can follow.

A example in plain english:

> If I'm hungry, then I can order some food from my local restaurant

```ruby
if hungry == true
  order_food_from_local_restaurant
end

order_food_from_local_restaurant if hungry == true
```

Both examples shown above work the same, just mind that the second one only work if you don't have an `else` statement.

An `if/else` statement would look like:

```ruby
if hot == true
  jump_into_the_pool
else
  turn_on_heater
end
```

In the example above, the code will take a decision based of the two scenarios presented, whether is hot or not. There could be a third scenario. Actually, there is no
limit when it comes to scenarios:

```ruby
if hot == true
  jump_into_the_pool
elsif raining == true
  take_out_umbrella
elsif windy == true
  get_windy_jacket
else
  turn_on_heater
end
```

Given the example above, take this into consideration:

* Each condition is evaluated in order
* The `else` comes after all of the `elsif`
* The `else` is optional, you could also write it with a `elsif`

#### || and && 

So far, all of the examples we have provided are based on single comparisons, whether is `hot` or `rainy` or something else, but in many cases you may want to compare for 
multiple conditions. You achieve this through the `||` and `&&` operators. `||` will always evaluate to `true` if one of the conditions is true. `&&` will always evaluate to `true` if 
all conditions are `true`.

We provide a table of truth so it may be easy to visualize:

**`||`**

| Condition 1 | Condition 2 | Evaluates into... |
| ----------- | ----------- | ----------- |
| True | True   | True |
| True | False  | True |
| False | False | False |
| False | True  | True |

**`&&`**

| Condition 1 | Condition 2 | Evaluates into... |
| ----------- | ----------- | ----------- |
| True | True   | True |
| True | False  | False |
| False | False | False |
| False | True  | False |

Another way to remember this, is to use basic math operations, so that every `true` is equals to 1 and every `false` to zero. For the `||` use sums and for the
`&&` use multiplication:

**`||`**

| Condition 1 | Condition 2 | Evaluates into... |
| ----------- | ----------- | ----------- |
| 1 | 1 | 1 |
| 1 | 0 | 1 |
| 0 | 0 | 0 |
| 0 | 1 | 1 |

**`&&`**

| Condition 1 | Condition 2 | Evaluates into... |
| ----------- | ----------- | ----------- |
| 1 | 1 | 1 |
| 1 | 0 | 0 |
| 0 | 0 | 0 |
| 0 | 1 | 0 |

Now some practical examples with Ruby:

```ruby
heroe = "Black Widow"
power = "Fighting Skills"

heroe == "Black Widow" && power == "Fighting Skills"
#=> true

heroe == "Black Widow" || power == "Can fly"
#=> true 

heroe == "Black Widow" && power == "Can fly"
#=> false
```

**Quick try: What would the following code return?. Try to explain why that happens, and how would you rewrite it**

```ruby
heroe = "Hulk"
heroe == "Hulk" || "Black Widow"
```

