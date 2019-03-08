# Apex Style Guide #

<!-- MarkdownTOC depth=0 autolink=true autoanchor=true bracket=round -->

- [Intro](#intro)
  - [Goals](#goals)
  - [Sources](#sources)
- [Basics](#basics)
  - [Special characters](#special-characters)
    - [Whitespace](#whitespace)
    - [Special escape sequences](#special-escape-sequences)
    - [Other Non-ASCII Characters](#other-non-ascii-characters)
- [Structure](#structure)
  - [Indentation](#indentation)
  - [New-lines and spaces](#new-lines-and-spaces)
  - [Prefer Explicit Declarations](#prefer-explicit-declarations)
  - [`@isTest`](#istest)
  - [Capitalization](#capitalization)
  - [Example](#example)
- [SOQL](#soql)
- [Apex-Specific SObject Constructor Syntax](#apex-specific-sobject-constructor-syntax)
- [Test.startTest() and Test.stopTest()](#teststarttest-and-teststoptest)
- [Naming Conventions](#naming-conventions)
  - [Class and Trigger](#class-and-trigger)
  - [Methods](#methods)
  - [Test classes](#test-classes)

<!-- /MarkdownTOC -->

<a name="intro"></a>
## Intro

<a name="goals"></a>
### Goals
The goal of this style guide is like that of any other style guide.  Everyone has their own ideas of what makes code pretty.  As long as there's some logic behind that beauty, no one is right or wrong.  But it's important to have a standard so that:

1. New and old developers, and outside contractors of all sorts can easily read, understand and maintain our growing code base.
2. Code merges are easy to handle and are content-ful, not style-ful.

See the Internet for more arguments about why style guides are important and useful things and not just a waste of time.

<a name="sources"></a>
### Sources
Since Apex is largely a Java and C# spin-off, I am largely relying on [Google Java Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html) first, then C# where those do not apply.  And some is just pulled out of my own opinions.

This is still a living document and I'm happy to make changes.

<a name="basics"></a>
## Basics
<a name="special-characters"></a>
### Special characters
<a name="whitespace"></a>
#### Whitespace
The only permissible whitespace characters in source code are newline and space (0x20).  Inside of a string literal, only a space is allowed.  Line must not end with spaces (`/ +$/` must not match anything in the file).  Classes should all end with a newline.

<a name="special-escape-sequences"></a>
#### Special escape sequences
For any character that has a special escape sequence (`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'` and `\\`), that sequence is used rather than the corresponding octal (e.g. `\012`) or Unicode (e.g. `\u000a`) escape.

<a name="other-non-ascii-characters"></a>
#### Other Non-ASCII Characters
For the remaining non-ASCII characters, either the actual Unicode character (e.g. `∞`) or the equivalent Unicode escape (e.g. `\u221e`) is used, depending only on which makes the code easier to read and understand.

  > Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

<a name="structure"></a>
## Structure

The ordering of the members of a class can have a great effect on learnability, but there is no single correct recipe for how to do it. Different classes may order their members differently.

What is important is that each class order its members in some logical order, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.

<a name="indentation"></a>
### Indentation
All blocks of code should be indented with 2 spaces.  Spaces, not tabs, to ensure that it looks the same on everyone's screen and doesn't waste horizontal space.

<a name="new-lines-and-spaces"></a>
### New-lines and spaces
Use vertical whitespace as appropriate.  Don't be afraid to separate blocks of code.

Prefer placing comments on a line by themselves.

Open braces should have a space before them and not a newline.  The matching close brace should line up with the start of the opening brace's line.

`else`s and `else if`s do not get a new-line before them.  Neither do `catch`es or `while`s in a `do...while` loop.

The parenthetical clause in `if`, `while`, `do`, `catch`, etc., statements should be preceded and followed by a single space.

In method definitions, there should be no space before the open parenthesis, and one space after.

In method calls and definitions, there should not be whitespace between the name of the method and the open parenthesis.

A single space should separate binary operators from the surrounding elements (e.g., `+`, `||`, `=`, `>=`).  Unary operators (`!`, `-`) should be attached to their parameters.  A colon inside a `for each` loop (e.g., `for (Contact cnt : contacts) {`) should have one space on either side.  There should be no whitespace before commas, and one space after (e.g., `System.debug(LoggingLevel.INFO, 'fsdfs');`).

If using C#-style properties, code should follow the following rules:

 * Always declare the getter, then the setter.
 * If there is no logic, it should read `{ get; set; }`.
 * If there is logic, there should be a new-line before each open-brace, and before and after each closed-brace.
 * If one clause has logic and one does not, place the clause without logic on its own line.

<a name="prefer-explicit-declarations"></a>
### Prefer Explicit Declarations
Always specify:

* `global`/`public`/`private` modifiers - prefer `private`, and if possible, `static`
* `with sharing`/`without sharing`
* `this` when calling local methods or setting local members/properties.

<a name="istest"></a>
### `@isTest`
In a test method, use the `@isTest` attribute instead of the `testmethod` modifier.

<a name="capitalization"></a>
### Capitalization

We follow the Java standard of capitalization with the listed exceptions.  That means that statements (`for`, `if`, etc.) should be lowercase, constants should be `UPPER_CASE_WITH_UNDERSCORES`, classes and class-level variables should be declared as `UpperCamelCase`, and methods, parameters and local variables should all be declard as `lowerCamelCase`.

Native Apex methods and classes should generally be referenced as written in official Salesforce documentation.  This means that schemas and classes are `UpperCamelCase` and methods are `lowerCamelCase`.  The only deviation from this rule is `SObject` which should be written as such (in the documentation, it is usually written `sObject` which does not conform to this style guide and should not be used).

However, when referencing any metadata (SObject, SObjectField, FieldSet, Action, Class, Page, etc.), use the declared capitalization.  Even when referencing a method, field, etc., that is not capitalized according to these rules, still use the declared capitalization.

<a name="example"></a>
### Example

```java
public class MyClass {

  private Contact internallyUsedContact { get; set; }

  public Integer calculatedInteger {
    get {
      return 5;
    }
    set {
      this.internallyUsedContact = [SELECT Id
                                    FROM Contact
                                    WHERE Number_of_Peanuts__c > :value
                                    LIMIT 1];
    }
  }

  private Id contactId {
    get;
    set {
      System.debug('Why do this?');
      this.contactId = value;
    }
  }

  public void foo(Integer bar) {
    if (bar == 3) {
      // Diane often asks when bar is 3.
      System.debug(this.debugCode(bar) + ' - hi there!');
      return;
    } else if (bar > 7) {
      List<Integer> wasteOfSpace = new List<Integer>();
      do {
        wasteOfSpace.add(this.calculatedInteger);
      } while (wasteOfSpace.size() < 5);
    } else {
      try {
        upsert v;
      } catch (Exception ex) {
        handleException(ex);
      }
    }

    for (Integer i : wasteOfSpace) {
      System.debug('Here\'s an integer! ' + i);
    }
  }

}
```


<a name="soql"></a>
## SOQL

In general, SOQL should be declared inline where it is used.  In some cases, like when referencing FieldSets, it's necessary to build SOQL queries dynamically.  The same rules will generally apply.

SOQL keywords (e.g., `SELECT`, `WHERE`, `TODAY`) should always be written in `ALL CAPS`.  Objects, fields and bind variables should be referenced as declared.  Each clause of the SOQL Query should be on its own line so that finding what changed in a diff is easier.  That is, each `SELECT`, `FROM`, `WHERE`, `AND`, `OR`, `GROUP BY`, `HAVING`, `ROLL UP`, `ORDER BY`, etc., with the exception of the first `SELECT` should start a new line.  That line should start in the same column as the most relevant `SELECT`.

Long lists of fields in a `SELECT` clause should be ordered in a logical manner and broken to fit within page width, with subsequent lines aligned with the first field.  Always select `Id`, and always select it first.

Example (in context):

```java
String typeToSelect = 'abcde';
List<Contact> cnts = [SELECT Id, FirstName, LastName, Phone, Email,
                             MailingCity, MailingState,
                             (SELECT Id, ActivityDate, Origin, Type,
                                     WhatId, What.Name, RecordTypeId
                              FROM ActivityHistories
                              WHERE Type = :typeToSelect)
                      FROM Contact
                      WHERE CreatedDate >= TODAY];
```

<a name="apex-specific-sobject-constructor-syntax"></a>
## Apex-Specific SObject Constructor Syntax
When creating an SObject, generally prefer the Apex-specific syntax wherein all fields can be initialized from the constructor.  When using this syntax, choose a different line for each property so that diff-ing and versioning is easier.

Example:

```java
Contact c = new Contact(RecordTypeId = CONTACT_RECORDTYPE_ID,
                        FirstName = firstName,
                        LastName = surname,
                        MailingCountry = DEFAULT_COUNTRY
                       );
```

<a name="teststarttest-and-teststoptest"></a>
## Test.startTest() and Test.stopTest()
When writing test cases, always use `Test.startTest();` and `Test.stopTest();`.  Always indent the code between those method calls.

<a name="naming-conventions"></a>
## Naming Conventions

<a name="class-and-trigger"></a>
### Class and Trigger
Name a class or trigger after what it does.  Triggers should be verbs and end with `Trigger` (e.g., `SyncCaseToplineWithDescriptionTrigger`).  Controllers and Controller Extensions should end with the word `Controller`.

<a name="methods"></a>
### Methods
Methods should all be verbs.  Getters and setters should have no side effects (with the exception of setting up cached values and/or logging), and should begin with `get` or `set`.

<a name="test-classes"></a>
### Test classes
Test classes should be named `TEST_ClassUnderTest`.  If the test is not a unit-level test but instead a broader test case, it it should be named `TEST_StuffThatsGenerallyBeingTested`.


> Written with [StackEdit](https://stackedit.io/).
