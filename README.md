---
tags:
level:
languages:
resources:
---

# Ruby Remembering Objects

## Outline

Remembering objects

We know a lot about OO
1. Classes
2. Instances
3. Initialize
4. Self / class vs instance scope
5. Class variables
6. Class methods

With that we can do something really cool.

Imagine wanting to build a music manager that kept track of all the songs you entered and let you browse them. Later we'll add more functionality to the application, but for now we'll keep it simple.

We've provided the base functionality of entering a song and creating instances, but what's missing is the core functionality of then being able to list out songs.

Somehow, as we enter song names into the cli and instantiate instances of songs, we need to keep track of all of them
So we can list them later? But how?

So first, where do we store songs? Whose responsibility is it to remember all the songs that were created? A specific songs or songs in general? Song class. But where? A class variable. Let's also build a reader.

Let's actually finish everything by stubbing out the last piece with some canned data a common technique

Now the last part, how the hell do we save a song? How do we add songs as they are created into that class variable?

So first, at what moment in our code do we have access to a newly born song? What represents the moment a song is created?
Initialize so that's where we are adding the newly born song into the class variable. Let's program what we know

Now the Last part, within initialize how do we grab the very song that was just born to add it to the class variable? What's the scope
Of intialize. Within instance scope, how do we refer to the object itself? What will self be?

Done.
