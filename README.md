# Ruby Object Remembrance

## Objectives

1.  Explain the concept of remembrance in object-oriented programming.
2.  Use class variables to remember, or store, instances of a class that are produced.

## Introduction

Let's say we're building a command line game in which players play various
rounds until a final tally determines the winner. Or creating an app in which we
want to store a list of all of the users who sign up. Or building a program that
helps users track and store the passwords for their various accounts.

In all of these situations, and many more we can imagine, our application needs
a way to store or remember a collection of class instances. Whether they are
instances of a `Game`, `User` or `Password` class, all of these examples would
require our program to keep track of instances that are created.

Luckily for us, Ruby allows us to do so by using class variables to store new
instances as soon as they are created. Let's take a look together.

## Using class variables to store instances of a class

Imagine we are building an app that manages a user's music. Our app should keep
track of all of the songs a user enters and allow our user to browse their
existing songs.

Let's take a look at the following class:

```ruby
class Song

  attr_accessor :name

  def initialize(name)
    @name = name
  end
end
```

With this code, we can create a new song like this:

```ruby
hotline_bling = Song.new("Hotline Bling")
```

Let's go ahead and create another song:

```ruby
thriller = Song.new("Thriller")
```

Uh-oh. Our user wants to browse their songs now and select one to play.
Currently, our code in the `Song` class has no way to keep track of the songs we
just created and display them back to the user.

### Creating the Class Variable

Let's take a step back and think about the concept of responsibility. Whose job
is it to know about _every instance of the `Song` class_? We have two choices
right now: an instance of the song class or the `Song` class itself.

It is not the responsibility of an individual song to know about all of the
other songs. Keeping track of all of the songs that it creates, however, fits
right into the purview of the Song class.

So, how can we tell the `Song` class to keep track of every instance that it
creates? We use a class variable.

Let's create a class variable, `@@all`, that will store every instance of the
`Song` class. Recall that `@@` before a variable name is how we define a class
variable.

```ruby
class Song

  @@all = []

  attr_accessor :name

  def initialize(name)
    @name = name
  end
end
```

Notice that we set our class variable equal to an empty array. Arrays are
perfect for storing lists of data, so we'll use an array to store our lists of
`Song` instances.

Now that our class is _set up_ to store the instances that it produces, we have
to ask: _how_ does it store these instances?

#### Adding Instances to the `@@all` Array

Before we can answer this question, we should ask another. _When_ should the
`Song` class become aware of, or store, an instance of itself?

This should happen at the time on instantiation––when a new song gets created,
it should be immediately stored by our `Song` class' `@@all`class variable.

We can implement this by simply adding the new instance that gets created into
the array stored in `@@all` _inside our `#initialize` method._

Let's take a look:

```ruby
class Song

  @@all = []

  attr_accessor :name

  def initialize(name)
    @name = name
    @@all << self
  end
end
```

In `#initialize` we use the `self` keyword to refer to the new object that has
just been created by `#new`. Remember that when `#new` is called, it creates a
new instance of the class and then calls `#initialize` on that new instance. So,
`#initialize` is technically an instance method. Inside an instance method we
are in what is called **method scope** and `self` will refer to whichever
instance the method is being called on.

We push `self` into the array that is stored in `@@all`. In this way, the
`@@all` class variable will point to an ever-growing array that contains every
instance of the `Song` class that gets created.

#### Our Code in Action

Let's see what happens when we actually execute the code we've written:

```ruby
ninety_nine_problems = Song.new("99 Problems")
thriller = Song.new("Thriller")
```

Now that we've created some songs, let's ask our `Song` class to show us all of
the instances that we just created:

```ruby
Song.all
  => NoMethodError: undefined method 'all' for Song:Class
```

Uh-oh, looks like we don't have a class method to access the contents of the
`@@all` array. Just like how we've built reader methods that expose the value of
instance variables, we need to build a method that will expose, or make
accessible outside of the class, the value of a class variable.

Let's build one now.

#### Building a Class Method to Access a Class Variable

Let's call our class method `#all`. All it needs to do is return the `@@all`
variable. Remember that the last line of any method in Ruby is automatically
_returned_. So simply putting `@@all` in `self.all` is that method's purpose.

```ruby
class Song

  @@all = []

  attr_accessor :name

  def initialize(name)
    @name = name
    @@all << self
  end

  def self.all
    @@all
  end
end
```

Recall that to define a **class** method we use the `def self.method_name`
syntax. The `self.` before the method name is a reminder that the method will
not be running on _a_ particular `Song` instance, but will be acting as the
factory from which all `Song`s are made: the `Song` class.

With a new method defined, we can try again to call our class method on the
`Song` class:

```ruby
Song.all
```

This should output something like:

```ruby
irb(main):020:0> Song.all
=> [#<Song:0x00007fd9910c45a0 @name="99 Problems">, #<Song:0x00007fd9900dba58 @name="Thriller">]
```

#### Building off of `all`

Implement a class method called `print_all_song_names` which prints out all the
names of the `Song`s that the class knows about. Use the `all` class method and
build off of it!

## Conclusion

We did it! We used a class variable to store a collection of instances of that
class. We added new instances to this storage container every time a new
instance was created with the help of the `self` keyword in our `#initialize`
method. Lastly, we wrote a class method to access and print out the name of
each song instance stored in our class variable.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/ruby-remembering-objects-readme' title='Ruby Object Remembrance'>Ruby Object Remembrance</a> on Learn.co and start learning to code for free.</p>
