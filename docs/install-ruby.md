---
title: Install Ruby
date: 2021-04-15
slug: install-ruby
---

## In Ubuntu 20.04

This guide will cover installing a couple of things:

* ``ruby-install``: a lightweight way to install multiple Ruby versions on the same box.
* ``chruby``: a way to easily switch between those Ruby installs
* ``Ruby 2.7.1``: at the time of writing the newest current stable release of Ruby.
* ``Bundler``: a package dependency manager used in the Ruby community
* ``Rails 6.0.0``: at the time of writing the newest current stable release of Rails.

By the end of this guide, you will have these things installed and have some very, very easy ways to manage gem dependencies for your different applications / libraries, as well as having multiple Ruby versions installed and usable all at once.

### Housekeeping

First of all, we’re going to run sudo apt-get update so that we have the latest sources on our box so that we don’t run into any package-related issues, such as not being able to install some packages.

Next, we’ll run another command which will install the essential building tools that will be used to install Ruby:

```bash
sudo apt-get install build-essential
```
## ruby-install
The installation instructions can be found on the README of [ruby-install](https://github.com/postmodern/ruby-install#install), but I’ll repeat them here so you don’t have to go over there:

```bash
wget -O ruby-install-0.8.1.tar.gz https://github.com/postmodern/ruby-install/archive/v0.8.1.tar.gz
tar -xzvf ruby-install-0.8.1.tar.gz
cd ruby-install-0.8.1/
sudo make install
```

First we fetch the ruby-install file, extract it into a directory, then make it. You can verify that these steps have worked by running the following command:

```bash
ruby-install -V
```
If you see this, then you’ve successfully installed ruby-install:
```bash
ruby-install: 0.8.1
```

### Ruby

Our next step is to install Ruby itself, which we can do with this command:

```bash
ruby-install ruby 2.7.1
```

This command will take a couple of minutes, so grab your $DRINKOFCHOICE and go outside or something. Once it’s done, we’ll have Ruby 2.7.1 installed. In order to use this Ruby version, we’ll need to install chruby as well. The instructions [can be found in chruby’s README](https://github.com/postmodern/chruby#install) too, but I will reproduce them here:

```bash
wget -O chruby-0.3.9.tar.gz https://github.com/postmodern/chruby/archive/v0.3.9.tar.gz
tar -xzvf chruby-0.3.9.tar.gz
cd chruby-0.3.9/
sudo make install
```

After this has been installed, we’ll need to load chruby automatically, which we can do by adding these lines to your shells configuration file using the following command:

```bash
cat >> ~/.$(basename $SHELL)rc <<EOF
source /usr/local/share/chruby/chruby.sh
source /usr/local/share/chruby/auto.sh
EOF
```

In order for this to take effect, we’ll reload the shell

```bash
exec $SHELL
```

Alternatively, opening a new terminal tab/window will do the same thing.
To verify that chruby is installed and has detected our Ruby installation, run chruby. If you see this, then it’s working:

```bash
ruby-2.7.1
```

Now we need to make that Ruby version the default one for our system, which we achieve by creating a new file called `~/.ruby-version` with this content:

```bash
ruby-2.7.1
```

This file tells `chruby` which Ruby we want to use by default. To change the ruby version that we’re using, we can run `chruby ruby-2.7.1` for example – assuming that we have Ruby 2.7.1 installed first!

Did this work? Let’s find out by running ruby -v:

```bash
ruby 2.7.1p114 (2019-10-01 revision 67812) [x86_64-linux]
```

### Rails 

Now that we have a version of Ruby installed, we can install Rails. Because our Ruby is installed to our home directory, we don’t need to use that nasty sudo to install things; we’ve got write-access! To install the Rails gem we’ll run this command:

```bash
gem install rails 
```

This will install the rails gem and the multitude of gems that it and its dependencies depend on, including Bundler.

### Rails pre-requisites

Before we can start a new Rails app, there are a few more things that we need to install.

### JavaScript Runtime

Rails requires a JavaScript runtime to run Webpacker for its assets.

To fix this error install nodejs, which comes with a JavaScript runtime:

```bash
sudo apt-get install nodejs
```
On top of this, modern Rails uses yarn, a JavaScript package manager. We will need to install that too. The instructions are on the Yarn site but here they are too:

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

### SQLite3

First of all, we need to install libsqlite3-dev, which is the package for the development headers for SQLite3. We need this so that when bundle install runs as a part of rails new, it will be able to install the sqlite3 gem that is a default dependency of Rails applications.

```bash
sudo apt-get install libsqlite3-dev
```

And that’s it! Now you’ve got a Ruby on Rails environment where you can use to start your (first?) Rails application with such minimal effort.

A good read after this would be the [official guides for Ruby on Rails](http://guides.rubyonrails.org/).
