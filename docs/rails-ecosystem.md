---
title: Rails Ecosystem
date: 2021-04-15
slug: rails-ecosystem
---

Ruby on Rails is a framework to create web applications that follow the [Model-View-Controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) pattern, along with
a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architecture.

> Ruby on Rails makes it much easier and more fun. It includes everything you need to build fantastic applications, and you can learn it with the support of our large, friendly community.

## Rails Doctrine

As you will see on further lessons, Rails is full of conventions, philosophies and ideas that not only allow the framework to stay relevant, but to grow its impact and community.

Here are the 9 most important pillars of the [Rails Doctrine](https://rubyonrails.org/doctrine/):

1. [Optimize for programmer happiness](https://rubyonrails.org/doctrine/#optimize-for-programmer-happiness)
2. [Convention over Configuration](https://rubyonrails.org/doctrine/#convention-over-configuration)
3. [The menu is omakase](https://rubyonrails.org/doctrine/#omakase)
4. [No one paradigm](https://rubyonrails.org/doctrine/#no-one-paradigm)
5. [Exalt beautiful code](https://rubyonrails.org/doctrine/#beautiful-code)
6. [Provide sharp knives](https://rubyonrails.org/doctrine/#provide-sharp-knives)
7. [Value integrated systems](https://rubyonrails.org/doctrine/#integrated-systems)
8. [Progress over stability](https://rubyonrails.org/doctrine/#progress-over-stability)
9. [Push up is a big tent](https://rubyonrails.org/doctrine/#big-tent)

We highly recommend you go and read about each of this points, as they help you understand the philosophy behind Rails.

There are a lot of conventions in Rails, thet will help you organize your code better(MVC), and follow top-notch standards to integrate your applications with ease(REST).

## What is REST?

To understand what it means for a web application to follow a REST architecure, let's consider a social network application:

* The user visits the following address http://localhost:3000/posts/new from a web browser
* The web browser sends the HTTP request to the server. In this case the web server by Rails - [puma](https://puma.io/) -
* The server responds with an HTML document with the form and links
* The user submits the form with the post information
* The web browser sends another HTTP request to the server
* The server prcess the request and respond with a redirection or by rendering new html

*This cycle repeats as long as the user is using the application.*

### Uniform Resource Identifier (URI)

A URI is what the user writes on the web browser at the start of the cycle described above. Another way that is commonly called is a [URL](https://en.wikipedia.org/wiki/URL). The [URI]() is a general term to refer to either a location or a name.

A [URI](https://en.wikipedia.org/wiki/URL) is the identification for a server resource

### Resources

A **resource** is anything that can be identified by a URI. In the previous example the URI written by the user is the address of a resource that corresponds to a web page
in this case http://localhost:3000/posts/new. On a static website, for example, each web page is a resource.

In step number four, the part of the server that creates a Post is another resource. The HTML form used to submit the form has the address (URI) of your resource.

### Representations

The HTML document that the server returns to the client is a representation of a resource. A representation is an encapsulation of the information (state, data, or markup) of the resource, encoded using formats such as: XML, JSON, or HTML.

A resource can have one or more representations. Clients and servers use 'media types' to indicate the type of representation for the receiving party (the client or the server). Most websites or web applications use the HTML format with the media type **text/html**. Similarly when the user submits a form, the browser submits a representation using the URI and the media type **application/x-www-form-urlencoded**.

### Uniform Interface

Clients use the [Hypertext Transfer Protocol (HTTP)](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) to send requests to resources and get responses back. In the first step, the client sends a [GET request](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_message) to fetch an HTML document. In step four the client sends a [POST request](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_message) to create a new Post.

Both methods (GET and POST) belong to the HTTP uniform interface. The use of the uniform interface is so that requests and responses are self-describing and visible. In addition to the methods shown in the example, there are other methods such as **OPTIONS, HEAD, PUT, DELETE, TRACE, and CONNECT**.

## MVC a.k.a Model-View-Controller

Ruby on Rails uses the MVC architecture pattern to organiza the application. It separates an application into the following components:

* **Models** for handling data and business logic. Is the prime bastion of object-oriented goodness
* **Controllers** for handling the user interface and application. In other words, is the bridge to communicate the Model with the view
* **Views** for handling graphical user interface objects and presentation

![Model View Controller Diagram](mvc.png)

### Models

Models handles the representation of business logic and the stored information in the database.


Models uses ActiveRecord underneath. 

> An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data.

[ActiveRecord](https://www.martinfowler.com/eaaCatalog/activeRecord.html) is the [object-relational mapping](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) (ORM) supplied with Rails, and it's basically the M in MVC. Designed to handle all of an application's tasks that relate to the database, including:

* establishing a connection to the database server
* perform the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations on a database through abstractions. 

### Controllers

The controllers are the ones the orchestrate the link between the view layer and the model layer:

* processes the requests and responses
* handles server errors and send them back to the client
* gather data from a browser request in order to use create or update a model 

### Views

The view is in charge of presenting the logic to the final user. In Rails, everything that is sent to the web browser is handled by a view.

* no logic or processing should be performed at this layer
* information retrieval should be avoided 

A view is not only HTML, and may not contain Ruby code at all. However when working with Rails, this is not commonly the case, because
you probably will be working with dynamic data floating from the server to the client.

Keep in mind that a view and be almost any format, [JSON](https://www.json.org/json-en.html), [XML](https://en.wikipedia.org/wiki/XML), [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) or any other you face with.

## Structure of rails app

Let's explore how a new rails application looks like after running the new application command:

```bash
$ rails new my_app
```
The top-level directory will be something like:

```bash
$ cd my_app
$ tree -l
.
├── Gemfile
├── Gemfile.lock
├── README.rdoc
├── Rakefile
├── config.ru
├── app
│   ├── assets
│   │   ├── images
│   │   ├── javascripts
│   │   └── stylesheets
│   ├── controllers
│   │   └── concerns
│   ├── helpers
│   ├── mailers
│   ├── models
│   │   └── concerns
│   └── views
│       └── layouts
├── bin
├── config
│   ├── environments
│   ├── initializers
│   └── locales
├── db
├── lib
│   ├── assets
│   └── tasks
├── log
├── public
├── test
│   ├── controllers
│   ├── fixtures
│   ├── helpers
│   ├── integration
│   ├── mailers
│   └── models
├── tmp
│   └── cache
│       └── assets
└── vendor
    └── assets
        ├── javascripts
        └── stylesheets

38 directories
```

Rails has a place for everything, let's start with the files in the top of the application directory

| File     | Description |
|------------------|-------------|
| config.ru | Configures the Rack Webserver inteerface, either to create Rails application or to use Rack middleware |
| Gemfile | Specifies the dependencies (gems) inside our Rails application.   | 
| Gemfile.lock | Records the specific versions for each of your rails applications dependencies. This file is maintained by Bundler  | 
| Rakefile | Contains tasks to run tests, create documentation, and more. |
| README | By default this will contain general information about the Rails framework, but is recommended to use this file to create a nice introductory page so that future developers will know what our application does|

### App Folder

Most of our work will happen inside the `app` directory, as the main code for the application lives below this folder. 

```bash
$ tree app/
app
├── assets
│   ├── images
│   ├── javascripts
│   │   └── application.js
│   └── stylesheets
│       └── application.css
├── controllers
│   ├── application_controller.rb
│   └── concerns
├── helpers
│   └── application_helper.rb
├── javascript
│   └── channels
│   └── packs
├── mailers
├── models
│   └── concerns
└── views
    └── layouts
        └── application.html.erb
```

### Test Folder

Rails owes its stability to the use of tests, and because of that it provides a layout for testing your application inside the `test` directory. 

When writing models or controllers it will produce skeleton test code automatically, so if we explore the `test` directory we'll see an output that looks really similar to the `app` directory:

```bash
$ tree test
test
├── controllers
├── fixtures
├── helpers
├── integration
├── mailers
├── models
└── test_helper.rb
```

We will be using `rspec`, therefore this folder is no longer going to be used, instead tests (or specs) will live inside `spec` folder.

### Configuration Folder

Look at a sneak peek of this folder: 

```bash
config/
├── application.rb
├── boot.rb
├── database.yml
├── environment.rb
├── environments
│   ├── development.rb
│   ├── production.rb
│   └── test.rb
├── initializers
│   ├── assets.rb
│   ├── backtrace_silencers.rb
│   ├── cookies_serializer.rb
│   ├── filter_parameter_logging.rb
│   ├── inflections.rb
│   ├── mime_types.rb
│   ├── session_store.rb
│   └── wrap_parameters.rb
├── locales
│   └── en.yml
├── routes.rb
└── secrets.yml
```

As you can see this directory contains all our rails application needs. When we want to configure routes, database, create initializers, modified some locales or defined deployment instructions this is the folder that we are looking for. 

Some important things to notice about this folder

- Before running your application, Rails loads and executes `config/environment.rb` and `config/application.rb` files
- In addition Rails will also load a per-environment configuration file. This file lives in the `environments` directory and is where you place configuration options that vary depending on the environment
- If you have special requirements, maybe you're in favor of creating an staging enviroment, you can easily create it with rails. Just add a new configuration file (`config/staging.rb`) and a new section in the database configuration file, and you are ready to go! 

### Lib Folder

You can place here your files that extend the rails app functionality. Anything that isn't directly related to a model, view, or controller should be placed in `lib`. It is a good practice to create subdirectories inside `lib`, so you can group related functionality. 

You'll also find a `tasks` directory under `lib`. This is where you can write your own Rake tasks

### Logs Folder

As our application executes, it produces a bunch of useful logging information. This is stored by default in this directory. Here you'll find three main log files:

```bash
$ tree log/
log/
├── development.log
├── production.log
└── test.log
```
This log contains trace lines, timing statistics, cache information, database queries executed and so on

### Public Folder

This is the folder that the outside world can see, the external face of your application. Contains static files and compiled assets

### Bin Folder

Contains all necessary scripts to run your application,  and that means that this directory contains the Rails script. This is the script that is run when you execute `rails` command in the command line. Take notice that the first argument you pass to that script determines the function Rails will perform. 

### Tmp Folder

Just like the name says, this one will include temporal files (caches, pids and sessions files). Generally this files are cleaned up automatically by Rails, but ocassionally if things go wrong, you might need to look in here and delete old files

### Vendor Folder

This is a place for all third-party code. In a typical rails application this includes vendored gems 

### Documentation Folder

This directory is no longer part of the required directories, and it won't be created in your Rails app by default. But just for you know Rails provides a bundle of rake tasks to generate automatic documentation, and all the files generated will be placed inside the `doc` folder.

* `doc:app` will generate application documentation in HTML format
* `doc:rails` will provide documentation for the version of Rails you're runnning. Just note that this command will take a while. 
* `doc:guides` will provide oficial rails guides. This needs `redcarpet` gem, so make sure to to add it to Gemfile before you run this command.

## Exercises

For this, we will need you to make sure you have Rails installed on your machine:

* For Ubuntu click [here](https://gorails.com/setup/ubuntu/21.04)
* For Mac click [here](https://gorails.com/setup/osx/11.0-big-sur)
* For Windows click [here](https://gorails.com/setup/windows)

We highly recommend you go with Ubuntu or Mac, as windows has shown us to fail or present some nasty installation bugs you may not be able to resolve.

## Additional Resources

+ [MVC in Rails](https://www.youtube.com/watch?v=lKUR4mu1M-U&ab_channel=GoRails)
+ [Ruby on Rails](https://rubyonrails.org/)
+ [Ruby on Rails Guides](https://guides.rubyonrails.org/)
+ [Rails API](https://api.rubyonrails.org/)