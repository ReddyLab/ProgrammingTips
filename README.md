# Programming Tips
## High Level

Much of the programming  we do falls into the realm of "batch processing" -- these are programs that are run with a set of data and result in a set of other data. Mathematically they are essentially functions. Often the programs can be broken down into sub-programs which may be further broken down, etc. We can think of the structure like a Directed-Acyclic Graph (DAG) with each vertex being a sub- (or sub-sub- etc.) program and each edge indicating a flow of data out of one sub-program into another.

This conception of our programs as DAGs can help guide us to write cleaner, more testable code.
Pure functions

In computer programming a "Pure Function" is a function that takes a set of arguments and returns one or more values. It doesn't have any "side effects"[0] and always returns the same outputs for a given set of inputs. Much like the program as a whole, these are mathematical functions. If a (programming) function relies on a side effect to work correctly (directly or indirectly) it is "impure".

Of course your whole program can't be pure because a program that doesn't accept any input or perform any output isn't very useful. So it's a good idea to isolate these impure parts from the pure ones.

Pure functions have several benefits.
- Easy to test
- easy(-ier) to reason about

There are a several way so isolate the impure portions. Here a few (not an exhaustive list)
- get the input values from a file, pass the values to the pure function, then write the function output to another file
- (Python) I/O objects like files are "iterable" -- pass the file object the the pure function and use only the "iterable" functionality. When testing create a test object that is also iterable which can be passed in just like the file. This is a specific case of:
- Wrap the impure thing in an "interface"[2] that can also be implemented by pure things and pass the wrapper in to the function. From within the pure function only use the interface for interacting with the passed-in object.

[0]  A side effect is a change to the environment outside the function itself. Common examples are print to the console[1], file input/out, and setting global variables.<br/>
[1] Printing to the console is generally harmless<br/>
[2] An interface is a set of methods an object<br/>

## Testing

As mentioned, pure functions make testing easier. This assumes you are testing! As your program grows in size testing becomes more and more important. Many language have testing frameworks, some even have them built in. Python has a unittest module that can be used to write tests, but the third-party framework pytest is very popular and pretty easy to use.

If you use a random number generator, make sure you have a way to seed it so program runs are repeatable. That way you can compare run-to-run outputs exactly instead of guessing that they are close enough and probably nothing went wrong.

## Source Control

Source control is The Reddy lab (and many other labs, probably) use GitHub, and this git for source control. You probably want one repository per projects

## Environment management

virtualenv or conda

## Version Pinning

For reproducibility reasons it's important that the version of any software you use is documented. However, not only do the software and libraries we use directly have versions, but they have their own dependencies, which have their own dependencies, etc. Fortunately many dependency managers, like pip or conda, have a way of outputting all the libraries they are using.

- pip freeze will output installed packages, along with their version numbers, in a way formatted for use with pip.  This works best in conjunction with an environment manager.
- conda list will output all installed packages installed using conda.

## Misc

Automate early and often

https://death.andgravity.com/aosa

Name things by what they do, their purpose, not by what they are.

Try to separate reading and writing data from doing work on data. Ideally read data from a file in one function, returning some data structure. Operate on that data in one or more functions, which all return their results. Write out those results, as needed, in other functions.