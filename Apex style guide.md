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

The parenthetical clause in `if`, `while`, `do`, `catch`, etc., statements should be preceded and followed by a single space.

In method definitions, there should be no space before the open parenthesis, and one space after.

In method calls and definitions, there should not be whitespace between the name of the method and the open parenthesis.

A single space should separate binary operators from the surrounding elements (e.g., `+`, `||`).  Unary operators (`!`, `-`) should be attached to their parameters.

E.g.:
```
public void foo(Integer bar) {
  if (bar == 3) {
    System.debug(debugCode(bar) + ' - hi there!');
    return;
  } else if (bar > 7) {
    List<Integer> wasteOfSpace = new List<Integer>();
    do {
        wasteOfSpace.add(8);
    } while (wasteOfSpace.size < 5);
  } else {
    try {
      upsert v;
    } catch (Exception ex) {
      handleException(ex);
    }
  }
}
```

### Capitalization ###

We follow the Java standard of capitalization with the listed exceptions.  That means that statements (`for`, `if`, etc.) should be lowercase, constants should be `UPPER_CASE_WITH_UNDERSCORES`, classes and class-level variables should be declared as `UpperCamelCase`, and methods, parameters and local variables should all be declard as `lowerCamelCase`.

Native Apex methods and classes should generally be referenced as written in official Salesforce documentation.  This means that schemas and classes are `UpperCamelCase` and methods are `lowerCamelCase`.  The only deviation from this rule is `SObject` which should be written as such (in the documentation, it is usually written `sObject` which does not conform to this style guide and should not be used).

However, when referencing any metadata (SObject, SObjectField, FieldSet, Action, Class, Page, etc.), use the declared capitalization.  When referencing a method, field, etc., that is not capitalized according to these rules, use the declared capitalization.

##SOQL##

In general, SOQL should be declared inline where it is used.  In some cases, like when referencing FieldSets, it's necessary to build SOQL queries dynamically.  The same rules will generally apply.

SOQL keywords (e.g., `SELECT`, `WHERE`, `TODAY`) should always be written in `ALL CAPS`.  Objects, fields and bind variables should be referenced as declared.  Each clause of the SOQL Query should be on its own line so that finding what changed in a diff is easier.  That is, each `SELECT`, `FROM`, `WHERE`, `AND`, `OR`, `GROUP BY`, `HAVING`, `ROLL UP`, `ORDER BY`, etc., with the exception of the first `SELECT` should start a new line.

Long lists of fields in a `SELECT` clause should be ordered in a logical manner and broken to fit within page width, with subsequent lines aligned with the first field.  Always select `Id` first.

Example (in context):

```
String typeToSelect = 'abcde';
List<Contact> cnts = [SELECT Id, FirstName, LastName, Phone, Email,
                             MailingCity, MailingState,
                             (SELECT Id, ActivityDate, Origin, Type,
                                     WhatId, What.Name, RecordTypeId
                              FROM ActivityHistories
                              WHERE Type = :typeToSelect)
                      FROM Contact
                      WHERE CreatedDate = TODAY];
```






> Written with [StackEdit](https://stackedit.io/).
