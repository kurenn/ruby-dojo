---
title: File Handling
date: 2021-04-15
slug: file-handling
---

## What are you going to learn?

* Understand how to create files with Ruby
* Understand how to read files with Ruby
* Be able to parse data from a file

Working with files with code is a very common pattern to deal with, on this lesson we will teach you how to create and manipulate them.

### Creating a file

Ruby includes a [File](https://ruby-doc.org/core-2.5.0/File.html) class to create, read and manipulate files. 

> A File is an abstraction of any file object accessible by the program and is closely associated with class IO.

This is how you would create a file with Ruby:

```ruby
File.open("the_filename.txt", "w")
# This code creates a file named the_filename.txt, but can be whatever you want
# The second argument is the mode on which you wan to open a file, in this case write.
```

Here is a list of available modes for a file:

* r - Read only. The file must exist.
* w - Create an empty file for writing.
* a - Append to a file.The file is created if it does not exist.
* r+ - Open a file for update both reading and writing. The file must exist.
* w+ - Create an empty file for both reading and writing.
* a+ - Open a file for reading and appending. The file is created if it does not exist.

For more information about the `open` method, you can check out the official [documentation](https://ruby-doc.org/core-2.5.0/File.html#method-c-open)

### Reading a file

To easily read a file, you can just:

```ruby
file = File.open("the_filename.txt", "r")
file.read # This reads the whole file and returns a string
file.rewind # Positions ios to the beginning of input, resetting lineno to zero. - https://ruby-doc.org/core-2.5.0/IO.html#method-i-rewind
file.readlines # Returns an array of each line of the file
```

In this case, the file must exist to be read.

You can also read each line of a file like this:

```ruby
File.readlines("fruits.txt").each do |line|
  puts line.upcase 
end
```

### Writing to a file

In order to write anything to a file you can do it like this:

```ruby
file = File.open("the_filename.txt", "w")
file.write("Hello World!\n")
file.close
```

## Exercises

Remember we have provided a repository with a bunch of exercises for you to complete. You can find it [here](https://github.com/kurenn/ruby-exercises)

You can find them under `/ruby-exercises/Module1/files`.

## Additional Resources

+ [Working with Files](https://www.vikingcodeschool.com/professional-development-with-ruby/working-with-files-in-ruby)
+ [Ruby Docs](https://www.ruby-doc.org/)
+ [TryRuby: Learn programming with Ruby](https://ruby.github.io/TryRuby/)
+ [Icalia Labs Internal Ruby Guides](https://github.com/IcaliaLabs/guides/tree/master/stack/ruby)