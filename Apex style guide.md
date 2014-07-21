# Apex Style Guide #
[TOC]
##Intro##

###Goals###
The goal of this style guide is like that of any other style guide.  Everyone has their own ideas of what makes code pretty.  As long as there's some logic behind that beauty, no one is right or wrong.  But it's important to have a standard so that:

1. New and old developers, and outside contractors of all sorts can easily read, understand and maintain our growing code base.
2. Code merges are easy to handle and are content-ful, not style-ful.

See the Internet for more arguments about why style guides are important and useful things and not just a waste of time.

###Sources###
Since Apex is largely a Java and C# spin-off, I am largely relying on [Google Java Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html) first, then C# where those do not apply.  And some is just pulled out of my own opinions.

This is still a living document and I'm happy to make changes.

##Basics##
###Special characters###
####Whitespace####
The only permissible whitespace characters in source code are newline and space (0x20).  Inside of a string literal, only a space is allowed.  Line must not end with spaces (`/ +$/` must not match anything in the file).  Classes should all end with a newline.

####Special escape sequences####
For any character that has a special escape sequence (`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'` and `\\`), that sequence is used rather than the corresponding octal (e.g. `\012`) or Unicode (e.g. `\u000a`) escape.

####Other Non-ASCII Characters####
For the remaining non-ASCII characters, either the actual Unicode character (e.g. `∞`) or the equivalent Unicode escape (e.g. `\u221e`) is used, depending only on which makes the code easier to read and understand.

  > Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

##Structure##

The ordering of the members of a class can have a great effect on learnability, but there is no single correct recipe for how to do it. Different classes may order their members differently.

What is important is that each class order its members in some logical order, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.


###Indentation###
All blocks of code should be indented with 2 spaces.  Spaces, not tabs, to ensure that it looks the same on everyone's screen and doesn't waste horizontal space.

Open braces should have a space before them and not a newline.  The matching close brace should line up with the start of the opening brace's line.

`else`s and `else if`s do not get a newline before them.  Neither do `catch`es or `while`s in a `do...while` loop.

E.g.:
```
public void foo(Integer bar) {
  if(bar == 3) {
    return;
  } else if(bar > 7) {
    List<Integer> wasteOfSpace = new List<Integer>();
    do {
        wasteOfSpace.add(8);
    } while(wasteOfSpace.size < 5);
  } else {
    upsert v;
  }
}
```

All `if`, `while` and `for` statements must use braces even if they control just one statement.






> Written with [StackEdit](https://stackedit.io/).
