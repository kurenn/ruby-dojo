---
title: Static Pages
date: 2021-04-15
slug: static-pages
---

## What are you going to learn?

* Understand how Controllers and Views Interact
* Usage of rails generators for controllers
* Introduction to built-in helpers

## Pages Controller

Before starting building a feature, it is important to create a new branch, let's do that:

```bash
$ git checkout -b pages-controller
```

After creating our whole new branch, let's create a `pages` controller:

```bash
$ rails g controller pages home about
      create  app/controllers/pages_controller.rb
       route  get 'pages/home'
get 'pages/about'
      invoke  erb
      create    app/views/pages
      create    app/views/pages/home.html.erb
      create    app/views/pages/about.html.erb
      invoke  rspec
      create    spec/requests/pages_spec.rb
      create    spec/views/pages
      create    spec/views/pages/home.html.erb_spec.rb
      create    spec/views/pages/about.html.erb_spec.rb
      invoke  helper
      create    app/helpers/pages_helper.rb
      invoke    rspec
      create      spec/helpers/pages_helper_spec.rb
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/pages.scss
```

As you can see we pass two arguments to the pages controller creation, in this case `home` and `about`. This will create:

* The `pages_controller.rb` file under `app/controllers`
  * The `pages_controller.rb` will have two methods, `home` and `about`
* A `pages` directory under the `app/views` directory with:
  * a `home.html.erb` file
  * an `about.html.erb` file
* Some code added to the `config/routes.rb` file

Before we start editing the code, let's fire up the server and try to navigate to the home page:

```bash
$ rails server
=> Booting Puma
=> Rails 6.1.3.2 application starting in development
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.3.2 (ruby 2.7.2-p137) ("Sweetnighter")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 1
* Listening on http://0.0.0.0:3000
Use Ctrl-C to stop   
```

After starting the server we can navigate to:

* http://localhost:3000/pages/home
* http://localhost:3000/pages/about

You should see something like the following:

![Pages About](/about_pages_controller.png)

Now let's break down the code a bit. First we start with the controller:

`app/controllers/pages_controller.rb`

```ruby
class PagesController < ApplicationController
  def home
  end

  def about
  end
end
``` 

As you can see the controller is just a Ruby class, inheriting from [ApplicationController](https://api.rubyonrails.org/v6.1.3.2/classes/ActionController/Base.html), that is in charge of handling
all of the requests, formats, exceptions, and so on.

> Action Controllers are the core of a web request in Rails. They are made up of one or more actions that are executed on request and then either it renders a template or redirects to another action. An action is defined as a public method on the controller, which will automatically be made accessible to the web-server through Rails Routes.

There is nothing much happening on the controller, now let's review the views. 

`app/views/pages/about.html.erb`

```html
<h1>Pages#about</h1>
<p>Find me in app/views/pages/about.html.erb</p>
```

The file above is the one that renders on the server when is requested by the URI `/pages/about`, just as discussed in previous lessons.

The way this works is:

* For each controller that Rails generates, you can have a view folder with the same name, in this case `pages`
* For each **method** or **action** on the controller, you can have a view, whether `html`, `js` or whatever you want
  * This applies only if you want to render or send something back to the client. We will discuss this on further lessons.

### Understanding the routes

Once we know how controllers and views are attached by Rails, let's jump to the routes, which is where everythin starts:

`config/routes.rb`

```ruby
Rails.application.routes.draw do
  get 'pages/home'
  get 'pages/about'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

> The Rails router recognizes URLs and dispatches them to a controller's action, or to a Rack application. It can also generate paths and URLs, avoiding the need to hardcode strings in your views.

So when your Rails application receives an incoming request for:

```
GET /pages/about
```

it asks the router to match it to a controller action. If the first matching route is:

```ruby
get 'pages/home'
```

the request is dispatched to the `pages controller's` `home` action. Raiils in this case deduce from the router definition which controller and which action.

> Rails uses snake_case for controller names here, if you have a multiple word controller like MonsterTrucksController, you want to use monster_trucks#show for example.

### Exercise

**Remember to perform a commit for each step below**

1. Try adding two more webpages to the application, `contact` and `faq`
2. The pages should respond to the URI:
  2.1 `/contact` should respond to the `contact` action on the `pages controller` 
  2.1 `/faq` should respond to the `faq` action on the `pages controller` 
3. Add some dummy html for each of the new views
4. Do the same for the `about` and `home` actions

You can find this resources useful to finish the exercise:

* https://guides.rubyonrails.org/routing.html#naming-routes

## Passing data from the controller

Most of the time we will be working with data coming from the controller, which we will print to the view. Take for example the `home` action:

**app/controllers/pages_controller.rb**

```ruby
class PagesController < ApplicationController
  def home
    @title = "Home"
  end
end
```

As you can see we are defining an instance variable for the `home` action. This is the way we can send data from the controller to the view. **This variable is only accesible for that action exclusively**

To read this variable on the view, we can simply do the following:

**app/views/pages/home.html.erb**

```ruby
<h1><%= @title %></h1>
<p>Find me in app/views/pages/home.html.erb</p
```

In the example above we are introducing a new concept, and is how we can embed Ruby code into the HTML, the syntax is as follows:

```ruby
<%= any_ruby_code_you_want_to_include_in_here %>
```

* We have an opening `<%=` and a closing `%>`. 
* The `=` is needed when you want to actually print the result into the template. For example if you were iterating a collection of elements, you won't include those:

```ruby
<% users.each do |user| %>
  <p><%= user.name %></p>
<% end %>
```

### Exercise

**Remember to perform a commit for each step below**

1. Add a `@title` variable to each of the actions and print them back to the html view
2. What happen if you try to access a variable that is not declared on the controller action?

You can read more on layouts and views [here](https://guides.rubyonrails.org/layouts_and_rendering.html). **We highly recommend you do it.**

## Layouts

Ruby on Rails is all about DRY, and is really pushes this with many of the conventions it has. One of them is the usage of layouts to prevent duplication of
html code in this case.

* All layouts live under the `app/views/layouts` directory
* You can have multiple layouts for differente behaviors
* If you name a layout the same as a controller, for example `pages.html.erb`. The `pages controller` will use that automatically
* The `app/views/layouts/application.html.erb` is the default for every controller action.

![Application Layout](/application_layout.png)

[Enlarge Image](/application_layout.png)

Within a layout, you have access to three tools for combining different bits of output to form the overall response:

* [Asset tags](https://guides.rubyonrails.org/layouts_and_rendering.html#asset-tag-helpers)
* [yield](https://guides.rubyonrails.org/layouts_and_rendering.html#understanding-yield) and [content_for](https://guides.rubyonrails.org/layouts_and_rendering.html#using-the-content-for-method)
* [Partials](https://guides.rubyonrails.org/layouts_and_rendering.html#using-partials)

Any change you make to the `app/views/layouts/application.html.erb` will affect the entire site, or at least to the ones responding to this layout.

### Exercise

**Remember to perform a commit for each step below**

1. Using the `content_for` directive set the `title` for every action on the pages controller following this format -> **Birdie::Home**

You can read more on layouts and views [here](https://guides.rubyonrails.org/layouts_and_rendering.html). **We highly recommend you do it.**

## The Root Route

As you may already notice, if you go to the root path for the application, there is a `Yay! You're on Rails` landing page. But we need to change that
to have a real root page for our application. We will be using the `home` action to achieve this:

1. Go to the `config/routes.rb`
2. Inside the file enter the following:
```ruby
Rails.application.routes.draw do
  get 'pages/about'

  root to: 'pages#home'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

The code above will tell Rails to respond with the `home` action from the `pages controller` each time someone requests the root path. As you can see we also removed the previous home URI. You don't actually have to do that, but just to avoid havind two actions that respond to a single action.

Remember you can always track the code [here](https://github.com/kurenn/birdie/tree/pages-controller)

### Resources

* [How to Use Rails Layouts & Display The Current Action Name](https://www.youtube.com/watch?v=KKg63bQocmw&ab_channel=JesusCastello)
* [ActionController::Base](https://api.rubyonrails.org/v6.1.3.2/classes/ActionController/Base.html)
* [Layouts and Rendering](https://guides.rubyonrails.org/layouts_and_rendering.html)
* [Sending Data Between Controllers and Views](https://gorails.com/episodes/sending-data-between-controllers-and-views?autoplay=1)
* [A look into routing](https://gorails.com/episodes/a-look-into-routing?autoplay=1)
* [Routes & Route Types](https://gorails.com/episodes/rails-for-beginners-part-5-routes-and-route-types?autoplay=1)
* [The Root Route](https://gorails.com/episodes/rails-for-beginners-part-6-the-root-route?autoplay=1)
