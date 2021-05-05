---
title: Setup Vscode
date: 2021-04-15
slug: setup-vscode
---

## Installation

If you don't yet have VS Code installed on your computer, head to [code.visualstudio.com](https://code.visualstudio.com/) to download it. You can open the dropdown to choose the versions you want to download, but usually, the big button should do the work.

## Extensions and settings Ruby on Rails Developers

Ruby is built with developer happiness in mind. However, if your editor is not correctly set up, you’re in for a painful ride. Finding the right extensions on VS Code can take you down a long trial-and-error path.
Here’s a list of powerful extensions for Ruby on Rails developers.

Extensions: 

* [Ruby (basic ruby support)](https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby) 
  
* [Solargraph (linter, autocomplete)](https://marketplace.visualstudio.com/items?itemName=castwide.solargraph) (You need to install the Ruby gem: ``gem install solargraph``)
  
* [Rubocop (linter)](https://marketplace.visualstudio.com/items?itemName=misogi.ruby-rubocop) (You need to install the Ruby gem: ``gem install rubocop``)
  
* [GitLens (git visibility)](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) 
 
* [Live Share (pair programming )](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare) 

* [RSpec Snippets (test snippets)](https://marketplace.visualstudio.com/items?itemName=ldrner.rspec-snippets-vscode) 

* [Erb (html.erb support)](https://marketplace.visualstudio.com/items?itemName=Riey.erb&ssr=false#overview) 

```Bash
code --install-extension rebornix.ruby
code --install-extension castwide.solargraph
code --install-extension misogi.ruby-rubocop
code --install-extension eamodio.gitlens
code --install-extension ms-vsliveshare.vsliveshare
code --install-extension ldrner.rspec-snippets-vscode
code --install-extension riey.erb

```
You can install these through command line, one by one using the command: ``code --install-extension $EXT_NAME``.

### Recommended settings for VSCode:

```JSON
  "ruby.useLanguageServer": true,
  "ruby.format": "rubocop",
  "ruby.intellisense": "rubyLocate",
  "ruby.useBundler": true,
  "ruby.rubocop.useBundler": true,
  "[ruby]": {
      "editor.defaultFormatter": "misogi.ruby-rubocop"
  },
  "editor.tabSize": 2,
  "files.insertFinalNewline": true,
  "telemetry.enableTelemetry": false,
  "window.titleBarStyle": "custom",
  "files.trimTrailingWhitespace": true,
```
Add these settings in ``File > Preferences > Settings`` (open the settings in json)
