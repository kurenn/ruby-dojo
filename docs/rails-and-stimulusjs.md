---
title: Rails & StimulusJS
date: 2021-04-15
slug: rails-and-stimulusjs
---

## What are you going to learn ?

* Installing stimulus

* Using stimulus
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
