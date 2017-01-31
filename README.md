earl
====

EARL, efficient algorithm representation language

What EARL is, and isn't -------------------



EARL IS

  EARL, in this repository, is a language for the efficient representations of algorithms on the ARDUINO platform.

  EARL shares many ideas from FORTH (an ancient language with a rich history created by brilliant people)

  EARL executes as an interpreter on Arduino (or similar) but (unlike FORTH) the development system, or "outer  interpreter" resides on a PC, or such, which serves to prepare code for execution in the Arduino environment.

  EARL is designed to enable an extremely compact, yet efficient, representation of the algorithmic portions of an application.  It does little to compress text and media.

  EARL is designed (or coded) in such a way that adaptation to other purposes is quite obvious.   Thus the version in the master branch of this repository is quite simple and devoid of many features or platform specific capabilities.
  
  
  

EARL IS NOT

  EARL is not intended to compete with, replace, bad mouth, or otherwise interact with FORTH.
  
  EARL is not mature, complete, blah, blah, and certainly not guaranteed to be good for anything.
  
  
  
  
ATTRIBUTES of EARL

  EARL (the execution part of it) maintains a return stack so that "verbs" can be invoked and then control returns to the next "verb".
  
  EARL has some "built-in" verbs (like + or dup).
  
  EARL is extended by the user who writes "user verbs" which string together built-in verbs and other user verbs.  The user verbs are "compiled" in a PC development environment and eventually transmitted as part of the complete execution package for the target (Arduino).   The PC development environment may include a simulator of the target environment.
  
  EARL, in this repository, handles 32 bit integers, 32 bit floats, and pointers.  Information is mostly passed by an "operand stack".  The operand stack keeps track of the type and the built-in verbs try to cover the various possibilities at some cost in speed but great benefit in code compactness.  (Like many attributes, this may be modified by easy variants of the code.)
  
  EARL allows the declaration and use of variables and arrays of variables.
  
  EARL for Arduino expects a user verb named "setup" and another named "loop" (like the Arduino SKETCH developent environment).   When the return stack is empty control returns to "setup" if it is present or "loop" otherwise.
  
  
  
SYNTAX (on a PC or other development platform)

EARL programs are expressed as "user verbs" which are defined in a program file (or typed in) and result in a package that is loaded onto an Arduino device where it executes.  The definition of a user verb begins with the "built-in" verb "means" and it ends with a period.  The "means" verb expects a pointer to a string on the top of the stack (and here it is meant the top of the stack during the operation of a simulator in the PC environment).   Here is a line by line example:
  (Parentheses mean commentary material.  White space separates verb names.)
  "triple"    (create a string "triple" and place a pointer to it on the top of the operand stack)
  means       (execute the built-in verb "means" which starts the definition of a user verb whose name is on the stack)
  dup dup     (duplicate the top of stack twice)
  + +         (add the two top stack elements, two times)
  %           (print the answer, and drop it from the stack)
  .           (this is the end of the definition)
If one then typed at a prompt the line:
2 triple
there would appear a response:
6

Mentioning a number puts its value on the top of stack.

Mentioning a string (with quotes) creates the string and leaves a pointer to it on the top of stack.

Constants may be defined as user verbs.   For example: "pi" means 3.1 .

Variables are defined with the built-in verb "varies.".  For example:  "age" varies.
The value of a variable is set with !    For example:  21 age ! (stores 21 into variable age)
The value of a variable is fetched with @   For example:  age @ (puts the value stored in age on the operand stack)

Arrays reuse the "varies." syntax but with intervening limits and dimensionality. For example:
"altitude" 50 60 2 varies.
would define a 2 dimensional array which is 50 by 60 named "altitude".   For example to fetch and print the [5, 7] element one could type:
5 7 age @ %
Element indexing is zero based.

The type (long integer, float, pointer) of a variable is set each time a value or an element value is stored into it.  (Be careful!)

The built-in verb "library" expects to find the pointer to a string on to stack.   That string is taken as the name of a file which is added to a list of library files.  They are processed repeatedly each time a user verb is defined or a source file is processed.  User verbs from a library file are compiled only if they are referenced by existing user verbs.

Lexical items are processed according to the first match in:
    a comment  (comments can be nested but must not span file boundries)
    a number
    a user verb
    a file name
    a built-in verb
    



  
  
