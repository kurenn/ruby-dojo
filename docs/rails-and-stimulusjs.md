---
title: Rails & StimulusJS
date: 2021-04-15
slug: rails-and-stimulusjs
---

## What are you going to learn ?

* Installing stimulus

* Using stimulus

* best practices and examples 
## What is Stimulus.js

[Stimulus.js](https://stimulusjs.org/) is a lightweight JavaScript framework from the creators of Rails.

Use **Stimulus** to seamlessly integrate unobtrusive JavaScript into a Rails application. Think of it like adding **React** or **Vue** on top of a Ruby on Rails application, but a lot more lightweight.

Since **Rails 6**, **JavaScript** is no longer a part of the asset pipeline and instead integrates using Webpacker by default. The code lives in the `app/javascript` folder and is loaded into the body of the application layout via the `javascript_pack_tag`.

## Installing Stimulus

Because Rails 6 JS is loaded via Webpacker, any JavaScript modules or external libraries are not added through gems or the vendor folder.
Each package is loaded via a package manager, such as yarn or npm and imported into the application. To add Stimulus into the app, first install it using a package like yarn:

```bash
yarn add stimulus 
```
Once Stimulus is installed, import it to the application via Webpack. The ``app/javascript/packs/application.js`` file is typically used to just reference the different imported plugins — it should not have individual JS.

First, create the ``controllers`` folder and then, add an ``index.js`` file inside. Then, reference the folder inside the ``application.js``.

```bash
mkdir app/javascript/controllers
touch app/javascript/controllers/index.js
```

```javascript
// app/javascript/packs/application.js
import 'controllers'
```
It is possible to simply import the name of the folder, because Webpacker will read the index.js file and imported folder by default.

Import the Stimulus starter files. The ``context`` component makes it possible to read from and include every file from inside the ``controllers`` folder.
```javascript
// app/javascript/controllers/index.js

import { Application } from "stimulus"
import { definitionsFromContext } from "stimulus/webpack-helpers"

const application = Application.start()
const context = require.context(".", true, /\.js$/)
application.load(definitionsFromContext(context))
```
## Using Stimulus
### Setup
The first step is to create a page where Stimulus.js will access the DOM.
Typically, this is done to interact with the database via a specific model or to use concurrently with websockets (via Action Cable). But for this demo, I’ll be working with a static page controller that will serve as the home page.
```bash
rails g controller pages home
```
```ruby
#config/routes.rb

root "pages#home"
```
Then, I’m adding some content to be manipulated on the home page.

```html
<!-- app/views/pages/home.html.erb -->
<div class=”btn btn-primary”>Say Hello</div>
```
Now, you’re ready to integrate Stimulus.

###  Create the Stimulus controller
Create the Stimulus controller named ``hello_controller.js``, inside the JavaScript folder (Note: not inside the ``app/controllers`` folder), then, add the boilerplate code.

There is no Rails generator for this, but I hope it can be incorporated in future releases.

```javascript
// app/javascript/controllers/hello_controller.js

import { Controller } from 'stimulus'; 
export default class extends Controller {
  connect() {
    console.log("hello from StimulusJS")
  }
}
```
This is an example of an **identifier**, **this is the name you use to reference a controller class in HTML**.

When you add a ``data-controller`` attribute to an **element**, Stimulus reads the identifier from the attribute’s value and creates a new instance of the corresponding controller class.

For example, this element has a controller which is an instance of the class defined in ``controllers/hello_controller.js``:

```html
<div data-controller="hello">
  <div class="btn btn-primary">Say Hello</div>
</div>
```

Once the data attribute is added, you can check the connection in the JS console.

### Actions 
*Actions are how you handle DOM events in your controllers*.
For this, I’ll be riffing off an example from the official Stimulus handbook. Let’s replace the header text with the classic programmer Hello World on button click.

he first step is to add a callback to my button div. This component of the data attribute is called the action descriptor. It creates a listener on click to call the greet action inside the hello controller.

```html
<div class="btn btn-primary" data-action="click->hello#greet">Say Hello</div>
```

The ``data-action`` value ``click->hello#greet`` is called an **action descriptor**. In this descriptor:

* ``click`` is the name of the DOM event to listen for.
* ``hello`` is the controller identifier.
* ``greet`` is the name of the method to invoke.

Next, add the action to the Stimulus controller.

```javascript
//app/javascript/controllers/hello_controller.js

export default class extends Controller {
  connect() {
    console.log("hello from StimulusJS")
  }
  greet() {
    console.log("click")
  }
}
```
### Changing the DOM using Targets

Finally, let’s add something in the DOM to be altered by the button click. The Stimulus framework makes it simple to reference elements, by designating them as targets inside of the controller.
Inside the HTML page, add the data-target attribute to the h1 element, referencing both the name of the controller and the name of the element.

```html
<h1 data-target="hello.heading">This is the home page</h1>
```
Then, inside the controller, create the data targets. Multiple targets can be added to the array.

```javascript
export default class extends Controller {
  static targets = ["heading"]
  [...]
}
```

Finally, amend the greet function to replace the innerHTML of the target on click:

```javascript
greet() {
   this.headingTarget.innerHTML = "Hello World"
}
```
## Writing better StimulusJS controllers

This article is explicitly not an introduction to Stimulus. The official documentation and Handbook are excellent resources that I will not be repeating here.

## What may go wrong
The common failure paths I’ve seen when getting started with Stimulus:

### Making controllers too specific (either via naming or functionality)

It’s tempting to start out writing one-to-one Stimulus controllers for each page or section where you want JavaScript. Especially if you’ve used React or Vue for your entire application view-layer. This is generally not the best way to go with Stimulus.

It will be hard to write beautifully composable controllers when you first start. That’s okay.

### Trying to write React in Stimulus

Stimulus is not React. React is not Stimulus. Stimulus works best when we let the server do the rendering. There is no virtual DOM or reactive updating or passing “data down, actions up”.

Those patterns are not wrong, just different and trying to shoehorn(force into an inadequate space) them into a Turbolinks/Stimulus setup will not work.

### Growing pains weaning off jQuery

Writing idiomatic ES6 can be a stumbling block for people coming from the old days of jQuery.

The native language has grown leaps and bounds, but you’ll still scratch your head from time to time wondering if people really think that:

```javascript
new Array(...this.element.querySelectorAll(".item"));
```
is an improvement on ``$('.item')``. (I’m right there with you, but I digress…)
## How to write better Stimulus controllers

After taking Stimulus for a test drive and making a mess, I revisited the Handbook and suddenly I saw the examples in a whole new light.

For instance, the Handbook shows an example for lazy loading HTML:

```html
<div data-controller="content-loader" data-content-loader-url="/messages.html">
  Loading...
</div>
```

Notice the use of data-content-loader-url to pass in the URL to lazily load.

The key idea here is that you aren’t making a MessageList component. You are making a generic async loading component that can render any provided URL.

Instead of the mental model of extracting page components, you go up a level and build “primitives” that you can glue together across multiple uses.

You could use this same controller to lazy load a section of a page, or each tab in a tab group, or in a server-fetched modal when hovering over a link.

You can see real-world examples of this technique on sites like GitHub.

(Note that GitHub does not use Stimulus directly, but the concept is identical)


![alt text](https://boringrails.com/images/github-lazy-load.png "github example")

The GitHub activity feed first loads the shell of the page and then makes an AJAX call that fetches more HTML to inject into the page.

```html
<!-- Snippet from github.com -->
<div class="js-dashboard-deferred" data-src="/dashboard-feed" data-priority="0">
  ...
</div>
```
GitHub uses the same deferred loading technique for the “hover cards” across the site.



![alt text](https://boringrails.com/images/github-hover-card.gif "github example 2")

```html
<!-- Snippet from github.com -->
<a
  data-hovercard-type="user"
  data-hovercard-url="/users/swanson/hovercard"
  href="/swanson"
>
  swanson
</a>
```
By making general-purpose controllers, you start the see the true power of Stimulus.

Level one is an opinionated, more modern version of jQuery on("click") functions.

Level two is a set of “behaviors” that you can use to quickly build out interactive sprinkles throughout your app.

### Example: toggling classes

One of the first Stimulus controllers you’ll write is a “toggle” or “show/hide” controller. You’re yearning for the simpler times of wiring up a click event to call ``$(el).hide()``.

Your implementation will look something like this

```javascript
// toggle_controller.js
import { Controller } from "stimulus";

export default class extends Controller {
  static targets = ["content"];

  toggle() {
    this.contentTarget.classList.toggle("hidden");
  }
}
```
And you would use it like so:

```html
<div data-controller="toggle">
  <button data-action="toggle#toggle">Toggle</button>
  <div data-target="toggle.content">
    Some special content
  </div>
</div>
```
To apply the lessons about building more configurable components that the Handbook recommends, rework the controller to not hard-code the CSS class to toggle.

```javascript
// toggle_controller.js
import { Controller } from "stimulus";

export default class extends Controller {
  static targets = ["content"];

  toggle() {
    this.contentTargets.forEach((t) => t.classList.toggle(data.get("class")));
  }
}
```

The controller now supports multiple targets and a configurable CSS class to toggle.

You’ll need to update the usage to:

```html
<div data-controller="toggle" data-toggle-class="hidden">
  <button data-action="toggle#toggle">Toggle</button>
  <div data-target="toggle.content">
    Some special content
  </div>
</div>
```

his might seem unnecessary on first glance, but as you find more places to use this behavior, you may want a different class to be toggled.

Consider the case when you also needed some basic tabs to switch between content.

```html
<div data-controller="toggle" data-toggle-class="active">
  <div
    class="tab active"
    data-action="click->toggle#toggle"
    data-target="toggle.content"
  >
    Tab One
  </div>
  <div
    class="tab"
    data-action="click->toggle#toggle"
    data-target="toggle.content"
  >
    Tab Two
  </div>
</div>
```
You can use the same code. New feature, but no new JavaScript! The dream!
