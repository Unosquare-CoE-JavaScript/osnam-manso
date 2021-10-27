# Surveying JS

## Each File is a Program

- In JS, each standalone file is its own separate program.
- From the perspective of your usage of an application, it sure seems like one big program. That’s because the execution of the application allows these individual programs to cooperate and act as one program.
- Regardless of which code organization pattern (and loading mechanism) is used for a file (standalone or module), you should still think of each file as its own (mini) program, which may then cooperate with other (mini) programs to perform the functions of your overall application.

## Values

- The most fundamental unit of information in a program is a value.
- Values are data. They’re how the program maintains state. Values come in two forms in JS: **primitive and object**.
- Values are embedded in programs using literals:

  ```jsx
  greeting("My name is Kyle.");
  greeting("My name is Kyle.");
  ```

- **Strings** are ordered collections of characters, usually used to represent words and sentences.
- Another option to delimit a string literal is to use the backtick ` character. However, this choice is not merely stylistic; there’s a behavioral difference as well.

  ```jsx
  console.log(`My name is ${firstName}.`);
  ```

- Assuming this program has already defined a variable firstName with the string value "Kyle", the `\*delimited string then resolves the variable expression (indicated with ${ .. }) to its current value. This is called interpolation.

- Other than strings, JS programs often contain other primitive literal values such as **booleans and numbers**:

  ```jsx
  while (false) {
    console.log(3.141592);
  }
  ```

- In addition to strings, numbers, and booleans, two other primitive values in JS programs are **null and undefined**.
- While there are differences between them, for the most part both values serve the purpose of indicating emptiness (or absence) of a value.
- It’s safest and best to use only undefined as the single empty value, even though null seems attractive in that it’s shorter to type!

  ```jsx
    while (value != undefined) {
        console.log("Still got something!");
    }
  }
  ```

- The final primitive value to be aware of is a symbol, which is a special-purpose value that behaves as a hidden unguessable value.
- You won’t encounter direct usage of symbols very often in typical JS programs. They’re mostly used in low-level code such as in libraries and frameworks.

### Arrays And Objects

- The other value type in JS is an object value.
- As mentioned earlier, arrays are a special type of object that’s comprised of an ordered and numerically indexed list of data.
- JS arrays can hold any value type, either primitive or object (including other arrays), even functions are values that can be held in arrays or objects.
- Objects are more general: an unordered, keyed collection of any various values. In other words, you access the element by a string location name (aka “key” or “property”) rather than by its numeric position (as with arrays)

### Value Type Determination

- For distinguishing values, the typeof operator tell built-in type, if primitive, or "object" otherwise:

  ```jsx
  typeof 42; // "number"
  typeof "abc"; // "string"
  typeof true; // "boolean"
  typeof undefined; // "undefined"
  typeof null; // "object" -- oops, bug!
  typeof { a: 1 }; // "object"
  typeof [1, 2, 3]; // "object"
  typeof function hello() {}; // "function
  ```

### Declaring and Using Variables

- Values can either appear as literal values, or they can be held in variables; think of variables as just containers for values.
- Variables have to be declared (created) to be used.

```jsx
var name = "Kyle";
var age;
```

- The var keyword declares a variable to be used in that part of the program, and optionally allows initial value assignment. Another similar keyword is let:

```jsx
let name = "Kyle";
let age;
```

- The **let** keyword has some differences to var, with the most obvious being that let allows a more limited access to the variable than var. This is called “block scoping” as opposed to regular or function scoping.

```jsx
var adult = true;
if (adult) {
  var name = "Kyle";
  let age = 39;
  console.log("Shhh, this is a secret!");
}
console.log(name);
// Kyle
console.log(age);
// Error!
```

- A third declaration form is **const**. It’s like let but has an additional limitation that it must be given a value at the moment it’s declared, and cannot be re-assigned a different value later.

```jsx
const myBirthday = true;
let age = 39;
if (myBirthday) {
  age = age + 1; // OK!
  myBirthday = false; // Error!
}
```

- If you stick to using const only with primitive values, you avoid any confusion of re-assignment (not allowed) vs. mutation (allowed)! That’s the safest and best way to use const.

- Besides var / let / const, there are other syntactic forms that declare identifiers (variables) in various scopes. For example:

```jsx
function hello(name) {
  console.log(`Hello, ${name}.`);
}
```

```jsx
hello("Kyle");
```

- The identifier hello is created in the outer scope, and it’s also automatically associated so that it references the function. But the named parameter name is created only inside the function, and thus is only accessible inside that function’s scope. hello and name generally behave as var-declared.

- Another syntax that declares a variable is a catch clause:

```jsx
try {
  someError();
} catch (err) {
  console.log(err);
}
```

- The err is a block-scoped variable that exists only inside the catch clause, as if it had been declared with let.

## Functions

- In JS, we should consider “function” to take the broader meaning of another related term: “procedure.” A procedure is a collection of statements that can be invoked one or more times, may be provided some inputs, and may give back one or more outputs.

- From the early days of JS, function definition looked like:

```jsx
function awesomeFunction(coolThings) {
  // ..
  return amazingStuff;
}
```

- This is called a **function declaration** because it appears as a statement by itself, not as an expression in another statement.
- The association between the identifier awesomeFunction and the function value happens during the compile phase of the code, before that code is executed.

- In contrast to a function declaration statement, a **function expression** can be defined and assigned like this:

```jsx
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function (coolThings) {
  // ..
  return amazingStuff;
};
```

- A function expression is not associated with its identifier until that statement during runtime.

- Each parameter is assigned the argument value that you pass in that position ("Kyle", here) of the call.

## Comparisons

- In other words, we must be aware of the nuanced differences between an **equality comparison** and an **equivalence comparison**

- For NaN comparisons, use the Number.isNaN(..) utility.
- For -0 comparison, use the Object.is(..) utility, which also does not lie. Object.is(..) can also be used for non-lying NaN checks, if you prefer

## Equality and Equivalence Comparison

- "Triple-equals" | `===` | "strict equality" operator, compares the value and the type of value. Meanwhile the "double equal" | `==` | "loose equality" only compares the value, cause this second one allows type conversion..

  ```jsx
  3 === 3.0; // true
  "yes" === "yes"; // true
  null === null; // true
  false === false; // true

  42 === "42"; // false
  "hello" === "Hello"; // false
  true === 1; // false
  0 === null; // false
  "" === null; // false
  null === undefined; // false
  ```

## Coercive Comparisons

Coercion means a value of one type being converted to its respective representation in another type (to whatever extent possible).

```jsx
var x = "10";
var y = "9";

x < y; // true, watch out!
```

## How We Organize in JS

- Two major patterns for organizing code (data and behavior) are: **Classes** and **Modules.**

- Both patterns can be use at the same time, stick just with one or even neither.

### Class

A class in a program is a definition of a "type" of custom data structure that includes both data and behaviors that operate on the data.

### Class Inheritance

Inheritance is a powerful tool for organizing data/behavior in separate logical units (classes), but allowing the child class to cooperate with the parent by accessing/using its behavior and data.

- Use the extends clause to extend the general definition of a `Publication` to include additional behavior.
- The `super(..)` call in each constructor delegates to the parent `Publication` class’s constructor for its initialization work.
- The fact that both the inherited and overridden methods can have the same name and co-exist is called _polymorphism_.

```jsx
class Publication {
  constructor(title, author, pubDate) {
    this.title = title;
    this.author = author;
    this.pubDate = pubDate;
  }

  print() {
    console.log(`Title: ${this.title} By: ${this.author} ${this.pubDate}`);
  }
}

// here is the class inheritance
class Book extends Publication {
  constructor(bookDetails) {
    super(bookDetails.title, bookDetails.author, bookDetails.publishedOn);

    this.publisher = bookDetails.publisher;
    this.ISBN = bookDetails.ISBN;
  }

  print() {
    super.print();
    console.log(`Publisher: ${this.publisher} ISBN: ${this.ISBN}`);
  }
}

var YDKJS = new Book({
  title: "You Don't Know JS",
  author: "Kyle Simpson",
  publishedOn: "June 2014",
  publisher: "O'Reilly",
  ISBN: "123456-789",
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Publisher: O'Reilly
// ISBN: 123456-789
```

## Modules

Has essentially the same goal as the class pattern, which is to group data and behavior together into logical units.

### Classic Modules

_Classic modules_ are an outer function which returns an "instance" of the module with one or more functions exposed that can operate on the modules instance's internal (hidden) data.

- The class form stores methods and data on an object instance, which must be accessed with the `this.`prefix. With modules, the methods and data are accessed as identifier variable in score, without any `this.`prefix.

```jsx
function Publication(title, author, pubDate) {
  var publicAPI = {
    print() {
      console.log(`Title: ${title} By: ${author} ${pubDate}`);
    },
  };

  return publicAPI;
}

function Book(bookDetails) {
  var pub = Publication(
    bookDetails.title,
    bookDetails.author,
    bookDetails.publishedOn
  );
  var publicAPI = {
    print() {
      pub.print();
      console.log(
        `Publisher: ${bookDetails.publisher} ISBN: ${bookDetails.ISBN}`
      );
    },
  };

  return publicAPI;
}

var YDKJS = Book({
  title: "You Don't Know JS",
  author: "Kyle Simpson",
  publishedOn: "June 2014",
  publisher: "O'Reilly",
  ISBN: "123456-789",
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Publisher: O'Reilly
// ISBN: 123456-789
```

### ES Modules- Introduced to the JS language in ES6

- There's no wrapping function to define a module. The wrapping context is a file.
- You don’t interact with a module’s “API” explicitly, but rather use the `export` keyword to add a variable or method to its public API definition.
- You don’t “instantiate” an ES module, you just import it to use its single instance.
- If your module needs to support multiple instantiations, you have to provide a classic module-style factory function on your ESM definition for that purpose.

```jsx
function printDetails(title, author, pubDate) {
  console.log(`Title: ${title} By: ${author} ${pubDate}`);
}

export function create(title, author, pubDate) {
  var publicAPI = {
    print() {
      printDetails(title, author, pubDate);
    },
  };

  return publicAPI;
}
```

```jsx
import { create as createPub } from "publication.js";

function printDetails(pub, URL) {
  pub.print();
  console.log(URL);
}

export function create(title, author, pubDate, URL) {
  var pub = createPub(title, author, pubDate);

  var publicAPI = {
    print() {
      printDetails(pub, URL);
    },
  };

  return publicAPI;
}
```

```jsx
import { create as newBlogPost } from "blogpost.js";

var forAgainstLet = newBlogPost(
  "For and against let",
  "Kyle Simpson",
  "October 27, 2014",
  "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

---
