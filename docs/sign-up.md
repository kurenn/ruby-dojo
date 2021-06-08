---
title: Sign Up
date: 2021-04-15
slug: user-sign-up
---

## What are you going to learn?

* Understand how to create a user sign up flow
* Send parameters from a Form to the controller

Now that we have a working user model, it is time to create a user's sign up flow, so that any person can register into our website. In this lesson
we will rely on our User model validations from last lesson. 

## User's profile

First of all, we need to create a user's profile page, so that when somebody registers, we can can redirect them there. For now we will just keep things simple
by just displaying the `full name` and eventually a list of microposts alongside a profile image.

Before proceeding, let's create a branch for our new feature:

```bash
$ git checkout master
$ git checkout -b feature/user-sign-up
```

![User Sign Up](/user-sign-up.png)
[Enlarge Image](/user-sign-up.png)

![User Profile](/user-profile.png)
[Enlarge Image](/user-profile.png)

### Installing Tailwind CSS

The first step into working with the user interface, would be to install a css framework. We wil be working with [Tailwind CSS](https://tailwindcss.com/).

> Tailwind CSS is a utility-first CSS framework packed with classes like flex, pt-4, text-center and rotate-90 that can be composed to build any design, directly in your markup.

```bash
$ bundle add tailwindcss-rails
$ rails g tailwindcss:install
.
.
.
Done in 14.85s.
      append  app/javascript/packs/application.js
Configuring Tailwind CSS
      create  app/javascript/stylesheets
      create  app/javascript/stylesheets/application.scss
         run  npx tailwindcss init from "."
  
   @tailwindcss/postcss7-compat 2.1.4
  
   ✅ Created Tailwind config file: tailwind.config.js
  
      insert  postcss.config.js
Add Tailwindcss include tags in application layout
      insert  app/views/layouts/application.html.erb
```

Let's add some sample code to give a little bit of style to our layout.

First, we will create a new [partial file](https://guides.rubyonrails.org/layouts_and_rendering.html#using-partials). A partial file is a device provided by Rails to render or move around
html code with ease. In this case we will be using this for the **navigation bar**.

```bash
$ mkdir app/views/shared
$ touch app/views/shared/_navigation_bar.html.erb
```

Mind the `_` for the file name, this is how Rails identifies partials. Add the following code to the `_navigation_bar.html.erb` file

```html
<!-- component -->
<nav class="font-sans flex flex-col text-center content-center sm:flex-row sm:text-left sm:justify-between py-2 px-6 bg-white shadow sm:items-baseline w-full mb-3">
  <div class="mb-2 sm:mb-0 inner">
    <a href="/home" class="text-2xl no-underline text-grey-darkest hover:text-blue-dark font-sans font-bold">Birdie</a><br>
    <span class="text-xs text-grey-dark">Show we what you got</span>

  </div>

  <div class="sm:mb-0 self-center">
    <!-- <div class="h-10" style="display: table-cell, vertical-align: middle;"> -->
    <%= link_to 'Home', root_path, class: 'text-md no-underline text-black ml-2 px-1' %>
    <%= link_to 'About', about_path, class: 'text-md no-underline text-black ml-2 px-1' %>
    <%= link_to 'Contact', contact_path, class: 'text-md no-underline text-black ml-2 px-1' %>
    <!-- </div> -->
  </div>
</nav>
```

After that, let's go ahead an add the partial into the `app/views/layouts/application.html.erb` layout.

```html
.
.
.
<body>
  <%= render 'shared/navigation_bar' %>
  <div class="container mx-auto">
    <%= yield %>
  </div>
</body>
.
.
.
```

> This will render a file named _navigation_bar.html.erb at that point within the view being rendered. Note the leading underscore character: partials are named with a leading underscore to distinguish them from regular views, even though they are referred to without the underscore. 

If you lift the server and open up the browser, you should see something like this:

![Birdie Home](/birdie-home.png)

Let's commit this before proceeding:

```bash
$ git add .
$ git commit -m 'Adds tailwindcss & navigation bar partial'
```

### The user resource

When following [REST](https://en.wikipedia.org/wiki/Representational_state_transfer), resources are commonly referenced using the resource **name** and an **id**.  This means that for any resource
on our application, in this case users, we should be requesting a single record with the with the following URI - `users/1`.

Rails has a complete support for handling resources. So if our app wants to handle users, we just need to add the following lines into our config/routes.rb file:

```ruby
Rails.application.routes.draw do
  resources :users
  get '/about',    to: 'pages#about'
  get '/contact',  to: 'pages#contact'
  get '/faq',      to: 'pages#faq'

  root to: 'pages#home'
end
```

The `resources :users` line will not only add the `users/:id` URI, but all of the seven REST actions needed for a full resouce access, meaning - update, create, display and destroy. In other words:

| HTTP | Path | URL | Match controller & action|
| -----| ----- | ----- | -------- | 
| GET | `users_path` | /users |  {action: "index", controller: "users"}|
| POST | `users_path` | /users  | {action: "create", controller: "users"} |
| GET | `new_user_path` | /users/new | {action: "new", controller: "users"} |
| GET | `edit_user_path` | /users/:id/edit |            {action: "edit", controller: "users"} |
| GET | `user_path` | /users/:id| {action: "show", controller: "users"} |
| PUT / PATCH  | `users_path` | /users/:id | {action: "update", controller: "users"} |
| DELETE | `users_path` |  /users/:id |{action: "destroy", controller: "users"} |

You can see that these seven actions contain the four basic CRUD operations (create, read, update and delete). They also contain an action to list resources and two auxiliary actions that return new and existing resources. Remember you can always run the `rails routes` command to display all of your router URI paths.

Now if we try to navigate to the `users/1` URI, we should get this error message.

![Users Resources Error](/user-resources-error.png)

This is happening because we don't actually have a controller to handle this requests. Let's fix that:

```bash
$ rails g controller users show
  create  app/controllers/users_controller.rb
    route  get 'users/show'
  invoke  erb
  create    app/views/users
  create    app/views/users/show.html.erb
  invoke  rspec
  create    spec/requests/users_spec.rb
  create    spec/views/users
  create    spec/views/users/show.html.erb_spec.rb
  invoke  helper
  create    app/helpers/users_helper.rb
  invoke    rspec
  create      spec/helpers/users_helper_spec.rb
  invoke  assets
  invoke    scss
```

The command above, will create a `users` controller, along with a `show` view. And now the view should be working.

We will use this view to allocate the user's profile, let's start by adding dynamic data to it:

```ruby
class UsersController < ApplicationController
  def show
    @user = User.find(params[:id])
  end
end
```

We are introducing a new concept in here, the [params](https://guides.rubyonrails.org/action_controller_overview.html#parameters) hash object, which serialize all of the params from the URL or forms
so we can access them on the controller. In this case we are reading the `:id` param from the URL, as part of the REST resource definition as seen on the table above. *You can read more on the official guides [here](https://guides.rubyonrails.org/action_controller_overview.html#parameters).*

So in easy steps:

1. We are assigning an `instance` variable named `@user` in order to access it on the view
2. We are using the `find` method provided by Active Record to fetch the record with a particular id
3. We are using the `params` hash object to read the `id` of the user we want to fetch.

If you try to navigate to the `/users/1` path, it will probably fail, as the record may no exist. For that, let's just create a bunch of users through the `rails console`

```bash
$ rails c
Loading development environment (Rails 6.1.3.2)
irb(main):001:0> User.create(username: 'alice', password: 'password', password_confirmation: 'password', email: 'alice@sample.io', name: 'Alice')
  TRANSACTION (0.4ms)  BEGIN
  User Exists? (2.2ms)  SELECT 1 AS one FROM "users" WHERE LOWER("users"."email") = LOWER($1) LIMIT $2  [["email", "alice@sample.io"], ["LIMIT", 1]]
  User Create (3.5ms)  INSERT INTO "users" ("name", "email", "created_at", "updated_at", "username", "password_digest") VALUES ($1, $2, $3, $4, $5, $6) RETURNING "id"  [["name", "Alice"], ["email", "alice@sample.io"], ["created_at", "2021-06-07 18:01:25.942845"], ["updated_at", "2021-06-07 18:01:25.942845"], ["username", "alice"], ["password_digest", "$2a$12$ua35TxM0Fax4pk1WDMzfN.RtCPawx2SbThPJkf.qKu4tEP4.THJp6"]]
  TRANSACTION (2.8ms)  COMMIT
=> #<User id: 4, name: "Alice", email: "alice@sample.io", created_at: "2021-06-07 18:01:25.942845000 +0000", updated_at: "2021-06-07 18:01:25.942845000 +0000", username: "alice", password_digest: [FILTERED]>
```

Mind the `id` for the user, in our case, it is `4`, but may not be the same for you.

Now if we navigate to `users/4`, the view should be displaying. Let's just add some more useful information:

**app/views/users/show.html.erb**
```ruby
<h1><%= @user.name %>, <%= @user.email %>,</h1>
```

The output should look, something like this:

![User Profile Initial](/user-profile-initial.png)

Now, you have access to a user's profile.

### Exercise

1. Create at least 3 users with different data, and display them on the browser

### Gravatar

Let's add a sample image for the user profile, and to achieve this, we will be using [Gravatar](https://en.gravatar.com/).

> Gravatar is a free service for site owners, developers, and users. It is automatically included in every WordPress.com account and is run and supported by Automattic.

This is a convenient way to include user profile images without the hassle; all we need to do is construct the proper URL for the avatar using the user's email and Gravatar will take
care of the rest.

We will create helper method for this:

**app/helpers/users_helper.rb**
```ruby
module UsersHelper
    # Returns the Gravatar for the given user.
  def gravatar_for(user, size: 80)
    gravatar_id = Digest::MD5::hexdigest(user.email.downcase)
    gravatar_url = "https://secure.gravatar.com/avatar/#{gravatar_id}?s=#{size}"
    image_tag(gravatar_url, alt: user.name, class: "gravatar")
  end
end
```

So now our `app/views/users/show.html.erb` should look like:

```ruby
<%= gravatar_for @user, size: 160 %>
<h1 class='text-3xl'><%= @user.name %>, <%= @user.email %></h1>
```

![User Profile With Gravatar](/gravatar-user-profile.png)

Remember to commit your changes:

```bash
$ git add .
$ git commit -m "Ads user show view for profile"
```

## The Sign Up Form

Now that we have a working user profile, we can sign them up, and redirect them to their profile. Remember we are building this:

![User Sign Up](/user-sign-up.png)
[Enlarge Image](/user-sign-up.png)

### form_for

As you alredy have noticed, Rails comes with multiple helpers that allow us to share code, and with more complex ones like a form, is really like magic happening in front of your eyes.

In this case we will be using a really common helper - [form_with](https://api.rubyonrails.org/v6.1.3.2/classes/ActionView/Helpers/FormHelper.html#method-i-form_with) - this helper can take a url or
an activerecord object.

The fist step is to create the view to display the form. If you recall from the user resources, we already have that route mapped and ready, we just need to add the corresponding controller action and view.

**app/controllers/users_controller.rb**

```ruby
class UsersController < ApplicationController
  def show
    @user = User.find(params[:id])
  end

  def new
    @user = User.new
  end
end
```

**app/views/new.html.erb**

```html
<div class="container max-w-lg mx-auto flex-1 flex flex-col items-center justify-center content-center px-2 mt-10">
  <div class='bg-white px-6 py-8 rounded shadow-md text-black w-full'>
    <h1 class="mb-8 text-3xl text-center">Sign up</h1>
    <%= form_with model: @user do |f| %>
      <%= f.label :name %>
      <%= f.text_field :name, class: 'block border border-grey-light w-full p-3 rounded mb-4', autocomplete: :off, autofocus: true %>

      <%= f.label :last_name %>
      <%= f.text_field :last_name, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>

      <%= f.label :username %>
      <%= f.text_field :username, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>

      <%= f.label :email %>
      <%= f.email_field :email, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>

      <%= f.label :password %>
      <%= f.password_field :password, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>
      
      <%= f.label :password_confirmation %>
      <%= f.password_field :password_confirmation, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>

      <%= f.submit "Create Account", class: 'w-full text-center py-3 rounded bg-green-500 text-white hover:bg-green-700 cursor-pointer focus:outline-none my-1' %>
    <% end %>
  </div>
</div>
```

We will break this into smaller pieces so you can have a better understanding of what is going on. We start with the `form_with` helper method

```ruby
<%= form_with model: @user do |f| %>
  .
  .
  .
<% end %>
```

When working with built-in helpers, you will notice that they wrap a lot of functionality, we encourage you to take a deeper read on how this works [here](https://guides.rubyonrails.org/form_helpers.html#dealing-with-basic-forms). For now what you need to understand is that the block variable `f` is a form object used inside the attach the fields that will be send on submission.

And as you can see we are attaching simple html tags, such as text fields, email fields, labels, and much more. You can read more in [here](https://guides.rubyonrails.org/v5.0/form_helpers.html). In other words,

```ruby
<%= f.label :last_name %>
<%= f.text_field :last_name, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>
```

creates the HTML needed to build a label attached to the text field with the same attribute(`:last_name`). When working with models, you can attach any field to an instance attribute, such as `:name` or `email`. They can be columns on the database or virtual attributes, such as the `password` and `password_confirmation`. In case an attribute does not exist on the instance, the view will raise an error.

Let's take a look at the HTML generated by Rails:

```html
<form action="/users" accept-charset="UTF-8" method="post">
  <input type="hidden" name="authenticity_token" value="rTm-HkUh2GqUfKkGmc-SK5t6tuHfGM8S_tECE8fjsWE3SJJypq5W6Ni_rEgYQT_PtPrm9nHc6LVPsBoWfkjxZw">
  <label for="user_name">Name</label>
  <input class="block border border-grey-light w-full p-3 rounded mb-4" autocomplete="off" autofocus="autofocus" type="text" name="user[name]" id="user_name">

  <label for="user_last_name">Last name</label>
  <input class="block border border-grey-light w-full p-3 rounded mb-4" type="text" name="user[last_name]" id="user_last_name">

  <label for="user_username">Username</label>
  <input class="block border border-grey-light w-full p-3 rounded mb-4" type="text" name="user[username]" id="user_username">

  <label for="user_email">Email</label>
  <input class="block border border-grey-light w-full p-3 rounded mb-4" type="email" name="user[email]" id="user_email">

  <label for="user_password">Password</label>
  <input class="block border border-grey-light w-full p-3 rounded mb-4" type="password" name="user[password]" id="user_password">
  
  <label for="user_password_confirmation">Password confirmation</label>
  <input class="block border border-grey-light w-full p-3 rounded mb-4" type="password" name="user[password_confirmation]" id="user_password_confirmation">

  <input type="submit" name="commit" value="Create Account" class="w-full text-center py-3 rounded bg-green-500 text-white hover:bg-green-700 cursor-pointer focus:outline-none my-1" data-disable-with="Create Account">
</form>
```

And if we break it down, you will find a pattern. So for example this,

```html
<%= f.label :name %>
<%= f.text_field :name, class: 'block border border-grey-light w-full p-3 rounded mb-4', autocomplete: :off, autofocus: true %>
```

generates

```html
<label for="user_name">Name</label>
<input class="block border border-grey-light w-full p-3 rounded mb-4" autocomplete="off" autofocus="autofocus" type="text" name="user[name]" id="user_name">
```

while

```html
<%= f.label :password %>
<%= f.password_field :password, class: 'block border border-grey-light w-full p-3 rounded mb-4' %>
```

produces

```html
<label for="user_password">Password</label>
<input class="block border border-grey-light w-full p-3 rounded mb-4" type="password" name="user[password]" id="user_password">
```

As you can see all of the html generated follows a particular pattern if you take a closer look at the `name` attribute for each html tag. This attribute allows Rails to build
the params hash to be sent over the form. That hash would look something like:

```ruby
{
  user: {
    name: 'Any name',
    last_name: 'Last name',
    username: 'username',
    email: 'example@email.com',
    password: 'password',
    password_confirmation: 'password'
  }
}
```

This way we can access the user hash and create a user record from our controller. But more on that, in the next section.

### Submitting the form

We are now at the point where we are ready to submit the form to the controller, but wait, how does the form knows where to go?. This is again achievable by the Rails helpers magic and the usage
of conventions in the framework. By providing an instance, Rails will recognize the resource name and if it is a new record, so it can be sent to the `create` action as describe in the [table above](#the-user-resource).

The request will be sent via **POST** to the **create** action, inside the `users_controller`. Let's add that action:

**app/controllers/users_controller.rb**

```ruby
def create
  @user = User.new(params[:user])
  if @user.save
    # Handle the success
  else
    # Handle the error
  end
end
```

If we try to post the form, we should get the following error message:

![Strong Parameters Error](/strong_parameters_error.png)

This is a very common error when submitting a form. Rails introduces a concept of [Strong Parameters](https://api.rubyonrails.org/classes/ActionController/StrongParameters.html) in order to prevent mass-assignment from a form and there be vulnerable to add any other attribute for the user in this case, such as an `admin` boolean for example.

> It provides an interface for protecting attributes from end-user assignment. This makes Action Controller parameters forbidden to be used in Active Model mass assignment until they have been explicitly enumerated.

So for this to work, we need to explicitly tell Rails what we want to mass-assign through the form:

**app/controllers/users_controller.rb**

```ruby
def create
  @user = User.new(params[:user])
  if @user.save
    # Handle the success
  else
    # Handle the error
  end
end

private

def user_params
  params.require(:user).permit(:name, :last_name, :username, :email, :password, :password_confirmation)
end
```

The `user_params` method will return a white list of safe params to be mass-assigned, we just need to now create the user through this method:

**app/controllers/users_controller.rb**

```ruby
def create
  @user = User.new(user_params)
  if @user.save
    # Handle the success
  else
    # Handle the error
  end
end

private

def user_params
  params.require(:user).permit(:name, :last_name, :email, :password, :password_confirmation)
end
```

By performing this change, our sign up form, should be up and running, even though it seems like nothing happen, we can see on the logs, that the user is being inserted into the database:

```bash
web_1        | Started POST "/users" for 172.22.0.1 at 2021-06-08 03:57:15 +0000
web_1        | Cannot render console from 172.22.0.1! Allowed networks: 127.0.0.0/127.255.255.255, ::1
web_1        | Processing by UsersController#create as HTML
web_1        |   Parameters: {"authenticity_token"=>"[FILTERED]", "user"=>{"name"=>"Abraham", "last_name"=>"Kuri", "username"=>"kurenn", "email"=>"kurenn@icalialabs.com", "password"=>"[FILTERED]", "password_confirmation"=>"[FILTERED]"}, "commit"=>"Create Account"}
web_1        |   TRANSACTION (1.0ms)  BEGIN
web_1        |   ↳ app/controllers/users_controller.rb:12:in `create'
web_1        |   User Exists? (1.3ms)  SELECT 1 AS one FROM "users" WHERE LOWER("users"."email") = LOWER($1) LIMIT $2  [["email", "kurenn@icalialabs.com"], ["LIMIT", 1]]
web_1        |   ↳ app/controllers/users_controller.rb:12:in `create'
web_1        |   User Create (1.1ms)  INSERT INTO "users" ("name", "email", "created_at", "updated_at", "username", "password_digest", "last_name") VALUES ($1, $2, $3, $4, $5, $6, $7) RETURNING "id"  [["name", "Abraham"], ["email", "kurenn@icalialabs.com"], ["created_at", "2021-06-08 03:57:15.858385"], ["updated_at", "2021-06-08 03:57:15.858385"], ["username", "kurenn"], ["password_digest", "$2a$12$BmsTffsH4TAGVFKBPryrwe8oIIx7fMJJYm7Ym5lXTWKoW1M3XfaW."], ["last_name", "Kuri"]]
web_1        |   ↳ app/controllers/users_controller.rb:12:in `create'
web_1        |   TRANSACTION (3.3ms)  COMMIT
web_1        |   ↳ app/controllers/users_controller.rb:12:in `create'
web_1        | No template found for UsersController#create, rendering head :no_content
web_1        | Completed 204 No Content in 209ms (ActiveRecord: 6.7ms | Allocations: 3657)
```

### Handling Success

Now that we have our form fully working, we need to add some behavior in order to handle when the record is saved. So right now we have this,

```ruby
def create
  @user = User.new(user_params)
  if @user.save
    # Handle the success
  else
    # Handle the error
  end
end
```

A common behavior after you sign up to a site, is to authenticate you and redirect you to your profile, or any dashboard view. For now we will focus on the redirection. Rails provides a
`redirect_to` method to handle this exact behavior. Through this we can redirect to any path from our site or even external ones. 

In our case we will redirect to the user profie we created earlier. There are multiple ways to do it, we will provide them in the same example and leave only one working, which is the preferred one.

```ruby
def create
  @user = User.new(user_params)
  if @user.save
    #redirect_to "/users/#{@user.id}"
    #redirect_to @user
    redirect_to user_path(@user)
  else
    # Handle the error
  end
end
```

As you can see from the code above, we provided 3 ways to redirect to the `users/:id` path, the last two are actually the most common ones, and yes, Rails is smart enough to identify the path from the 
object and build the correct path.

So if you go and try to create a new user, you will be redirected to the user profile page we created earlier.

Now would be a good time to commit our changes:

```bash
$ git add .
$ git commit -m 'Adds user success sign up flow'
```
