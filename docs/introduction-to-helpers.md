---
title: Introduction to Helpers
date: 2021-04-15
slug: introduction-to-helpers
---

## What are you going to learn?

* Understand how helpers work on Rails
* How to create helpers
* Acknowledge the most common built-in helper methods


Before starting building a feature, it is important to create a new branch, let's do that:

```bash
$ git checkout -b helpers
```

Helpers in Rails are methods that are mostly used in your views, to share behavior among them. There are a bunch of built-in
helper methods.

> The Rails framework provides a large number of helpers for working with assets, dates, forms, numbers and model objects, to name a few. These helpers are available to all templates by default.

In addition to the ones provided, you can create custom ones with complicated logic or to reuse some code.

When working with helpers, consider the following:

* They are all located under the `app/helpers` directory
* They are all modules, which are injected into the views.
* By default, all helpers are accessible by the views.
* By default, each controller will include all helpers

### Built-in Helpers

There are a lot of buil-in helpers that will enable you to code faster while sharing some of the most common behavior across your application:

#### link_to(name = nil, options = nil, html_options = nil, &block)

Creates an anchor element of the given name using a URL created by the set of options.

```ruby
link_to "Home", root_path, class: 'btn'
# <a href='/' class='btn'>Home</a>
```

When working with this helper, the second parameter refers to the path, which you can get them from running the `rails routes` command:

```bash
 Prefix Verb   URI Pattern                                                                                       Controller#Action
   about GET    /about(.:format)                                                                                  pages#about
 contact GET    /contact(.:format)                                                                                pages#contact
     faq GET    /faq(.:format)                                                                                    pages#faq
    root GET    /                                                                                                 pages#home
```

By adding the `path` suffix to the prefix column, will map the correct URI.

https://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to

#### button_to(name = nil, options = nil, html_options = nil, &block)

Generates a form containing a single button that submits to the URL created by the set of options. This is the safest method to ensure links that cause changes to your data are not triggered by search bots or accelerators.

```ruby
button_to('Destroy', 'http://www.example.com',
          method: :delete, remote: true, data: { confirm: 'Are you sure?', disable_with: 'loading...' })
# => "<form class='button_to' method='post' action='http://www.example.com' data-remote='true'>
#       <input name='_method' value='delete' type='hidden' />
#       <input value='Destroy' type='submit' data-disable-with='loading...' data-confirm='Are you sure?' />
#       <input name="authenticity_token" type="hidden" value="10f2163b45388899ad4d5ae948988266befcb6c3d1b2451cf657a0c293d605a6"/>
#     </form>"
```

https://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-button_to

#### number_to_currency(number, options = {})

Formats a number into a currency string (e.g., $13.65). You can customize the format in the options hash.

```ruby
number_to_currency(1234567890.50, unit: "R$", separator: ",", delimiter: "", format: "%n %u")
# => 1234567890,50 R$
```

https://api.rubyonrails.org/classes/ActionView/Helpers/NumberHelper.html#method-i-number_to_currency

#### number_to_human(number, options = {})

Pretty prints (formats and approximates) a number in a way it is more readable by humans (e.g.: 1200000000 becomes “1.2 Billion”). This is useful for numbers that can get very large (and too hard to read).

```ruby
number_to_human(1234567, precision: 1,
                        separator: ',',
                        significant: false)                   # => "1,2 Million"
```

https://api.rubyonrails.org/classes/ActionView/Helpers/NumberHelper.html#method-i-number_to_human

#### number_with_delimiter(number, options = {})

Formats a number with grouped thousands using delimiter (e.g., 12,324). You can customize the format in the options hash.

```ruby
number_with_delimiter(98765432.98, delimiter: " ", separator: ",")
 # => 98 765 432,98
```

https://api.rubyonrails.org/classes/ActionView/Helpers/NumberHelper.html#method-i-number_with_delimiter

#### distance_of_time_in_words(from_time, to_time = 0, options = {})

Reports the approximate distance in time between two Time, Date or DateTime objects or integers as seconds.

```ruby
from_time = Time.now
distance_of_time_in_words(from_time, from_time + 50.minutes)                                # => about 1 hour
distance_of_time_in_words(from_time, 50.minutes.from_now)                                   # => about 1 hour
```

https://api.rubyonrails.org/classes/ActionView/Helpers/DateHelper.html#method-i-distance_of_time_in_words

#### time_ago_in_words(from_time, options = {})

Like `distance_of_time_in_words`, but where to_time is fixed to `Time.now`

```ruby
time_ago_in_words(3.minutes.from_now)                 # => 3 minutes
```

https://api.rubyonrails.org/classes/ActionView/Helpers/DateHelper.html#method-i-time_ago_in_words

#### distance_of_time_in_words_to_now(from_time, options = {})

Alias for: time_ago_in_words

```ruby
distance_of_time_in_words_from_now(3.minutes.from_now)                 # => 3 minutes
```

https://api.rubyonrails.org/classes/ActionView/Helpers/DateHelper.html#method-i-distance_of_time_in_words_to_now

#### pluralize(count, singular, plural_arg = nil, plural: plural_arg, locale: I18n.locale)

Attempts to pluralize the singular word unless count is 1. If plural is supplied, it will use that when count is > 1, otherwise it will use the Inflector to determine the plural form for the given locale, which defaults to I18n.locale.

```ruby
pluralize(3, 'person', plural: 'users')
# => 3 users
```

https://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html#method-i-pluralize

#### truncate(text, options = {}, &block)

Truncates a given text after a given `:length` if text is longer than `:length` (defaults to 30). The last characters will be replaced with the :omission (defaults to "…") for a total length not exceeding `:length`.

```ruby
truncate("And they found that many people were sleeping better.", length: 25, omission: '... (continued)')
# => "And they f... (continued)"
```

https://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html#method-i-truncate

#### simple_format(text, html_options = {}, options = {})

Returns text transformed into HTML using simple formatting rules. Two or more consecutive newlines(`\n\n` or `\r\n\r\n`) are considered a paragraph and wrapped in `<p>` tags. One newline (`\n` or `\r\n`) is considered a linebreak and a `<br />` tag is appended. This method does not remove the newlines from the text.

```ruby
simple_format(my_text, {}, wrapper_tag: "div")
# => "<div>Here is some basic text...\n<br />...with a line break.</div>"
```

https://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html#method-i-simple_format

#### stylesheet_link_tag(*sources)

Returns a stylesheet link tag for the sources specified as arguments. If you don't specify an extension, .css will be appended automatically.

```ruby
stylesheet_link_tag "style"
# => <link href="/assets/style.css" media="screen" rel="stylesheet" />

stylesheet_link_tag "style.css"
# => <link href="/assets/style.css" media="screen" rel="stylesheet" />

stylesheet_link_tag "http://www.example.com/style.css"
# => <link href="http://www.example.com/style.css" media="screen" rel="stylesheet" />
```

https://api.rubyonrails.org/classes/ActionView/Helpers/AssetTagHelper.html#method-i-stylesheet_link_tag

#### javascript_include_tag(*sources)

Returns an HTML script tag for each of the sources provided.

Sources may be paths to JavaScript files. Relative paths are assumed to be relative to assets/javascripts, full paths are assumed to be relative to the document root. Relative paths are idiomatic, use absolute paths only when needed.

```ruby
javascript_include_tag "xmlhr"
# => <script src="/assets/xmlhr.debug-1284139606.js"></script>

javascript_include_tag "xmlhr", host: "localhost", protocol: "https"
# => <script src="https://localhost/assets/xmlhr.debug-1284139606.js"></script>

javascript_include_tag "template.jst", extname: false
# => <script src="/assets/template.debug-1284139606.jst"></script>
```

https://api.rubyonrails.org/classes/ActionView/Helpers/AssetTagHelper.html#method-i-javascript_include_tag

#### image_tag(source, options = {})

Returns an HTML image tag for the source. The source can be a full path, a file, or an Active Storage attachment.

```ruby
image_tag("icon")
# => <img src="/assets/icon" />
image_tag("icon.png")
# => <img src="/assets/icon.png" />
image_tag("icon.png", size: "16x10", alt: "Edit Entry")
# => <img src="/assets/icon.png" width="16" height="10" alt="Edit Entry" />
```

https://api.rubyonrails.org/classes/ActionView/Helpers/AssetTagHelper.html#method-i-image_tag

### Exercise

**Remember to perform a commit for each step below**

1. In the `app/views/layouts/application.html.erb` add a `link_to` for each page we currently have. Use the `paths` provided by the router

### Custom Helpers

Writing your own helpers is a very common thing when working with Rails. You can use them to reuse or share behavior across multiple views.

All of your helpers are located under `app/helpers`

Every Rails application comes with a base helper module by default, it’s called `ApplicationHelper`.

**Remember all of the helpers you write are automatically included to all your views.**

You could write all of your helpers inside the `ApplicationHelper`, but we hihgly recommend you not to do it. This is just for organization
purposes.

Here is an example:

```ruby
module ApplicationHelper
  def format_title_for(user)
    return "Mr. #{user.name}" if user.male?

    "Ms. #{user.name}"
  end
end
```

And you can call this on any view like so:

```html
<%= forma_title_for(user) %>
```

### Exercise

**Remember to perform a commit for each step below**

1. Create a custom helper to provide a default title. In the last lesson we used `content_for` for this. 

The expected usage is as following:

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

### Resources

* [ActionController::Helpers](https://api.rubyonrails.org/classes/ActionController/Helpers.html)
* [How to use Rails Helpers](https://www.rubyguides.com/2020/01/rails-helpers/)
* [Action View Form Helpers](https://guides.rubyonrails.org/form_helpers.html)
* [Url Helper](https://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html)
* [Number Helper](https://api.rubyonrails.org/classes/ActionView/Helpers/NumberHelper.html)
* [Text Helper](https://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html)
* [Asset Tag Helper](https://api.rubyonrails.org/classes/ActionView/Helpers/AssetTagHelper.html)
