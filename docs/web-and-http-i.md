---
title: Web & HTTP I
date: 2021-04-15
slug: web-and-http-i
---

## What are you going to learn ?
* Make a simple HTTP server
* How HTTP and TCP work together
* Using Rack

## What is a Web server?

It accepts requests from clients(users), then it returns a response. Along this lesson we will be creating a super simple [Web Server](https://en.wikipedia.org/wiki/Web_server), to handle GET HTTP requests correctly.

## How HTTP and TCP work together

* **TCP** is a transport protocol that describes how a server and a client exchange data.

* **HTTP** is a request-response protocol that specifically describes how web servers exchange data with HTTP clients or web browsers. HTTP commonly uses TCP as its transport protocol. In essence, an HTTP server is a TCP server that "speaks" HTTP.

## Launch a TCP server

Ruby provides a [TCPServer](https://ruby-doc.org/stdlib-2.4.0/libdoc/socket/rdoc/TCPServer.html) class to create TCP connection.

```ruby
#tcp_server.rb
require 'socket'
server = TCPServer.new '0.0.0.0', 5678

while session = server.accept
  session.puts "Hello world! The time is #{Time.now}"
  session.close
end
```

In this example above, the server binds to port **5678** and waits for a client to connect. When that happens, it sends a message to the client, and then closes the connection. After it's done talking to the first client, the server waits for another client to connect to send its message to again.

```ruby
#tcp_client.rb
require 'socket'
server = TCPSocket.new 'localhost', 5678

while line = server.gets
  puts line
end

server.close
```

To connect to our server, we'll need a TCP client. This example client connects to the same port (5678) and uses `server.gets` to receive data from the server, which is then printed. When it stops receiving data, it closes the connection to the server and the program will exit.

When you start the server by running `ruby tcp_server.rb`, you can start the client in a separate tab to receive the server's message:

```bash
$ ruby tcp_client.rb
Hello world! The time is 2016-11-23 15:17:11 +0100
```
## A Minimal Ruby HTTP server

Enough talk. Now that we know how to create a TCP server in Ruby and what some HTTP requests and responses look like, we can build a minimal HTTP server. You'll notice that the web server looks mostly the same as the TCP server we discussed earlier. The general idea is the same, we're just using the HTTP protocol to format our message. Also, because we'll use a browser to send requests and parse responses, we won't have to implement a client this time.

```ruby
# http_server.rb
require 'socket'
server = TCPServer.new 5678

while session = server.accept
  request = session.gets
  puts request

  session.print "HTTP/1.1 200\r\n" # 1
  session.print "Content-Type: text/html\r\n" # 2
  session.print "\r\n" # 3
  session.print "Hello world! The time is #{Time.now}" #4

  session.close
end
```

After the server receives a request, like before, it uses `session.print` to send a message back to the client: Instead of just the server message, it prefixes the response with a status line, a header and a newline:

* The status line (HTTP 1.1 200\r\n) to tell the browser that the HTTP version is 1.1 and the response code is "200"

* A header to indicate that the response has a text/html content type (Content-Type: text/html\r\n)

* The newline (\r\n)

* The body: "Hello world! …"

Like before, it closes the connection after sending the message. We're not reading the request yet, so it just prints it to the console for now.

If you start the server and open http://localhost:5678 in your browser, you should see the "Hello world! …" string with the current time, like we received from our TCP client earlier. 🎉

## Serving a Rack app

Rack is the underlying technology behind nearly all of the web frameworks in the Ruby world. "Rack" is actually a few different things:

* An architecture - Rack defines a very simple interface, and any code that conforms to this interface can be used in a Rack application. This makes it very easy to build small, focused, and reusable bits of code and then use Rack to compose these bits into a larger application.

* A Ruby gem - Rack is is distributed as a Ruby gem that provides the glue code needed to compose our code.

Rack defines a very simple interface. Rack compliant code must have the following three characteristics:

* **It must respond to** `call`.

* **The** `call` **method must accept a single argument** - This argument is typically called `env` or environment, and it bundles all of the data about the request.

* The `call` **method must return an array of three elements** These elements are, in order, status for the HTTP status code, headers, and body for the actual content of the response.

A nice side effect of the `call` interface is that **procs** and **lambdas** can be used as Rack objects.

The following code sample shows a bare-bones Rack-compliant application that simply returns the text *"Hello World"*.

```ruby
require "rack"
require "thin"

class HelloWorld
  def call(env)
    [ 200, { "Content-Type" => "text/plain" }, ["Hello World"] ]
  end
end

Rack::Handler::Thin.run HelloWorld.new
```
In the sample we implement a class with a single instance method, call, that takes in the env and always returns a 200 (HTTP "OK" status), a "Content-Type" header, and the body of "Hello World".

Note that **Rack expects the body portion of the response array** to respond to each, so bare strings need to be wrapped in an array. More commonly, the body object will be some other type of IO object, rather than a bare string, so this wrapping is not needed in those cases.

Lastly, we can see that we've required thin, a Ruby web server, and by doing so we can use the Rack compliant handler provided by **Thin**. Thanks to the simple nature of the Rack interface, nearly all Ruby web servers have implemented a Rack handler and we can largely swap them out, without needing to change our application code. Hooray for useful abstractions!

## Using a lambda
Just for fun, and because we can, let's update the hello world sample to use a lambda, rather than a class:

```ruby
require "rack"
require "thin"

app = -> (env) do
  [ 200, { "Content-Type" => "text/html" }, ["<h2>Hello world! The time is #{Time.now}</h2>"] ]
end

Rack::Handler::Thin.run app
```

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can finde them under `/ruby-exercises/Module1/web-and-http-i`.

## Additional Resources

+ [Rack Repository](https://github.com/rack/rack)
+ [Rack Explained For Ruby Developers](https://www.rubyguides.com/2018/09/rack-middleware/)
+ [Rack tutorial](https://thoughtbot.com/upcase/videos/rack)
+ [Ruby Docs](https://www.ruby-doc.org/)
+ [Official Ruby Language Doumentation](https://ruby-doc.org/core-2.6/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)
