---
title: Introduction to Rails
date: 2021-04-15
slug: introduction-to-rails
---

## What are you going to learn?

* Understand how Rails works on the surface
* Usage of basic Rails commands

## Understanding the CLI

As you already experienced from the last lesson, there a bunch of conventions and commands that Rails provides in order to create
any kind of resource in our application, whether is a `controller` a `model` or a `view`.

Even though you can do it manually, we highly recommend you to familiarize with these and other commands in upcoming lessons.

### Basic Rails Commands

The following commands are absolutely critical to your everyday usage.

* rails new app_name
* rails server
* rails generate
* rails console
* rails dbconsole
* rake
* bundle install

All commands can run with -h or --help to list more information.

### rails new

This creates a new Rails application, which is probably the first thing we'll want to do after installing Rails. 

```bash
rails new hacker_store
```

The former will give you the basic structure to immediately get started. 

```bash
❯ rails new hacker_store
      create
      create  README.rdoc
      create  Rakefile
      create  config.ru
      create  .gitignore
      create  Gemfile
      create  app
      create  app/assets/javascripts/application.js
      create  app/assets/stylesheets/application.css
      create  app/assets/images/.keep
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      create  app/views/layouts/application.html.erb
      create  app/mailers/
      create  app/models/
      ....
      create  bin
      create  bin/bundle
      create  bin/rails
      create  bin/rake
      create  bin/setup
      create  config
      create  config/routes.rb
      create  config/application.rb
      create  config/environment.rb
      create  config/secrets.yml
      create  config/environments
      ....
      create  config/initializers
      .....
      create  config/locales
      create  config/locales/en.yml
      create  config/boot.rb
      create  config/database.yml
      create  db
      create  db/seeds.rb
      create  lib
      create  lib/tasks
      ....
      create  log
      create  log/.keep
      create  public
      create  public/404.html
      create  public/422.html
      create  public/500.html
      create  public/favicon.ico
      create  public/robots.txt
      create  test/fixtures
      create  test/controllers
      create  test/mailers
      create  test/models
      create  test/helpers
      create  test/integration
      create  test/test_helper.rb
      ...
      create  tmp/cache
      ...
      create  vendor/assets/javascripts
      create  vendor/assets/stylesheets
      ...
         run  bundle install
```

You have the option to specify what kind of database and even what kind of source code management system your application is going to use! The main purpose of this is save a few minutes and many keystrokes


```bash
$ mkdir hacker_store && cd hacker_store
$ git init
Initialized empty Git repository in .git/
$ rails new . --git --database=postgresql
```

The only catch by using this options is that you have to make your application's directory first, then initialize your git repository and finally run `rails new` command

### rails server

This start the WEBrick server at port 3000. Go to your favorite browser and open `http://localhost:3000`, you will see your rails app running and responding to your requests.

You can also use the alias `s` to start the server:

```bash
$ rails s
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

For now on we will use this to start up our rails apps.

The server can be run on a different port through the `-p` option, this is very handy when you have multiple rails app running on your machine.
 

```bash
$ rails s -p 3001
=> Booting Puma
=> Rails 6.1.3.2 application starting in development
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.3.2 (ruby 2.7.2-p137) ("Sweetnighter")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 1
* Listening on http://0.0.0.0:3001
Use Ctrl-C to stop   
```
After that if you go `http://localhost:3001` you will see your rails app running

The server uses development environment as a default environment but this can be changed using `-e` flag

```bash
$ rails s -e production -p 4000
=> Booting Puma
=> Rails 6.1.3.2 application starting in production
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.3.2 (ruby 2.7.2-p137) ("Sweetnighter")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 1
* Listening on http://0.0.0.0:4000
Use Ctrl-C to stop   
```

Finally if you want to stop the server just go to the terminal and hit Ctrl + C.

```bash
^C
Gracefully stopping... (press Ctrl+C again to force)
```

### rails generate

This command is basically a code generator. It will create everything you need for your rails app: `controllers`, `models`, `mailers`, `scaffolds`, etc. 

Run `rails generate` with no arguments for usage information:

```bash
rails generate
Usage: rails generate GENERATOR [args] [options]

General options:
  -h, [--help]     # Print generator's options and usage
  -p, [--pretend]  # Run but do not make any changes
  -f, [--force]    # Overwrite files that already exist
  -s, [--skip]     # Skip files that already exist
  -q, [--quiet]    # Suppress status output

Please choose a generator below.

Rails:
  application_record                                                                                                                                                                                assets                                                                                                                                                                                            benchmark                                                                                                                                                                                         channel                                                                                                                                                                                           controller                                                                                                                                                                                        generator                                                                                                                                                                                         helper                                                                                                                                                                                            integration_test                                                                                                                                                                                  jbuilder                                                                                                                                                                                          job                                                                                                                                                                                               mailbox                                                                                                                                                                                           mailer                                                                                                                                                                                            migration                                                                                                                                                                                         model                                                                                                                                                                                             resource                                                                                                                                                                                          responders_controller                                                                                                                                                                             scaffold                                                                                                                                                                                          scaffold_controller                                                                                                                                                                               system_test                                                                                                                                                                                       task  
```

Also you can use `rails generate` for information on a particular generator: 

```bash
$ rails generate migration
Usage:
  rails generate migration NAME [field[:type][:index] field[:type][:index]] [options]

Options:
      [--skip-namespace], [--no-skip-namespace]  # Skip namespace (affects only isolated applications)
      [--skip-collision-check], [--no-skip-collision-check]  # Skip collision check
  -o, --orm=NAME                                 # Orm to be invoked
                                                 # Default: active_record
...
```

You can also use the shortcut command `g`:

```bash
rails g migration
Usage:
  rails generate migration NAME [field[:type][:index] field[:type][:index]] [options]

Options:
      [--skip-namespace], [--no-skip-namespace]  # Skip namespace (affects only isolated applications)
  -o, --orm=NAME                                 # Orm to be invoked
                                                 # Default: active_record
...
```

### rails destroy

This is the exact opposite of `generate`. It'll undo autogenerated files created by `generate` did:

```bash
$ rails g model user
      invoke  active_record
      create    db/migrate/20160131205854_create_users.rb
      create    app/models/user.rb

$ rails destroy model user
invoke  active_record
      remove    db/migrate/20160131205854_create_user.rb
      remove    app/models/user.rb
```

Just like any other rails command you can use `d` shortcut to invoke the destroy command, so the former could be also written like this:

```bash
$ rails d model user
invoke  active_record
      remove    db/migrate/20160131205854_create_users.rb
      remove    app/models/user.rb
```

### rails console

This command allows you to interact with your Rails application methods from the command line. It's basically the IRB for rails.

```bash
$ rails console
Running via Spring preloader in process 35
Loading development environment (Rails 6.1.3.2)
[1] pry(main)>
```
You can use the `c` shortcut for the same purposes

```
$ rails c
Running via Spring preloader in process 35
Loading development environment (Rails 6.1.3.2)
[1] pry(main)>
```

For now on we'll use this shortcut.  Just as the `rails server` command the console will run in a development environment by default, but you can also specify another environment:

```
$ rails c -e test
Running via Spring preloader in process 35
Loading test environment (Rails 6.1.3.2)
[1] pry(main)>
```

If you want to test out some code **without changing any data** `--sandbox` is your friend

```
$ rails c --sandbox
Loading development environment in sandbox (Rails 6.1.3.2)
Any modifications you make will be rolled back on exit
[1] pry(main)>
```

To leave the console just type `exit` or `quit`  

### rails dbconsole

This is the forgotten friend of `rails console` command, by running it, it will figure out which database your application is currently using and drops you into the corresponding command line interface.

So if we we're using a sqlite in your rails app, by running `rails dbconsole` in our terminal, we'll enter into sql command line interface:

```bash
rails dbconsole
SQLite version 3.8.10.2 2015-05-20 18:17:19
Enter ".help" for usage hints.
sqlite>
```

### rake

Rake is a utility similar to Make in Unix, it stands for Ruby Make. In Rails, Rake is used for common administration tasks, you can get a list of Rake tasks available with `rake --tasks`.

From version 5 of Rails, this command migrate from `rake` to `rails`, just to keep things simple, so any `rake` command you see below would run under the `rails` command. 

**We highly recommend the rails command usage**

```bash
$ rails --tasks
rails about                              # List versions of all Rails frameworks and the environment
rails action_mailbox:ingress:exim        # Relay an inbound email from Exim to Action Mailbox (URL and INGRESS_PASSWORD required)
rails action_mailbox:ingress:postfix     # Relay an inbound email from Postfix to Action Mailbox (URL and INGRESS_PASSWORD required)
rails action_mailbox:ingress:qmail       # Relay an inbound email from Qmail to Action Mailbox (URL and INGRESS_PASSWORD required)
rails action_mailbox:install             # Installs Action Mailbox and its dependencies
rails action_mailbox:install:migrations  # Copy migrations from action_mailbox to application
rails action_text:install                # Copy over the migration, stylesheet, and JavaScript files
rails action_text:install:migrations     # Copy migrations from action_text to application
rails active_storage:install             # Copy over the migration needed to the application
rails app:template                       # Applies the template supplied by LOCATION=(/path/to/template) or URL
rails app:update                         # Update configs and some other initially generated files (or use just update:configs or update:bin)
rails assets:clean[keep]                 # Remove old compiled assets
rails assets:clobber                     # Remove compiled assets
rails assets:environment                 # Load asset compile environment
rails assets:precompile                  # Compile all the assets named in config.assets.precompile
rails cache_digests:dependencies         # Lookup first-level dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rails cache_digests:nested_dependencies  # Lookup nested dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rails db:create                          # Creates the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:create:all to create all databases in the config)....
rails db:drop                            # Drops the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:drop:all to drop all databases in the config). Witho...
rails db:environment:set                 # Set the environment value for the database
rails db:fixtures:load                   # Loads fixtures into the current environment's database
rails db:migrate                         # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rails db:migrate:down                    # Runs the "down" for a given migration VERSION
rails db:migrate:redo                    # Rolls back the database one migration and re-migrates up (options: STEP=x, VERSION=x)
rails db:migrate:status                  # Display status of migrations
rails db:migrate:up                      # Runs the "up" for a given migration VERSION
rails db:prepare                         # Runs setup if database does not exist, or runs migrations if it does
...
... and many many more... 
```

As you can see each task has a description, and it should help you to get going. You can also use `-T` flag to obtain the same results

#### rails about

Gives you a general overlook for your rails app, including ruby version, ruby gems, rails database adapter, schema version, etc. It is useful when you need some stats for an exiting Rails installation or to verify if your rails application has everything you need to work for

```bash
$ rails about
Rails version             6.1.3.2
Ruby version              ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-linux]
RubyGems version          3.1.4
Rack version              2.2.3
Middleware                Webpacker::DevServerProxy, Rack::MiniProfiler, ActionDispatch::HostAuthorization, Rack::Sendfile, ActionDispatch::Static, ActionDispatch::Executor, ActiveSupport::Cache::Strategy::LocalCache::Middleware, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, ActionDispatch::RemoteIp, Sprockets::Rails::QuietAssets, Rails::Rack::Logger, ActionDispatch::ShowExceptions, WebConsole::Middleware, ActionDispatch::DebugExceptions, ActionDispatch::ActionableExceptions, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActiveRecord::Migration::CheckPending, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ContentSecurityPolicy::Middleware, ActionDispatch::PermissionsPolicy::Middleware, Rack::Head, Rack::ConditionalGet, Rack::ETag, Rack::TempfileReaper, Warden::Manager, Bullet::Rack
Application root          /usr/src
Environment               development
Database adapter          postgresql
Database schema version   20210526054850
```

### rails db

This is the most common task of Rake, particulary `migrate` and `create` methods. Rails encourages agile practices, and that applies to our database schema, as it constantly evolves with each step of our application's code. With rails each of those steps are made possible through the use of migrations. Migrations we'll cover in later lessons.

For now you can read more on migrations [here](https://edgeguides.rubyonrails.org/active_record_migrations.html)

#### rails routes

This will show a complete list of available routes in your application, which is a useful overlook of the URL's in an application

```bash
$ rake routes
Prefix Verb   URI Pattern                  Controller#Action
    products GET    /products(.:format)          products#index
             POST   /products(.:format)          products#create
 new_product GET    /products/new(.:format)      products#new
edit_product GET    /products/:id/edit(.:format) products#edit
     product GET    /products/:id(.:format)      products#show
             PATCH  /products/:id(.:format)      products#update
             PUT    /products/:id(.:format)      products#update
             DELETE /products/:id(.:format)      products#destroy
  home_hello GET    /home/hello(.:format)        home#hello
home_goodbye GET    /home/goodbye(.:format)      home#goodbye
```
#### rails stats
We might be interested to see how much code we've written. And guess what?? There's a task for that:

```bash
$ rake stats
+----------------------+--------+--------+---------+---------+-----+-------+
| Name                 |  Lines |    LOC | Classes | Methods | M/C | LOC/M |
+----------------------+--------+--------+---------+---------+-----+-------+
| Controllers          |    277 |    217 |       9 |      40 |   4 |     3 |
| Helpers              |      6 |      5 |       0 |       1 |   0 |     3 |
| Jobs                 |      7 |      2 |       1 |       0 |   0 |     0 |
| Models               |    143 |    115 |      10 |       9 |   0 |    10 |
| Mailers              |      4 |      4 |       1 |       0 |   0 |     0 |
| Channels             |      8 |      8 |       2 |       0 |   0 |     0 |
| JavaScript           |   1778 |   1285 |       0 |      78 |   0 |    14 |
| Libraries            |     17 |      7 |       0 |       0 |   0 |     0 |
| System specs         |    119 |    103 |       0 |       0 |   0 |     0 |
| Model specs          |    195 |    154 |       0 |       0 |   0 |     0 |
+----------------------+--------+--------+---------+---------+-----+-------+
| Total                |   2554 |   1900 |      23 |     128 |   5 |    12 |
+----------------------+--------+--------+---------+---------+-----+-------+
  Code LOC: 1643     Test LOC: 257     Code to Test Ratio: 1:0.2
```

### bundle commands

Lets talk about [bundler](http://bundler.io)! If you're developing as a part of your team, you probably want every member of the team to use the same version of dependencies. The same happens when you deploy, you want to ensure that the version of the dependencies that you developed with are the ones actually used in production. Welll bundler take care of this for you! 

Bundler uses `Gemfile` to list all dependencies used in your application, a `Gemfile` for a new rails app would look something like this:

 *Note that we're using rails 6, if you're using a different version of rails, Gemfile would look a bit different. Also comments were removed for reading purposes*

```ruby
source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '2.7.2'

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '~> 6.1.1'
# Use postgresql as the database for Active Record
gem 'pg', '~> 1.1'
# Use Puma as the app server
gem 'puma', '~> 5.0'
# Use SCSS for stylesheets
gem 'sass-rails', '>= 6'
# Transpile app-like JavaScript. Read more: https://github.com/rails/webpacker
gem 'webpacker', '~> 5.0'
# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
gem 'jbuilder', '~> 2.7'
# Use Redis adapter to run Action Cable in production
gem 'redis', '~> 4.0'
# Use Active Model has_secure_password
# gem 'bcrypt', '~> 3.1.7'

# Use Active Storage variant
# gem 'image_processing', '~> 1.2'

# Reduces boot times through caching; required in config/boot.rb
gem 'bootsnap', '>= 1.4.4', require: false

# # Simple, efficient background processing for Ruby.
gem 'sidekiq', '~> 6.0', '>= 6.0.7'

gem 'on_container', '0.0.7'

group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]

  gem 'annotate'

  gem 'bullet'

  gem 'rspec-rails', '~> 4.0'
  gem 'spring-commands-rspec'

  # Use Pry as your rails console
  gem 'pry-rails', '~> 0.3.9'

  # Combine 'pry' with 'byebug'. Adds 'step', 'next', 'finish', 'continue' and 'break' commands to control execution.
  gem 'pry-byebug', '~> 3.8'

  gem 'factory_bot_rails', '~> 5.2'
  gem 'ffaker'
end

group :development do
  # Access an interactive console on exception pages or by calling 'console' anywhere in the code.
  gem 'web-console', '>= 4.1.0'
  # Display performance information such as SQL time and flame graphs for each request in your browser.
  # Can be configured to work on production as well see: https://github.com/MiniProfiler/rack-mini-profiler/blob/master/README.md
  gem 'rack-mini-profiler', '~> 2.0'
  gem 'listen', '~> 3.3'
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

group :test do
  gem 'shoulda-matchers', '~> 4.0'

  # Generate test coverage reports
  gem 'simplecov', '~> 0.17.1', require: false

  # Format test coverage reports for console output
  gem 'simplecov-console', '~> 0.7.2', require: false

  # System specs support gems
  gem 'capybara', '>= 2.15'
  gem 'webdrivers', '~> 4.0'
end
```

Some things to notice here:

* First line `source 'https://rubygems.org'` just specifies where to find new gems and new version of existing gems. 
* `gem 'rails', '~> 6.1.1'` specifies the rails version to use in the app
* The remaining lines list a few gems that you might consider using
* Some gems are placed in in groups named `:development`, `:test` or `:production` and only be available on those environments

#### Basic workflow

1. Specify your dependencies in a Gemfile project root
2. Install all of the required gems through `bundle install`
3. Thats it! 

Later if you want to update a new gem you can use `bundle update <gem_name>`, e.g: `bundle update devise` this will update `devise` gem to his latest version without updating any other gems.
 
#### Bundler Operators

Gemfile supports different operators but two are the most commonly used (In the previous `Gemfile` you might have catched them):

* `>=` - This basically means "greater or equal than". So in this case `gem 'uglifier', '>= 1.3.0'`, we can use any version `uglifier` that is greather or equal to `1.3.0` 
* `~>` - This one is more widely recommended. Essentially all the parts of the version, with the exception of the last part, must be matched exactly. So `~> 3.1.4` matches any version that starts with `3.1` and is not less than `3.1.4`. Similarly `~> 2.1` means any version string that starts with `2.1` and ends `3.0` 

#### Gemfile.lock

This is the companion file of Gemfile. This file ensures that other developers in your app use the exact same dependencies as you just installed. For this reason **it's important that this files gets checked into your version control system** because it will ensure that your developing team and deployment targets will be use the same configuration. 

## Additional Resources

+ [Ruby on Rails](https://rubyonrails.org/)
+ [Ruby on Rails Guides](https://guides.rubyonrails.org/)
+ [Rails API](https://api.rubyonrails.org/)