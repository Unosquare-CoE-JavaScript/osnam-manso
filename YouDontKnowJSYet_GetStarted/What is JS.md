# What is JavaScript

## What’s with the name of JavaScript

- The name JavaScript is probably the most mistaken and misunderstood programming language name.
- The truth is, the name JavaScript is an artifact of marketing shenanigans. When Brendan Eich first conceived of the language, he code-named it Mocha.
- Internally at Netscape, the brand LiveScript was used. But when it came time to publicly name the language, “JavaScript” won the vote.
- Because this language was originally designed to appeal to an audience of mostly Java programmers, and because the word “script” was popular at the time to refer to lightweight programs.
- These lightweight “scripts” would be the first ones to embed inside of pages on the web.
- Oracle (via Sun), is the owner of the official trademark for the name “JavaScript” (via Netscape). For these reasons, some have suggested we use **JS** instead of JavaScript.
- The official name of the language specified by **TC39** and formalized by the ECMA standards body is **ECMAScript**.
- In other words, the JavaScript/JS that runs in your browser or in Node.js, is an implementation of the ES2019 standard.

## Language Specification

- The TC39 committee is comprised of between 50 and about 100 different people from a broad section of web-invested companies, such as browser makers (Mozilla, Google, Apple) and device makers (Samsung, etc). All members of the committee are volunteers, though many of them are employees of these companies and so may receive compensation in part for their duties on the committee.
- JavaScript is most definitely a multi-paradigm language. You can write procedural, class-oriented, or FP-style code, and you can make those decisions on a line-by-line basis instead of being forced into an all-or-nothing choice.

## Backwards & Forwards

- One of the most foundational principles that guides JavaScript is preservation of backwards compatibility.
- Backwards compatibility means that once something is accepted as valid JS, there will not be a future change to the language that causes that code to become invalid JS.
- The idea is that JS developers can write code with confidence that their code won’t stop working unpredictably because a browser update is released.
- Being forwards-compatible means that including a new addition to the language in a program would not cause that program to break if it were run in an older JS engine.
- **JS is not forwards-compatible**.\*\*

  - If the feature is a new syntax, the program will in general completely fail to compile and run, usually throwing a syntax error. If the feature is an API (such as ES2016 Object.is), the program may run up to a point but then throw a runtime exception and stop once it encounters the reference to the unknown API.
  - For new and incompatible syntax, the solution is transpiling. Transpiling is a contrived and community-invented term to describe using a tool to convert the source code of a program from one form to another (but still as textual source code). Typically, forwards-compatibility problems related to syntax are solved by using a transpiler (the most common one being Babel ([https://babeljs.io](https://babeljs.io/))) to convert from that newer JS syntax version to an equivalent older syntax.

    ```jsx
    // old snipped code
    if (something) {
      let x = 3;
      console.log(x);
    } else {
      let x = 4;
      console.log(x);
    }

    // babel transpile code
    var x$0, x$1;

    if (something) {
      x$0 = 3;
      console.log(x$0);
    } else {
      x$1 = 4;
      console.log(x$1);
    }
    ```

  - If the forwards-compatibility issue is not related to new syntax, but rather to a missing API method that was only recently added, the most common solution is to provide a definition for that missing API method that stands in and acts as if the older environment had already had it natively defined. This pattern is called a polyfill (aka “shim”).

    ```jsx
    /*
     The following code uses an ES2019 feature, the finally(..) method on the promise prototype.
    If this code were used in a pre-ES2019 environment, the finally(..) method would not exist,
    and an error would occur.
    */

    // getSomeRecords() returns us a promise for some // data it will fetch
    var pr = getSomeRecords();
    // show the UI spinner while we get the data
    startSpinner();
    pr
    .then(renderRecords) // render if successful
    .catch(showError) // show an error if not
    .finally(hideSpinner) // always hide the spinner

    // A polyfill for finally(..) in pre-ES2019 environments could look like this:
    if (!Promise.prototype.finally) {
     Promise.prototype.finally = function f(fn){
      return this.then(
       function t(v){
        return Promise.resolve(fn())
         .then(function t(){
          return v;
         });
       },
       function c(e){
        return Promise.resolve(fn())
         .then(function t()) {
          throw e;
         }
       }
      );
     };
    }
    ```

- Transpilation and polyfilling are two highly effective techniques for addressing that gap between code that uses the latest stable features in the language and the old environments a site or application needs to still support.

- HTML and CSS, by contrast, ar

## Interpretation

- The majority opinion seems to be that JS is an interpreted (scripting) language. But the truth is more complicated than that.
  - Languages regarded as “compiled” usually produce a portable (binary) representation of the program that is distributed for execution later. Since we don’t really observe that kind of model with JS (we distribute the source code, not the binary form), many claim that disqualifies JS from the category.
  - The real reason it matters to have a clear picture on whether JS is interpreted or compiled relates to the nature of how errors are handled.
  - JS source code is parsed before it is executed. It calls for “early errors” to be reported before the code starts executing and those errors cannot be recognized without the code having been parsed. But the JS “compilation” produces a binary byte code (of sorts), which is then handed to the “JS virtual machine” to execute.
  - JS is a compiled language.
  - The reason that matters is, since JS is compiled, we are informed of static errors. That is a substantively different interaction model than we get with traditional “scripting” programs, and arguably more helpful!

## Web Assembly

- WASM is a representation format more akin to Assembly (hence, its name) that can be processed by a JS engine by skipping the parsing/compilation that the JS engine normally does. The parsing/compilation of a WASM-targeted program happen ahead of time (AOT); what’s distributed is a binarypacked program ready for the JS engine to execute with very minimal processing.
  - WASM is a representation format more akin to Assembly (hence, its name) that can be processed by a JS engine by skipping the parsing/compilation that the JS engine normally does. The parsing/compilation of a WASM-targeted program happen ahead of time (AOT); what’s distributed is a binarypacked program ready for the JS engine to execute with very minimal processing.
  - If a language like Go supports threaded programming, but JS (the language) does not, WASM offers the potential for such a Go program to be converted to a form the JS engine can understand, without needing a threads feature in the JS language itself.
  - WASM isn’t only for the web, and also isn’t JS and WASM it will not replace JS. WASM significantly augments what the web (including JS) can accomplish.

## Strictly Speaking

- Back in 2009 with the release of ES5, JS added strict mode as an opt-in mechanism for encouraging better JS programs.
- Strict mode shouldn’t be thought of as a restriction on what you can’t do, but rather as a guide to the best way to do things so that the JS engine has the best chance of optimizing and efficiently running the code.
- Most JS code is worked on by teams of developers, so the strictness of strict mode (along with tooling like linters!) often helps collaboration on code by avoiding some of the more problematic mistakes that slip by in non-strict mode.
- Strict mode is like a linter reminding you how JS should be written to have the highestquality and best chance at performance. If you find yourself feeling handcuffed, trying to work around strict mode, that should be a blaring red warning flag that you need to back up and rethink the whole approach.

- Strict mode is switched on per file with a special pragma (nothing allowed before it except comments/whitespace):
  `"use strict";`
- Can alternatively be turned on per-function scope, with exactly the same rules about its surroundings, but the only valid reason to use a per-function approach to strict mode is when you are converting an existing non-strict mode program file and need to make the changes little by little over time. Otherwise, it’s vastly better to simply turn strict mode on for the entire file/program.

  ```jsx
  function someOperations() {
    // whitespace and comments are fine here
    "use strict";
    // all this code will run in strict mode
  }
  ```

- ES6 modules assume strict mode, so all code in such files is automatically defaulted to strict mode.

## Defined

- JS is an implementation of the ECMAScript standard (version ES2019 as of this writing), which is guided by the TC39 committee and hosted by ECMA. It runs in browsers and other JS environments such as Node.js.
- JS is a multi-paradigm language, meaning the syntax and capabilities allow a developer to mix and match (and bend and reshape!) concepts from various major paradigms, such as procedural, object-oriented (OO/classes), and functional (FP).
- JS is a compiled language, meaning the tools (including the JS engine) process and verify a program (reporting any errors!) before it executes.
