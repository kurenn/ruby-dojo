---
title: Modeling Users
date: 2021-04-15
slug: modeling-users
---

## What are you going to learn?

* How to create new models using the Rails command
* Basic usage of ActiveRecord
* Perform CRUD actions to our model
* Understand how migrations work

Before starting building a feature, it is important to create a new branch, let's do that:

```bash
$ git checkout -b modeling-users
```

## Creating Models

Models are the way we interact with the database through Ruby classes and instances. All of the models live under the `app/models` directory, and are all Ruby classes.

Models uses ActiveRecord underneath. ActiveRecord is the object-relational mapping (ORM) supplied with Rails, and it's basically the M in MVC

An ORM (Object Relational Mapping system) a technique that connects the rich objects of an application to tables in a relational database.

### Conventions

- **Database Table.** Plural with underscores separating words (e.g., `book_clubs`).
- **Model Class.** Follows the same conventions of ruby classes: Singular with the first letter of each word capitalized (e.g., `BookClub`).


| Model / Class	| Table / Schema |
|--------------|-----------------|
| User		| users |
| Article	| articles |
| LineItem	| line_items |
| Mouse		| mice |
| Person	| people |
| TaxAgency | tax_agencies |

- **Foreign keys.** These fields should be named following "Singularized Table Name" pattern *singularized_table_name_id* (e.g., `item_id`, `order_id`). These are the fields that Active Record will look for when you create associations between your models.

- **Primary keys.** By default, Active Record will use an integer column named id as the table's primary key. When using Active Record Migrations to create your tables, this column will be automatically created.

- **`created_at` & `updated_at`** Automatically added by model generator. ActiveRecord  uses `created_at` to set the current date and time when the record is created and `updated_at` to set the current time and date whenever the record is updated

- **ID.** By default migration added by model generator will include the id[autoincrement] column as an index.

#### Overriding conventions

Although Rails handles most of your database configuration, occasionally you might want to change that:


```ruby
class Product < ActiveRecord::Base
  self.table_name = "regular_products"
  self.primary_key = "product_id"
end
```

### A Ruby User class

As you may be wondering, a `User` class may look like this:

```ruby
class User
  attr_accessor :name, :email
  .
  .
  .
end
```

The problem with the class above, is that it lacked the critical property of persistence, so they only live on memory, and as soon as this is flush, all of the intances dissapear. The goal in here is to prevent that.

So in order to create a Rails model that rely on ActiveRecord, so it persists or can be fetch for later usage.

Let's create the `user` model with the rails generator:

```bash
$ rails g model User name:string email:string
Running via Spring preloader in process 21
      invoke  active_record
      create    db/migrate/20210601171454_create_users.rb
      create    app/models/user.rb
      invoke    rspec
      create      spec/models/user_spec.rb
```

The code above creates:

1. A migration file `db/migrate/20210601171454_create_users.rb`. Rails provides a DSL to interact with the database in order to create tables, add columns, indexes or any other
type of operation. Please read more on this [here](https://guides.rubyonrails.org/active_record_migrations.html)

All of the instruction below will translate to valid SQL, depending on which adapter we are using, Postgres, MySQL or any other.

Note that the name of the migration file is prefixed by a timestamp based on when the migration was generated. The migration itself consists of a change method that determines the change to be made to the database.

```ruby
class CreateUsers < ActiveRecord::Migration[6.1]
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
```

2. The user model that inherits from `ApplicationRecord`. This super class will add all of the behavior needed to interact with the `users` table through Ruby objects.Please read the [this](https://guides.rubyonrails.org/active_record_basics.html) for more information.

By inhering from `ApplicationRecord`, we will be able to perform [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions or add validations for example. We will go into
detail later on the lesson. For now just make sure everything is being generated correctly.

```ruby
class User < ApplicationRecord
end
```

3. The spec files, which we will be using later.

### Database Migrations

> Migrations are a convenient way to alter your database schema over time in a consistent way. They use a Ruby DSL so that you don't have to write SQL by hand, allowing your schema and changes to be database independent.

After creating the model, the next step is to actually map or `migrate` what we have under the `db/migrate` directory, in this case, only the `users` table creation. We can achieve this by:

```bash
$ rails db:migrate
== 20210601171454 CreateUsers: migrating ======================================
-- create_table(:users)
   -> 0.0132s
== 20210601171454 CreateUsers: migrated (0.0134s) =============================
```

Rails keeps track of all of the migrations, so the next time you run the `rails db:migrate` command it will do nothing. And now we are ready to start creating records into the database.

Before we move on, it would be a good time to commit:

```bash
$ git commit -am 'Creates the user model'
```

## Creating user

As you can recall from previous lessons, we can start exploring the data models through the Rails console. And since we don't(yet) want to make any changes, we'll fire up the
console in *sandbox* mode:

```bash
$ rails c --sandbox
Running via Spring preloader in process 38
Loading development environment in sandbox (Rails 6.1.3.2)
Any modifications you make will be rolled back on exit
irb(main):001:0>
```

Once in there, let's start by creating new users. You don't need to load anything, all of the models are automatically loaded for you by Rails.

```bash
irb(main):001:0> user = User.new
=> #<User id: nil, name: nil, email: nil, created_at: nil, updated_at: nil>
```

When we called the `User.new` it returns an object with all nil attributes. But we can certainly change that. 

```bash
irb(main):001:0> user = User.new(name: 'Alice', email: 'alice@sample.com')
=> #<User id: nil, name: "Alice", email: "alice@sample.com", created_at: nil, updated_at: nil>
```

ActiveRecord initialize objects through a hash of attributes, just as you see in the code above. It also maps every column of the table to instance `accessor` methods,
so you don't have to specify those neither.

So far, we have not touched the database, the `user` will only exists as long as we keed the console open. In order to save the object into the database, we need to call the
`save` method on the `user` variable:

```bash
irb(main):001:0> user.save
TRANSACTION (0.3ms)  SAVEPOINT active_record_1
  User Create (1.3ms)  INSERT INTO "users" ("name", "email", "created_at", "updated_at") VALUES ($1, $2, $3, $4) RETURNING "id"  [["name", "Alice"], ["email", "alice@sample.com"], ["created_at", "2021-06-01 20:45:52.582268"], ["updated_at", "2021-06-01 20:45:52.582268"]]
  TRANSACTION (0.3ms)  RELEASE SAVEPOINT active_record_1
=> true
```

The `save` method returns `true` when the records gets inserted into the database, otherwise, will return `false`. As a Rails developer you hardly need raw SQL to perform almost
any operation. We will not go into much detail around this as is not the purpose of the lesson.

If you try to access the `user` variable again, you wil notice that the `id` and the `created_at` and `updated_at` columns were magically set:

```ruby
irb(main):001:0> user
=> #<User id: 1, name: "Alice", email: "alice@sample.com", created_at: "2021-06-01 20:45:52.582268000 +0000", updated_at: "2021-06-01 20:45:52.582268000 +0000">
```

As with any other instance, you can access the attributes for the user:

```ruby
user.name
#=> "Alice"
user.id
#=> 1
user.created_at
#=> Tue, 01 Jun 2021 20:45:52.582268000 UTC +00:00
```

There is another way to create users, even though *instantiate* and *saving* them is the prefer way, you can do it like this:

```ruby
User.create(name: 'Alice', email: 'alice@sample.com')
TRANSACTION (0.3ms)  SAVEPOINT active_record_1
  User Create (0.6ms)  INSERT INTO "users" ("name", "email", "created_at", "updated_at") VALUES ($1, $2, $3, $4) RETURNING "id"  [["name", "Alice"], ["email", "alice@sample.com"], ["created_at", "2021-06-01 21:12:38.186403"], ["updated_at", "2021-06-01 21:12:38.186403"]]
  TRANSACTION (0.5ms)  RELEASE SAVEPOINT active_record_1
=> #<User id: 2, name: "Alice", email: "alice@sample.com", created_at: "2021-06-01 21:12:38.186403000 +0000", updated_at: "2021-06-01 21:12:38.186403000 +0000">
```

Unlike the `save` method, `create` returns the instance, which you can assign to a variable for later use. 

## Finding user objects

*We'll present the most common methods to read information, but you might want to check the complete [documentation](http://edgeguides.rubyonrails.org/active_record_querying.html) to see the power of ActiveRecord*

Reading from a database involves specyfing which particular rows of data are you interested in. The simplest way to finding a row in a table is by specifying it's primary key

ActiveRecord has his own query interface, imagine you want to find the product with an id of 1

```ruby
User.find(1)
=> #<User id: 1, name: "Alice", email: "alice@sample.com", created_at: "2021-06-01 20:45:52.582268000 +0000", updated_at: "2021-06-01 20:45:52.582268000 +0000">
```

The [find](https://api.rubyonrails.org/v6.1.3.2/classes/ActiveRecord/FinderMethods.html#method-i-find) method can also accept multiple primary keys values, so to find users with an id of 1 & 2:

```ruby
=> User.find([1,2])
=> [#<User id: 1, ...">, #<User id: 2,...>]
```

If none of the users match the criteria an `ActiveRecord::RecordNotFound` exception will be thrown:

```ruby
=> Product.find(43432)
ActiveRecord::RecordNotFound: Couldn't find User with 'id'=43432
```

In addition to the generic `find`, ActiveRecord also offers the ability to [find by](https://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html#method-i-find_by) a set of attributes:

```ruby
User.find_by(name: 'Alice')
 User Load (0.3ms)  SELECT "users".* FROM "users" WHERE "users"."name" = $1 LIMIT $2  [["name", "Alice"], ["LIMIT", 1]]
=> #<User id: 1, name: "Alice", email: "alice@sample.com", created_at: "2021-06-01 20:45:52.582268000 +0000", updated_at: "2021-06-01 20:45:52.582268000 +0000">
```

Note that `find` and `find_by` will always return the first record that matches the search.

A commonly use to find records with ease, is through the [first](https://api.rubyonrails.org/classes/ActiveRecord/Associations/CollectionProxy.html#method-i-first) method:

```ruby
User.first
  User Load (0.5ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT $1  [["LIMIT", 1]]
=> #<User id: 1, name: "Alice", email: "alice@sample.com", created_at: "2021-06-01 20:45:52.582268000 +0000", updated_at: "2021-06-01 20:45:52.582268000 +0000">
```

Naturally there is a `last` method, which will retrieve the last record, but also if you want them all, you can simply run:

```ruby
User.all
=> [#<User id: 1, ...">, #<User id: 2,...>]
```

This will return an [ActiveRecord::Relation](https://api.rubyonrails.org/v6.1.3.2/classes/ActiveRecord/Relation.html) behaves like an array, but have a more powerful interface that
we will discuss later.

## Updating user objects

Once we have created records, we will often want to change or update them. There are basically two way to achieve this.

1. The first way to do it, is simply by setting the attributes and then calling `save`:

```ruby
user = User.create(name: 'Alice', email: 'alice@sample.io')
user.name = 'Abraham'
=> "Abraham"

irb(main):005:0> user.email = 'abraham@sample.io'
=> "abraham@sample.io"

irb(main):006:0> user.save
  TRANSACTION (0.5ms)  SAVEPOINT active_record_1
  User Update (0.8ms)  UPDATE "users" SET "name" = $1, "email" = $2, "updated_at" = $3 WHERE "users"."id" = $4  [["name", "Abraham"], ["email", "abraham@sample.io"], ["updated_at", "2021-06-02 15:53:06.765844"], ["id", 3]]
  TRANSACTION (0.5ms)  RELEASE SAVEPOINT active_record_1
=> true

#<User id: 3, name: "Abraham", email: "abraham@sample.io", created_at: "2021-06-02 15:52:37.363358000 +0000", updated_at: "2021-06-02 15:53:06.765844000 +0000">
```

As you can see from above, now the `created_at` and `updated_at` columns are not the same.

2. The second way to update a record is with the `update` method:

```ruby
user.update(name: 'Ash Ketchum', email: 'pikachu@pokemon.io')
  TRANSACTION (0.3ms)  SAVEPOINT active_record_1
  User Update (0.5ms)  UPDATE "users" SET "name" = $1, "email" = $2, "updated_at" = $3 WHERE "users"."id" = $4  [["name", "Ash Ketchum"], ["email", "pikachu@pokemon.io"], ["updated_at", "2021-06-02 15:56:29.820321"], ["id", 3]]
  TRANSACTION (0.3ms)  RELEASE SAVEPOINT active_record_1
=> true
#<User id: 3, name: "Ash Ketchum", email: "pikachu@pokemon.io", created_at: "2021-06-02 15:52:37.363358000 +0000", updated_at: "2021-06-02 15:56:29.820321000 +0000">
```

The `update` method accepts a hash of attributes, and so it performs the update and save in one step. As you can see it returns **true** when the record was successfully updated. There is a weay to update just one attribute, through the `update_attribute` method, you can read more about it [here](https://api.rubyonrails.org/classes/ActiveRecord/Persistence.html#method-i-update_attribute)

## Destroying user objects

There would be a time when you want to delete records from the database, well, Rails got your back, you can achieve this easily by calling the [destroy](https://api.rubyonrails.org/classes/ActiveRecord/Persistence/ClassMethods.html#method-i-destroy) method

```ruby
user = User.create(name: 'Alice', email: 'alice@sample.io')
user.destroy
#<User id: 3, name: "Alice", email: "alice@sample.io", created_at: "2021-06-02 15:52:37.363358000 +0000", updated_at: "2021-06-02 15:53:06.765844000 +0000">

user = User.create(name: 'Alice', email: 'alice@sample.io')
user.delete
#<User id: 3, name: "Alice", email: "alice@sample.io", created_at: "2021-06-02 15:52:37.363358000 +0000", updated_at: "2021-06-02 15:53:06.765844000 +0000">

User.destroy(3)
#<User id: 3, name: "Alice", email: "alice@sample.io", created_at: "2021-06-02 15:52:37.363358000 +0000", updated_at: "2021-06-02 15:53:06.765844000 +0000">
```

* The **destroy** method will return the instance, but also trigger callbacks and remove associated records if it has them.
* The **delete** method will return the instance, but will not trigger anything else, unless specified
* The **destroy class** method works the same as the `destroy` method for an instance

**Keep in mind that once you delete a record, you will not be able to recover it, so handle with care**

## Validations

Now that we have a working `User` class, and understand how to create, update or destroy records, it is time to add some validations to prevent unwanted records to be saved into
the database and have data integrity on the database. 

For example, the `name` attribute should not be empty, we will always require a user to have it, but also not to exceed a number of characters. Also the email should be
present and follow a particular format, but also to be unique so users can login.

### Validate presence

Let's add some of these validations to our model, but first we add some specs.

```ruby
require 'rails_helper'

RSpec.describe User, type: :model do
  describe 'name presence for the user' do
    it 'is not valid without a name' do
      user = User.new(email: 'alice@sample.io')

      expect(user).not_to be_valid
    end
  end
end
```

At this point we should be **red**:

```bash
rspec spec/models/user_spec.rb 

Randomized with seed 43742

User
  name presence for the user
    is not valid without a name (FAILED - 1)

Failures:

  1) User name presence for the user is not valid without a name
     Failure/Error: expect(user).not_to be_valid
       expected #<User id: nil, name: nil, email: "alice@sample.io", created_at: nil, updated_at: nil> not to be valid
     # ./spec/models/user_spec.rb:8:in `block (3 levels) in <top (required)>'

Top 1 slowest examples (0.05911 seconds, 89.4% of total time):
  User name presence for the user is not valid without a name
    0.05911 seconds ./spec/models/user_spec.rb:5

Finished in 0.06613 seconds (files took 2.12 seconds to load)
1 example, 1 failure

Failed examples:

rspec ./spec/models/user_spec.rb:5 # User name presence for the user is not valid without a name

Randomized with seed 43742
```

Let's change that:

**app/models/user.rb**

```ruby
class User < ApplicationRecord
  validates :name, presence: true
end
```

And then run the specs again:

```bash
rspec spec/models/user_spec.rb 

Randomized with seed 45458

User
  name presence for the user
    is not valid without a name

Top 1 slowest examples (0.01811 seconds, 89.1% of total time):
  User name presence for the user is not valid without a name
    0.01811 seconds ./spec/models/user_spec.rb:5

Finished in 0.02032 seconds (files took 0.77453 seconds to load)
1 example, 0 failures

Randomized with seed 45458[]
```

Because the record is not valid, it won't be inserted into the database or even updated if its an existing one.

Before proceeding, let's check for validity on the terminal, and see how the instance behaves:

```ruby
user = User.new
user.valid?
#=> false

user.errors
#<ActiveModel::Errors:0x0000563fa44ff5a8 @base=#<User id: nil, name: nil, email: nil, created_at: nil, updated_at: nil>, @errors=[#<ActiveModel::Error attribute=name, type=blank, options={}>]>
user.errors.full_messages
=> ["Name can't be blank"]
```

The `errors` methods return an `ActiveModel::Errors` instance which holds all of the errors that made the object invalid. The `full_messages` is a humanized version of the errors
which we can easily print onto a view, when registering a user for example.

**Let's commit**

```bash
$ git commit -am "Adds presence validation for user's name"
```

### Exercise

**Remember to perform a commit for each step below**

1. Add a spec to validate the presence of the email as well, and make it pass
2. Try some examples on your terminal, see how the `full_messages` behave

### Validate length

Right now our `User` model is a bit more rigid, and at least won't accept records without `name` or `email`, but let's go further and limit the number of
characters for the `name` attribute.

Let's say that *61* characters is a too long for a name, remember, first we test:

```ruby
require 'rails_helper'

RSpec.describe User, type: :model do
  describe 'name validations' do
    it 'is not valid without a name' do
      user = User.new(email: 'alice@sample.io')

      expect(user).not_to be_valid
    end

    it 'is not valid with names longer that 60 characters' do
      user = User.new(name: 'a'*61)

      expect(user).not_to be_valid
    end
  end
end
```

```bash
Failures:

  1) User name validations is not valid with names longer that 60 characters
     Failure/Error: expect(user).not_to be_valid
       expected #<User id: nil, name: "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...", email: nil, created_at: nil, updated_at: nil> not to be valid
     # ./spec/models/user_spec.rb:14:in `block (3 levels) in <top (required)>'

Top 2 slowest examples (0.04065 seconds, 92.2% of total time):
  User name validations is not valid with names longer that 60 characters
    0.03653 seconds ./spec/models/user_spec.rb:11
  User name validations is not valid without a name
    0.00412 seconds ./spec/models/user_spec.rb:5

Finished in 0.04411 seconds (files took 0.90745 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/models/user_spec.rb:11 # User name validations is not valid with names longer that 60 characters

Randomized with seed 58574
```

Now we make the tests pass:

```ruby
class User < ApplicationRecord
  validates :name, presence: true, length: { maximum: 60 }
end
```

And run the specs again:

```bash
User
  name validations
    is not valid with names longer that 60 characters
    is not valid without a name

Top 2 slowest examples (0.02521 seconds, 82.3% of total time):
  User name validations is not valid with names longer that 60 characters
    0.02262 seconds ./spec/models/user_spec.rb:11
  User name validations is not valid without a name
    0.00259 seconds ./spec/models/user_spec.rb:5

Finished in 0.03063 seconds (files took 1.02 seconds to load)
2 examples, 0 failures

Randomized with seed 50448
```

**Let's commit**

```bash
$ git commit -am "Adds length validation for user's name"
```

### Exercise

**Remember to perform a commit for each step below**

1. Add a spec to validate the length of the email as well, and make it pass. The length should not be longer than **244** characters.
2. Try some examples on your terminal, see how the `full_messages` behave

### Validate format

So far, we have added some basic constraints for the `name` and `email` - any non-blank name under 60 is valid for example - but of course
this is not enough for the `email` attribute. Emails follow a more complex structure and for that we can use a pattern to validate how it is
formed. This is where the `format` validation comes into play.

But first, let's add a spec for that:

```ruby
require "rails_helper"

RSpec.describe User, type: :model do
  describe "name validations" do
    it "is not valid without a name" do
      user = User.new(email: "alice@sample.io")

      expect(user).not_to be_valid
    end

    it "is not valid with names longer that 60 characters" do
      user = User.new(name: "a" * 61)

      expect(user).not_to be_valid
    end
  end

  describe "email validations" do
    it "is not valid without the correct format" do
      invalid_addresses = %w[user@example,com user_at_foo.org user.name@example.
                             foo@bar_baz.com foo@bar+baz.com]

      user = User.new(name: 'Alice')

      invalid_addresses.each do |invalid_address|
        user.email = invalid_address
        expect(user).not_to be_valid
      end
    end
  end
end
```

Our tests are now failing:

```bash
User
  name validations
    is not valid with names longer that 60 characters
    is not valid without a name
  email validations
    is not valid without the correct format (FAILED - 1)

Failures:

  1) User email validations is not valid without the correct format
     Failure/Error: expect(user).not_to be_valid
       expected #<User id: nil, name: "Alice", email: "user@example,com", created_at: nil, updated_at: nil> not to be valid
     # ./spec/models/user_spec.rb:27:in `block (4 levels) in <top (required)>'
     # ./spec/models/user_spec.rb:25:in `each'
     # ./spec/models/user_spec.rb:25:in `block (3 levels) in <top (required)>'

Top 3 slowest examples (0.04509 seconds, 90.8% of total time):
  User name validations is not valid with names longer that 60 characters
    0.02577 seconds ./spec/models/user_spec.rb:11
  User email validations is not valid without the correct format
    0.01674 seconds ./spec/models/user_spec.rb:19
  User name validations is not valid without a name
    0.00259 seconds ./spec/models/user_spec.rb:5

Finished in 0.04968 seconds (files took 0.89746 seconds to load)
3 examples, 1 failure

Failed examples:

rspec ./spec/models/user_spec.rb:19 # User email validations is not valid without the correct format

Randomized with seed 32264
```

Let's fix this:

```ruby
class User < ApplicationRecord
  validates :name, presence: true, length: { maximum: 60 }

  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
  validates :email, format: { with: VALID_EMAIL_REGEX }
end
```

And now we are green:

```bash
User
  name validations
    is not valid with names longer that 60 characters
    is not valid without a name
  email validations
    is not valid without the correct format

Top 3 slowest examples (0.02939 seconds, 88.6% of total time):
  User name validations is not valid with names longer that 60 characters
    0.02232 seconds ./spec/models/user_spec.rb:11
  User name validations is not valid without a name
    0.00367 seconds ./spec/models/user_spec.rb:5
  User email validations is not valid without the correct format
    0.00341 seconds ./spec/models/user_spec.rb:19

Finished in 0.03319 seconds (files took 0.86319 seconds to load)
3 examples, 0 failures

Randomized with seed 32721
```

**Let's commit**

```bash
$ git commit -am "Adds format validation for user's email"
```

## Validate uniqueness

Emails are commonly use to authenticate users on any application, that is the primary reason why they have to be unique. Let's start with
some short tests.

```ruby
require "rails_helper"

RSpec.describe User, type: :model do
  .
  .
  .
  describe "email validations" do
    it 'is not valid when the email already exists on the database' do
      duplicated_email = "alice@sample.io"
      user = User.create(name: "Alice", email: duplicated_email)

      duplicated_user = user.dup

      expect(duplicated_user).not_to be_valid
    end

    it "is not valid without the correct format" do
      invalid_addresses = %w[user@example,com user_at_foo.org user.name@example.
                             foo@bar_baz.com foo@bar+baz.com]

      user = User.new(name: 'Alice')

      invalid_addresses.each do |invalid_address|
        user.email = invalid_address
        expect(user).not_to be_valid
      end
    end
  end
end
```

We are back to `red`

```bash
Failures:

  1) User email validations is not valid when the email already exists on the database
     Failure/Error: expect(duplicated_user).not_to be_valid
       expected #<User id: nil, name: "Alice", email: "alice@sample.io", created_at: nil, updated_at: nil> not to be valid
     # ./spec/models/user_spec.rb:25:in `block (3 levels) in <top (required)>'

Top 4 slowest examples (0.08351 seconds, 91.6% of total time):
  User email validations is not valid when the email already exists on the database
    0.06945 seconds ./spec/models/user_spec.rb:19
  User name validations is not valid without a name
    0.00532 seconds ./spec/models/user_spec.rb:5
  User name validations is not valid with names longer that 60 characters
    0.00439 seconds ./spec/models/user_spec.rb:11
  User email validations is not valid without the correct format
    0.00435 seconds ./spec/models/user_spec.rb:28

Finished in 0.09114 seconds (files took 0.97034 seconds to load)
4 examples, 1 failure

Failed examples:

rspec ./spec/models/user_spec.rb:19 # User email validations is not valid when the email already exists on the database
```

Let's fix that:

```ruby
class User < ApplicationRecord
  validates :name, presence: true, length: { maximum: 60 }

  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
  validates :email, format: { with: VALID_EMAIL_REGEX },
                    uniqueness: true
end
```

```bash
User
  name validations
    is not valid without a name
    is not valid with names longer that 60 characters
  email validations
    is not valid without the correct format
    is not valid when the email already exists on the database

Top 4 slowest examples (0.05709 seconds, 93.9% of total time):
  User name validations is not valid without a name
    0.03151 seconds ./spec/models/user_spec.rb:5
  User email validations is not valid without the correct format
    0.01436 seconds ./spec/models/user_spec.rb:28
  User email validations is not valid when the email already exists on the database
    0.00727 seconds ./spec/models/user_spec.rb:19
  User name validations is not valid with names longer that 60 characters
    0.00395 seconds ./spec/models/user_spec.rb:11

Finished in 0.06077 seconds (files took 0.96831 seconds to load)
4 examples, 0 failures

Randomized with seed 54344
```

We are back to green. But we are not done yet. Email addresses are commonly processed as if they were case-insensituve, for example `alice@sample.io`, so our validation
should incorporate this as well.

```ruby
  .
  .
  .
  describe "email validations" do
    it "is not valid when the email already exists on the database" do
      duplicated_email = "alice@sample.io"
      user = User.create(name: "Alice", email: duplicated_email)

      duplicated_user = user.dup
      duplicated_user.email = user.email.upcase

      expect(duplicated_user).not_to be_valid
    end
  end
  .
  .
  .
```

```ruby
class User < ApplicationRecord
  validates :name, presence: true, length: { maximum: 60 }

  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
  validates :email, format: { with: VALID_EMAIL_REGEX },
                    uniqueness: { case_sensitive: false }
end
```

And we are green again.

**Let's commit**

```bash
$ git commit -am "Adds uniqueness validation for user's email"
```

### Exercise

**Remember to perform a commit for each step below**

1. Add a `last_name` attribute for the user model. 
* This attribute can be nil, but when present, should be no longer than 80 characters
2. Add a `username` attribute for the user model.
* The `username` has to be unique(case insensitive), present and with no more than 40 characters
3. Implement the `has_secure_password` for users. You can read more in [here](https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password)
4. Through a new migration add an `index` for the user email, it should be unique. You can read how to add indexes [here](https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/SchemaStatements.html#method-i-add_index). And [this](https://guides.rubyonrails.org/active_record_migrations.html#creating-a-standalone-migration) is how you can create a standalone migration file.

### Resources

* [Rails for Beginners Part 11](https://gorails.com/episodes/rails-for-beginners-part-11-creating-the-user-model?autoplay=1)
* [Rails for Beginners Part 12](https://gorails.com/episodes/rails-for-beginners-part-12-validations?autoplay=1)
