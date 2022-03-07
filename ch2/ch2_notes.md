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
###Code Generation
After applying optimization, we have to convert the code into a form that the machine can work with. This is dubbed "code gen," although this code doesn't generally resemble the high level languages we know and love. Instead, this "code" is primitive and assembly-like, better suited for a CPU.

At this point we have reached the back end. Earlier, we were figuring out the semantics of our code. Now, we're trying to get it more and more basic, until it's in a form that the machine can read. 

Languages make a choice at this point--do they generate code for a physical, real CPU? Or do they generate code for a virtual one? 

**Generating for a Real CPU: **
* Get an executable that the OS can load directly on the chip
* Generally faster than generating code for a virtual CPU 
* Code generation is very difficult and time consuming
* Compiler is tied to a specific architecture--a compiler that targets x86 machine code will not run on ARM devices.

**Generating for a Virtual CPU:**
* Virtual machine code formed in response to the aforementioned difficulties in generating machine code for a real CPU
* Produce code for a hypothetical, idealized machine. This code is generally called **bytecode** today because each instruction tends to be a single byte long.
* These instructions map a little closer to the language's semantics and are not as tied to the architecture of any specific computer
* Dense, binary encoding of language's low-level operations


<br>
### Virtual Machines

Assuming that your compiler produces bytecode (you're going the virtual CPU route) you still have some work to do. 

There is no chip that understands bytecode--it's an intermediate abstraction between the CPU and your source code. Therefore, you have to translate the bytecode into something the computer can read.

**The farther down the pipeline you push architecture-specific work, the more earlier phases you can share between architectures**

However, there is a tradeoff- a lot of optimizations are at their most efficient when you're using the features of a specific chip to the fullest. 

One should seek a fine balance between facets of a compiler that target specific architectures, and facets that are shared between architectures.

You can write a **virtual machine**, a program which emulates a hypothetical chip that supports your virtual architecture at runtime. However, it's slower than translating it to native code, [since every instruction must be simulated at runtime every time it exectures] (what exactly does this mean--note, do more research on virtual machines and how they function)

However, you get a tradeoff of portability; implement your VM in C and run your language on any platform with a C compiler. 

This is not to be confused with a system virtual machine, which may let you run windows on your Linux laptop. We discuss a language virtual machine/process virtual machine. 

<br>
<br>


### Runtime
After compilation is over, we have the program in a form that we can execute. 

* If compiled to machine code, the OS can simply load the executabl
* If we took the bytecode approach, we need to start up the language's VM and load the program into that

In the vast majority of languages, our language still needs to provide some services while a program runs--for example, if it's a language like Java, it needs to run its garbage collector so as to reclaim unused memory. Or, if our language has "instance of" test, the language should keep track of each object's type during execution.

Since this all goes on at runtime, all the processes are aptly called the "runtime." In a fully compiled language, the code that implements the runtime gets injected directly in the executable. 

If the language runs inside an interpreter or VM, then the runtime lives there (Java, Python, JS). 


### Single Pass Compiler

Some compilers interleave parsing, analysis, and code gen without ever touching intermediate representations. Since you have no intermediate data structures to store the program's global info, you never revisit any previously parsed part of code; your code is sequential in the truest, strictest sense. 

This means that as soon as you see an expression, you must have all the information necessary to parse it.

Both Pascal and C are built on this because of intensive memory constraints at the time of their release. 

**syntax-directed translation** - A technique for building single pass compilers. (tbc)

### Tree-walk Interpreters

Some programming languages begin executing code right after parsing it to an AST. 

To run the program, the interpreter traverses the syntax tree one branch and lead at a time, progressively evaluating each node as it goes.

This style is most common for student projects and little languages, but it's very slow so it's not really used in general-purpose languages. Some use "interpreter" only to refer to this implementation. ![8273B839-34D5-4A5B-9F8F-CC85B041548C](https://user-images.githubusercontent.com/52178869/157052490-445e20ed-053d-44eb-8ff6-d30fa4a93db9.jpeg)


### Transpilers

Writing a complete backend for a language can be very time consuming. However, if you have an existing intermediate representation to target, you can connect your frontend to that instead. Otherwise, you're a bit stuck.

However, there's the approach of treating some other source language as if it were an intermediate representation

Write a frontend for your language. Then, in the backend, you're not "going down the mountain" and lowering your code to machine code, you produce a string of valid source code for another language that's about as high level as yours. Then, use existing compilation tools for _that_ language to reach the machine code level.

* Many languages target Javascript instead of machine code to get your code to run in the browser in this way. 

The first transcompiler translated one assembly language to another. Today, most transpiler work on higher level language.

After the viral spread of UNIX, there began a long tradition of compilers that produced C as their output language. C compielrs were everywhere Unix was, and produced efficient code, so targetting C was a great way of getting your language running on a vast range of architectures.


This may be known as a source-to-source compiler or transcompiler. Today, the more common term is transpiler. 

The front end of a transpiler looks like traditional compilers. THen, if the source language is just a syntactically different than the target language, one might skip analysis and go directly to outputting analogous syntax in a destination language.

If two languages are more semantically different, you may see more typical compiler features, like analysis and optimization.

Instead of outputting machine code, you produce grammatically correct code in the target language. Just run that resulting code through the output language's existing compilation pipeline, and you're set. 


### JIT (Just in Time) Compiling

The fastest way to execute code is compiling it to machine code. However, you might not know the architecture your end user machine supports. 

The JVM opts for the approach of compiling bytecode to native code _when the program is loaded on the user machine_ , for the user's given architecture. Naturally, this is called JIT (Just in Time) Compiling, pronounced like fit.

### Compilers vs Interpreters

What's the difference between compilers and interpreters? 

This question is analagous to asking if something is a fruit or a vegetable--while it seems like an "either or" choice, "fruit" is a botanical term and "vegetable" is culinary. Something can be both a fruit or a vegetable, and something can be both a compiler and interpreter. 


**Compilers** -An implementation technique. Usually parse a source language into a lower level language that's easier for computers to understand, such as machine code or bytecode (transpilers also fall under the compiler umbrella). A compiler just translates source code into some other form, but doesn't execute. A user has to execute the output themselves

Example: 
* GCC + clang -- take C code and compile it to machine code. An end user runs that executable directly and never has to worry about what tool was used for compilation. 
*
<br>
**Interpreters** - takes in source code and executes it immediately--runs a program "from source." 

Example:
* In older versions of Ruby, the implementation parsed the source code and executed it directly by traversing the syntax tree


Addendum: What about CPython?
* When you run your Python program w CPython, the code is parsed + converted to an internal bytecode format. Then, this is executed inside the VM. While the user never sees anything and it _seems_ python is running from source, there is a bit of both going on. 
