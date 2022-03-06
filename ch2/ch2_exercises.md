# **CHALLENGES**

1. I decided to check out Python's source code. Poking through it, I'm decently sure I found the scanner (assuming it's tokenizer) and parser files.
I couldn't spot any .yak or .l files so it looks like it was written by hand.
2. JIT compilation seems to be particularly useful when we expect our code to run on a wide variety of platforms--like Java. But it also adds another layer of
abstraction to our compiler--a program only has to run JIT compilation once, but this obviously presents a performance cost. So, if a program has to run a ton of times,
JIT compilation makes sense. But if it's short-lived, then JIT isn't optimal. Furthermore, JIT can hog resources from other user processes.
3. Could it be the runtime? Even if the code is transpiled, maybe LISP has to take care of garbage collection or finding the types of objects.