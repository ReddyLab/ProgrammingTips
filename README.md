# Programming Tips

Please note that these tips are meant to be guidelines, not laws. Additionally, they reflect the author's own biases. Suggestions and discussion are/is encouraged.

## High Level

Much of the programming  we do falls into the realm of "batch processing" -- these are programs that are run with a set of data and result in a set of other data. Mathematically they are essentially functions. Often the programs can be broken down into sub-programs which may be further broken down, etc. We can think of the structure like a Directed-Acyclic Graph (DAG) with each vertex being a sub- (or sub-sub- etc.) program and each edge indicating a flow of data out of one sub-program into another.

This conception of our programs as DAGs can help guide us to write cleaner, more testable code.
Pure functions

In computer programming a "Pure Function" is a function that takes a set of arguments and returns one or more values. It doesn't have any "side effects"[0] and always returns the same outputs for a given set of inputs. Much like the program as a whole, these are mathematical functions. If a (programming) function relies on a side effect to work correctly (directly or indirectly) it is "impure".

Of course your whole program can't be pure because a program that doesn't accept any input or perform any output isn't very useful. So it's a good idea to isolate these impure parts from the pure ones.

Pure functions have several benefits.
- Easy to test
- easy(-ier) to reason about

There are a several way so isolate or factor out the impure portions of functions. Here a few (not an exhaustive list):
- read the input values from a file, pass the values to the pure function, then write the function output to another file
- [Python only] IO objects, like files, are "iterable" -- they can be iterated over in a `for` loop. So pass the file object to the pure function and just use the "iterable" functionality. When testing create a test object that is also iterable. The test object can be passed in to the pure function just like the file. This is a specific case of:
- Wrap the impure thing in an "interface"[2] that can also be implemented by pure things and pass the wrapper in to the function. From within the pure function only use the interface for interacting with the passed-in object.

[0]  A side effect is a change to the environment outside the function itself. Common examples are print to the console[1], file input/out, and setting global variables.<br/>
[1] Printing to the console is generally harmless<br/>
[2] An interface is a set of methods an object<br/>

## Testing

As mentioned, pure functions make testing easier. This assumes you are testing! As your program grows in size testing becomes more and more important. Many language have testing frameworks, some even have them built in. Python has a unittest module that can be used to write tests, but the third-party framework pytest is very popular and pretty easy to use.

If you use a random number generator, make sure you have a way to seed it so program runs are repeatable. That way you can compare run-to-run outputs exactly instead of guessing that they are close enough and probably nothing went wrong.

## Source Control

Source control is very important so you don't lose work. Additionally, if your programming is "exploratory" it makes it easy to backtrack if you go down a dead-end. The Reddy lab (and many other labs, probably) use GitHub, and thus [`git`](https://git-scm.com), for source control.

You probably want one `git` repository per project (there is an alternative to this called a "mono repo" that puts all projects in a single repository, but that's probably not a good fit).

While programming it's a good idea to commit your changes to `git` somewhat often, at least once a day. Good practice is for each commit to be a set of changes for a single purpose. This purpose could be adding a new feature, removing some code, or cleaning up formatting.

When working on your own you can use whatever source control workflow you like, but once you start collaborating on software with other people you'll want to come up with a way of collaborating together. [GitHub](https://github.com) has [a guide](https://guides.github.com/introduction/flow/) explaining the "GitHub Flow" for collaboration.

## Environment management

virtualenv or conda

## Version Pinning

For reproducibility reasons it's important that the version of any software you use is documented. However, not only do the software and libraries we use directly have versions, but they have their own dependencies, which have their own dependencies, etc. Fortunately many dependency managers, like pip or conda, have a way of outputting all the libraries they are using.

- pip freeze will output installed packages, along with their version numbers, in a way formatted for use with pip.  This works best in conjunction with an environment manager.
- conda list will output all installed packages installed using conda.

## Naming variables

There's a joke in the programming community that "There are only two hard problems in computer science: cache invalidation and naming things." Generally, it's a good idea to name things by what they __do__ i.e., their purpose, not by what they are.

That said, sometimes non-descriptive names are okay. Loop counters, for instance, are often named `i` or `j`. This is a known convention and is fine.

## Python Tips

https://death.andgravity.com/aosa has some interesting resources for learning about how to structure larger programs.

Don't worry about supporting Python 2, unless you have a very good reason. It's no longer supported by the Python Software Foundation and library authors have had over a decade to transition to python 3.
Python release and support schedules:<br/>
[Python 2](https://www.python.org/dev/peps/pep-0373/)<br/>
[Python 3](https://www.python.org/dev/peps/pep-0602/)

Use [`os.path.join`](https://docs.python.org/3/library/os.path.html#os.path.join) or [`pathlib`](https://docs.python.org/3/library/pathlib.html) to join directory and file names (instead of concatenating strings).

When creating a string, use [f-Strings](https://realpython.com/python-f-strings/).

[icecream](https://github.com/gruns/icecream) looks like a helpful improvement for debug printfs

## Misc

Automate early and often

Try to separate reading and writing data from doing work on data. Ideally read data from a file in one function, returning some data structure. Operate on that data in one or more functions, which all return their results. Write out those results, as needed, in other functions.

[UP](https://github.com/akavel/up) looks like a helpful utility for building those `awk`/`sed`/`tr`/`grep` pipelines
