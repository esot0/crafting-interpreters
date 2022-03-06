### **Scanning**

Also known as lexing, or lexical analysis. Scanning is most common.
Takes in a linear stream of characters and chunks them into words for our program to work with. Each of these “words” is called a token. Each token has a particular programmatical meaning.

###

### Parsing

Takes flat sequence of tokens, builds tree structure that mirrors nested structure of grammar
• May be known as parse trees or abstract syntax trees. Usually called syntax trees, ASTs, or just trees
• Parser’s job includes letting us know when we have a syntax error
**Static Analysis**
Both parsing and scanning are quite similar across all implementations. From here on, languages differ quite a bit. At this point, we know the code’s syntax well (what expressions are nested in what), by not much else.
**Binding/resolution**
**F**or a given identifier, we find where the name is defined/declared. and then wire the two together. This is how we make sure a variable knows where it’s value is, and what its value is, when we call it.

This is also where scope comes into play—the region of source code where a certain name can be used to refer to a certain declaration.

a + b

If the language is statically typed, we type check at this point. Once we know where a and b are declared, we can figure out their types. If those types don’t support being added, we report a type error

But where does data from analysis get stored?
• Sometimes, it gets stored as attributes on the syntax tree. They’re extra fields in the nodes that aren’t initialized during parsing, but then get filled in later

• Sometimes, we store data in a lookup table off to the side. Typically, keys to this table are identifiers, names of variables and declarations. We call this a symbol table, and the values it associates with each key tell us what that identifier refers to

• The best thing we can do is to transform the tree into an entirely new data structure which more directly expresses code semantics
**Front Ends/Back Ends**
Everything up to this point is the “front end” of the implementation. However, not everything else is the “back end

While the front end of a language is tied to the language it was written in, and the backend of a language is tied to the final architecture the program will run on, we can
have a "middle end," tied to neither the source or destination.

In the middle end, code should be stored in an **intermediate representation**, aka, not in the target or source language. C is a really popular intermediate representation, even if
it wasn't designed to be one, because of its ubiquity in UNIX systems and its nature as an abstraction of assembly.

Without a middle end, you'd have to write a full compiler for every single source language and target architecture you have.

With an IR, you only have to write one front end for every source language you use, and one back end for every architecture. Very useful.

[Wikipedia Page on IR](https://en.wikipedia.org/wiki/Intermediate_representation)

<br>
### Optimization
<br>
Once we understand a user's program, we can swap it out with another program that does the same thing--but quicker.

An example is constant folding: imagine we have an expression

`a = (4*7) + 2/4 - 4;`

And we know that a never changes throughout the program. Since the expression only has to be evaluated once, we can do arithmetic in the compiler and set a to its numerical value.

However, many successful langs produce relatvely unoptimized code during compilation, and focus their optimization efforts on runtime.
<br>
### Virtual Machines
<br>
<br>
### JIT (Just in Time) Compiling

Some languages opt to compile programs
<br>
### Compilers vs Interpreters
<br>
**Compilers -** Usually parse a source language into a lower level language that's easier for computers to understand. The user then runs the program themselves.

<br>
**Interpreters -** Usual