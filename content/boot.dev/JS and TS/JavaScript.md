# Print to console
- `console.log` is used to print to console.
- `console.log("edit me");`
# Variables
## Basic Variables
- The var keyword declares a variable 
- Some of JavaScript's most common variable types are:
	- number: represents both integer and floating-point numbers
	- boolean: either true or false
	- string: a sequence of characters
	- undefined: a variable that hasn't been assigned a value
```js
var smsSendingLimit = 100;
var isAdmin = true;
var username = "wagslane";
var nothing = undefined;
```
### Let and Const
- The `var` keyword is the "old" way to declare variables in JavaScript.
- The `let` keyword is for variables that can be reassigned, while `const` is for variables that can't.

>[!Warning]
>The `var` keyword is function-scoped instead of block-scoped, meaning when it's used inside an if block the variable leaks out, while let and const don't.

```js
let username = "dengar_the_bh";
username = "boba_fett";

const smsSendingLimit = 1000;
```

## Why JS?
- JavaScript is the only language that runs in the browser. 
- If your app is accessed via a web browser, it's almost certainly gonna need some JavaScript.
- Disadvantage
	- It is not statically typed (although TypeScript helps)
	- It has legacy baggage (like var and old code that doesn't use promises)
	- Always acts slightly differently in every browser/runtime
- Advantage
	- Many built-in features that are particularly useful on the web
	- Can be used for both front-end and back-end development, simplifying an org's tech stack
	- Great support for asynchronous programming
## Comment
- JavaScript has two styles of comments
```Js
// This is a single-line comment

/*
   This is a multi-line comment
   neither of these comments will execute
   as code
*/
```

## Numbers in JS
- In JavaScipt all numbers are just a [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) type.
- The JavaScript `Number` type is a [double-precision 64-bit binary format IEEE 754](https://en.wikipedia.org/wiki/Double_precision_floating-point_format "External link (opens in new tab)") value, like `double` in Java or C#.
- This means it can represent fractional values, but there are some limits to the stored number's magnitude and precision. 
- Very briefly, an IEEE 754 double-precision number uses 64 bits to represent 3 parts:
	- 1 bit for the _sign_ (positive or negative)
	- 11 bits for the _exponent_ (-1022 to 1023)
	- 52 bits for the _mantissa_ (representing a number between 0 and 1)
		- The mantissa stores the digits after the leading **1**. IEEE 754 does **not store the first 1**.
		- Because after normalization, it's always 1. This saves one bit.
	- $\text{Number} = (-1)^{\text{sign}}.(1+\text{mantissa}).2^{\text{exponent}}$
	- Storing `5.75 = 101.11₂`, `5 = 101`, `0.75 = 0.11₂` ; Normalize `1.0111 × 2²`;`Sign->0,Exponent->2,Mantissa->0111000...`
	- Reconstruct `Sign->0,Exponent->3,Mantissa->101`; `1.101₂ = 1 + 1/2 + 0/4 + 1/8 = 1.625`;`1.625 × 2³ = 1.625 × 8 = 13`.

```Js
let x = 2; // this is a number
x = 5.69; // this is also a number
x = -5.42; // yup, still a number
let sum = 2 + 3 + 7; // 12
let difference = 5.3 - 2.1; // 3.2
let product = 2 * 3; // 6
let quotient = 6 / 2; // 3
```
### Why do 0.1 + 0.2 problems happen?
- Because some decimal numbers cannot be written exactly in binary.
	- `0.1 = 0.0001100110011001100...`
- It repeats forever. The computer only has **52 mantissa bits**, so it cuts it off.

```
0.1 + 0.2
0.10000000000000000555... 
+ 
0.20000000000000001110...
=
0.30000000000000004
```
## Increment and Decrement
- We use the `+=` operator to increment a number and `-=` to decrement a number
- IT also has a `++` operator for when you only want to increment by `1`.
	- Postfix, with operator after operand (`x++`), the increment operator increments and returns the value before incrementing.
	- Prefix, with operator before operand (`++x`), the increment operator increments and returns the value after incrementing.
	- The increment operator can only be applied on operands that are references (variables and object properties; i.e., valid assignment targets).
	- `++x` itself evaluates to a value, not a reference, so you cannot chain multiple increment operators together.

```Js
let bootdevCourseRating = 4;
bootdevCourseRating++;
console.log(bootdevCourseRating); // 5
bootdevCourseRating += 5;
console.log(bootdevCourseRating); // 10

let bootdevCourseRating = 11;
bootdevCourseRating--;
console.log(bootdevCourseRating); // 10
bootdevCourseRating -= 5;
console.log(bootdevCourseRating); // 5

let x = 3;
const y = x++;// x is 4; y is 3
let x2 = 3n;
const y2 = x2++;// x2 is 4n; y2 is 3n
let x = 3;
const y = ++x;// x is 4; y is 4
let x2 = 3n;
const y2 = ++x2;// x2 is 4n; y2 is 4n
```

##  Undefined vs. Undeclared
- `undefined` is a property of the global object  represents the primitive value [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Data_structures#undefined_type). That is, it is a variable in global scope.
- It is one of JavaScript's primitive types. 
- In all non-legacy browsers, `undefined` is a non-configurable, non-writable and non-emurable property.
- We can create an `undefined` variable by giving it a name, but no value.
- However if we never create the name, it's `undeclared`, and undeclared variables actually throw an error
- As an aside, you can use `const` to declare a variable that is assigned to `undefined`

```Js
let favoriteSandersonCharacter; // undefined
console.log(typeof favoriteSandersonCharacter); // "undefined"
console.log(favoriteRothfussCharacter); // ReferenceError: favoriteRothfussCharacter is not defined
const favoriteSandersonCharacter; // SyntaxError: missing = in const declaration
const favoriteSandersonCharacter = undefined; // undefined
```

## Null vs. Undefined
- One of JavaScript's most cursed features is that it has two values for "nothing":
	- `undefined`: It doesn't exist at all.  `undefined` is "very nothing"
	- `null`: It (kind of) exists, but it's empty. `null` is "kinda nothing"
- The keyword `null` is a literal for the `null` value. 
- Unlike `undefined`, which is a global variable, `null` is not an identifier but a syntax keyword.
- `undefined` represents the absence of a value, while `null` represents the absence of an object.
- `null` has the following behaviors:
	- Like `undefined`, accessing any property on `null` throws a `TypeError`
	- Like `undefined`, `null` is treated as falsy for boolean operations, and nullish for nullish coalescing.
	- The `typeof null` result is `"object"`. This is a bug in JavaScript that cannot be fixed due to backward compatibility.
	- Unlike `undefined`, `JSON.stringify()` can represent `null` faithfully.
	- Unlike `undefined` which is default value of a variable when it has not been assigned. `null` must be assigned

```JS
let myName;
console.log(myName); // undefined
let myName = null;
console.log(myName); // null
typeof null; // "object" (not "null" for legacy reasons)
typeof undefined; // "undefined"
null === undefined; // false
null == undefined; // true
null === null; // true
null == null; // true
!null; // true
Number.isNaN(1 + null); // false
Number.isNaN(1 + undefined); // true
```

- Only use `null` in cases where the behavioral difference matters, or relying on external code that forces to use `null`.
## Dynamic and Weak
- Like Python, Ruby, and PHP, JavaScript is a dynamically-typed (not statically-typed) language. 
- Its variable types are only known at runtime 
- unlike Python, it's also weakly-typed, meaning it will automatically convert types when you do things like add a number to a string.
```JS
let answerToLife = 42;
let answerToTheUniverse = "42";

// obviously JavaScript thinks that adding strings
// and numbers is totally sane and normal behavior
const answerToEverything = answerToLife + answerToTheUniverse;

console.log(answerToEverything);
// "4242"
```

## Same Line Declarations
- You can declare multiple variables on the same line:
```JS
let miles = 80276, org = "Tesla";
// The above is same as.
let miles = 80276;
let org = "Tesla";
```
## JavaScript Speed

- JavaScript is not as fast as C, Rust or Zig.
	- It's almost always going to be outperformed by non-garbage-collected languages.
- Modern JavaScript is typically JIT-compiled (just-in-time compiled) into machine code via the V8 engine at runtime. 
	- It's usually not as fast as AOT (ahead-of-time) compiled languages like Go or C, but it's usually much faster than interpreted languages like Python or Ruby.
- JavaScript runs on a single thread, but has great support for asynchronous programming.
	- It's typically not as performant for CPU-bound tasks (like heavy math calculations), but it does well for I/O-bound tasks (like contacting a database or making an API call).
## Strings
- In JavaScript, a (non-template) string can be written with either single or double quotes. 
- For example:
	- `'Hello'`
	- `"Hello"`.
- Prefer using double quotes
### Indexing

- Square brackets are used to access individual characters inside a string.
- The characters are numbered from `0 to length-1`. 
- The `.length` property is used to get the number of characters in a string.
```JS
const greeting = "Hello";
console.log(greeting[0]); // 'H'
console.log(greeting[1]); // 'e'
console.log(greeting[2]); // 'l'
console.log(greeting[3]); // 'l'
console.log(greeting[4]); // 'o'
// you can also get the last char at length-1
console.log(greeting[greeting.length - 1]); // 'o'
```
### .length
- Maximum string length
	- JavaScript numbers are IEEE-754 doubles.
	- The largest integer they can represent exactly is `2^53 − 1 = 9007199254740991`
	- How much memory would that need?
		- Assume every character uses `2 bytes` because UTF-16 code units are 16 bits. 
			- `2^53 × 2 bytes = 2^54 bytes = 16 PiB (pebibytes) = 16384 TiB`
	- So what do browsers actually do?
		- Instead of allowing `9 quadrillion` characters engines use much smaller limits.
		- For example, many modern JavaScript engines cap string lengths
		- At around 2³⁰ − 1 or 2²⁹ − 24 UTF-16 code units (the exact limit depends on the engine and whether it's a 32-bit or 64-bit build). That is roughly ≈ `1 billion code units` which already requires about 2 GB of memory (before additional engine overhead).
		- Thus implementations often keep the length in a 32-bit integer internally.
- String.length
	- `"hello".length`-> Output: `5` and `String.length` -> Output: `1`. They are completely unrelated.
	- First one is string's length and second one is How many declared (formal) parameters does this function have? Because `String` itself is a function
```JS
function add(a, b) {}
console.log(add.length); // 2
function hello(name, age, city) {}
console.log(hello.length); // 3
// String(value); Only one parameter.
console.log(String.length); // 1
```

## Template Literals
- In JavaScript, [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) are a fantastic way to interpolate dynamic values into a string.
- Template literals must start and end with a backtick, and anything inside of the dollar-sign bracket enclosure is automatically cast to a string.
- You aren't limited to just variable names inside the `${}`. You can actually write valid JavaScript code directly inside them.
- This means you can do math, change text styles, or run logic checks right inside the string.

```JS
const shadeOfRed = 101;
console.log(`The shade is ${shadeOfRed}`);
// The shade is 101
const price = 2.5;
const quantity = 3;
const item = "coffee";
console.log(`Total: $${price * quantity}`);
// Total: $7.5
console.log(`I need ${item.toUpperCase()}`);
// I need COFFEE
```
## Semi-Colons in JS
- In JavaScript, semicolons are optional as a terminating character.
- Terminating character allows the compiler to know when a statement ends without relying on whitespace or newlines.
- They can be inserted by the JavaScript engine automatically during the parsing phase.
```JS
// This works
let x = 5
let y = 10
// But we prefer this
let x = 5;
let y = 10;
```
## String Encoding
- JavaScript stores strings using `UTF-16`, and `.length` returns the number of `UTF-16 code units`, `not Unicode characters`.
- This allows for more than just the 128 ASCII characters, but also characters from non-English languages and other symbols.
- Unicode character may be encoded as one or two code units, so it's possible for the value returned by length to not match the actual number of Unicode characters in the string.
```JS
let str = "Hello";
console.log(str.length); // 5; Each letter fits into one UTF-16 code unit.
let str = "😊";
console.log(str.length); // 2; Because 😊 is outside the Basic Multilingual Plane (BMP).
/*
UTF-16 stores it as
High surrogate  16 bits
Low surrogate  +16 bits
			   =32 bits
which are 2 code units
*/
```
-  Why does UTF-16 do this?
	- Unicode currently has over 1 million possible code points.
	- A 16-bit number can store only `2^16 = 65,536`values.That wasn't enough. 
	- So Unicode introduced surrogate pairs.
-  What is a code unit?
	- Think of UTF-16 storage.
	- `String -> 16 bits`
	- Each block of `16 bits` is called a code unit
- Counting actual Unicode characters
	- Instead of `str.length` you can use 
		- Split the string with its iterator. `[..."😊"].length` ; Output `1`
		- `Array.from("😊").length`; Output `1`
	- These iterate over Unicode code points instead of UTF-16 code units.
-  UTF-8 files become UTF-16 strings
	- JavaScript decodes those UTF-8 bytes into a JavaScript string. JavaScript always stores strings as UTF-16.
	
```
UTF-8 File
      ↓ decode
JavaScript String (UTF-16)
```

### Camel Case in JS
- By convention in Python, Ruby, and Rust, most programmers use `snake_case` to write variable names. 
- In JavaScript, the more-popular convention (and the one we use) is `camelCase`.
- Here are some casing examples:
	- `camelCase`
	- `snake_case`
	- `PascalCase`
	- `SCREAMING_SNAKE_CASE`
# Comparisons
## Conditionals
- `if, else if, else` statements in JavaScript use parentheses around the condition:
- 
```javascript
if (height > 6) {
  console.log("You are super tall!");
} else if (height > 4) {
  console.log("You are tall enough!");
} else {
  console.log("You are not tall enough!");
}
```
- `===` equal to
- `!==` not equal to
- `<` less than
- `>` greater than
- `<=` less than or equal to
- `>=` greater than or equal to
## Comparison Operators
```javascript
5 < 6; // evaluates to true
5 > 6; // evaluates to false
5 >= 6; // evaluates to false
5 <= 6; // evaluates to true
```
- To compare two values to see if they are exactly the same, use the strict equality (`===`) and strict inequality (`!==`) operators
- The "strict equals" (`===`) and "strict not equals" (`!==`) compare both the value and the type. 
```JS
5 === 6; // evaluates to false
5 !== 6; // evaluates to true
```

- The "normal" equality (`==`) and inequality (`!=`) operators are a bit more... flexible
- The "loose equals" (`==`) and "loose not equals" (`!=`) attempt to convert and compare values of different types. 
```JS
5 == 6; // evaluates to false
5 == "5"; // evaluates to true

5 != 6; // evaluates to true
5 != "5"; // evaluates to false
```

| x                   | y             | `==`  | `===` |
| ------------------- | ------------- | ----- | ----- |
| `undefined`         | `undefined`   | true  | true  |
| `null`              | `null`        | true  | true  |
| `true`              | `true`        | true  | true  |
| `false`             | `false`       | true  | true  |
| `'foo'`             | `'foo'`       | true  | true  |
| `0`                 | `0`           | true  | true  |
| `+0`                | `-0`          | true  | true  |
| `+0`                | `0`           | true  | true  |
| `-0`                | `0`           | true  | true  |
| `0n`                | `-0n`         | true  | true  |
| `0`                 | `false`       | true  | false |
| `""`                | `0`           | true  | false |
| `'0'`               | `0`           | true  | false |
| `'17'`              | `17`          | true  | false |
| `[1,2]`             | `'1,2'`       | true  | false |
| `new String('foo')` | `'foo'`       | true  | false |
| `null`              | `undefine`    | true  | false |
| `null`              | `false`       | false | false |
| `undefined`         | `false`       | false | false |
| `{foo:'bar'}`       | `{foo:'bar'}` | false | false |
| `0`                 | `null`        | false | false |
| `0`                 | `NaN`         | false | false |
| `'foo'`             | `NaN`         | false | false |
| `NaN`               | `NaN`         | false | false |

## Logical Operators

- `&&` (and) - Returns `true` if both conditions are `true`
- `||` (or) - Returns `true` if either of the conditions are `true`
- `!` (not) - Returns `true` only if the input is `false`
```Js
true && true; // true
true && false; // false
true || false; // true
false || false; // false
!false; // true
!true; // false
```
## Switch
- The **`switch`** statement evaluates an expression
- Matching the expression's value against a series of `case` clauses (using the strict equality comparison)
- And executes statements after the first `case` clause with a matching value, until a `break` statement is encountered.
- The `default` clause of a `switch` statement will be jumped to if no `case` matches the expression's value.
- The clause expressions are only evaluated when necessary — if a match is already found, subsequent `case` clause expressions will not be evaluated, even when they will be visited by fall-through.

```JS
switch (expression) {
  case caseExpression1:
    statements
  case caseExpression2:
    statements
  // …
  case caseExpressionN:
    statements
  default:
    statements
}
```
- `expression` 
	- An expression whose result is matched against each `case` clause.
- `caseExpressionN` (Optional)
	- A `case` clause used to match against `expression`.
	- If the value of `expression` matches the value of any `caseExpressionN`, execution starts from the first statement after that `case` clause until either the end of the `switch` statement or the first encountered `break`.
- `default` (Optional)
	- this clause is executed if the value of `expression` doesn't match any of the `case` clauses.
	- A `switch` statement can only have one `default` clause

```Js
const os = "mac";
let creator;
switch (os) {
  case "linux":
    creator = "Linus Torvalds";
    break;
  case "windows":
    creator = "Bill Gates";
    break;
  case "mac":
    creator = "Steve";
    break;
  default:
    creator = "Unknown";
    break;
}

console.log(creator);
// Steve
```
### Breaking and fall-through
- If break is omitted, execution will proceed to the next case clause, even to the default clause, regardless of whether the value of that clause's expression matches. 
- This behavior is called "fall-through".
- Hence include a `break/return/continue` statement after each case to prevent this behavior.
### Lexical scoping
- The `case` and `default` clauses are like labels
- they indicate possible places that control flow may jump to. 
- However, they don't create lexical scopes themselves (neither do they automatically break out).
- Hence whenever you need to use `let` or `const` declarations in a `case` clause, wrap it in a block.

```Js
const action = "say_hello";
switch (action) {
  case "say_hello": {
    const message = "hello";
    console.log(message);
    break;
  }
  case "say_hi": {
    const message = "hi";
    console.log(message);
    break;
  }
  default: {
    console.log("Empty action received.");
  }
}
```
### default clause between two case clauses
- If no match is found, execution will start from the `default` clause, and execute all statements after that.
```Js
const foo = 5;
switch (foo) {
  case 2:
    console.log(2);
    break; // it encounters this break so will not continue into 'default:'
  default:
    console.log("default");
  // fall-through
  case 1:
    console.log("1");
}
// default
// 1

```
## Ternary Operator
- The ternary operator makes it easy to write a conditional as a single expression.
- `const price = isMember ? "$2.00" : "$10.00";`
- It is same as If `isMember` is true, evaluate to $2.00, otherwise evaluate to $10.00.
- It called ternary because it's the only JavaScript operator that takes three operands.
- Syntax
	- `condition ? exprIfTrue : exprIfFalse`
	- condition
		- An expression whose value is used as a condition.
	- `exprIfTrue`
	    - An expression which is executed if the condition evaluates to a truthy value 
	    - one which equals or can be converted to true.
	- `exprIfFalse`
	    - An expression which is executed if the condition is falsy 
	    - that is, has a value which can be converted to false.
### Conditional chains
- The ternary operator is right-associative, which means it can be "chained".

```Js
function example() {
  return condition1 ? value1
    : condition2 ? value2
    : condition3 ? value3
    : value4;
}
//-------------------------
const vehicleName = isTruck
  ? "truck"
  : isCar
    ? "car"
    : isScooter
      ? "scooter"
      : "vehicle";
```
- This is equivalent to
```JS
function example() {
  if (condition1) {
    return value1;
  } else if (condition2) {
    return value2;
  } else if (condition3) {
    return value3;
  } else {
    return value4;
  }
}
//----------------------
function getVehicleName(isTruck, isCar, isScooter) {
  if (isTruck) {
    return "truck";
  }
  if (isCar) {
    return "car";
  }
  if (isScooter) {
    return "scooter";
  }
  return "vehicle";
}
```

## Truthy and Falsy

- JavaScript uses type conversion to coerce any value to a Boolean in contexts that require it, such as conditionals and loops.
- A "truthy" value is a value that is considered `true` when encountered in a Boolean context.
- In JavaScript, you don't need to explicitly convert a value to a Boolean before using it in a conditional
- All values are truthy unless they are defined as falsy.
- That is, all values are truthy except `false, 0, -0, 0n, "", null, undefined, NaN, and document.all`.
- Common truthy values include `true`, `42` (any number that isn't 0), `"hello"` (any non-empty string), `[]` (an empty array), `{}` (an empty object), `function() {}` (an empty function)`

```Js
if ("hello") {
  console.log("hello is truthy");
}
if (42) {
  console.log("42 is truthy");
}
// hello is truthy
// 42 is truthy
if (!0) {
  console.log("0 is falsy");
}
if (!null) {
  console.log("null is falsy");
}
if (!undefined) {
  console.log("undefined is falsy");
}
```

## Nullish Coalescing

- JavaScript is truly a language built for the web, and the web runs on... uncertainty. Thus JavaScript exists solely to handle cases where a value might be null or undefined.
- The nullish coalescing operator `??` is a way to handle these cases in a more concise way.

```JS
let myName = null;
console.log(myName ?? "Anonymous"); // "Anonymous"

myName = "Lane";
console.log(myName ?? "Anonymous"); // "Lane"
```
- If the value on the left of `??` is null or undefined, the value on the right is returned. 
- Otherwise, the value on the left is returned. 
- It's a way to set sane defaults for variables that might be empty.

### Assigning a default value to a variable
- when one wanted to assign a default value to a variable, a common pattern was to use the logical OR operator (`||`).
- However, due to `||` being a boolean logical operator, the left-hand-side operand was coerced to a boolean for the evaluation and any falsy value (including 0, '', NaN, false, etc.) was not returned.
- The nullish coalescing operator avoids this pitfall by only returning the second operand when the first one evaluates to either `null` or `undefined`.

```JS
const myText = ""; // An empty string (which is also a falsy value)

const notFalsyText = myText || "Hello world";
console.log(notFalsyText); // Hello world

const preservingFalsy = myText ?? "Hi neighborhood";
console.log(preservingFalsy); // '' (as myText is neither undefined nor null)
```

### Short-Circuiting
- The right-hand side expression is not evaluated if the left-hand side proves to be neither `null` nor `undefined`
- his happens with:
	- `||`
	- &&`
	- `??`
	- `?.`
- The right side is evaluated only if necessary
```JS
user ?? getDefaultUser()
//If user already exists,
//why waste time creating another user?
//So JavaScript skips the second function.
```

### Optional Chaining (?.)
- It is useful to access a property of an object which may be `null` or `undefined`.
- Combining them, you can safely access a property of an object which may be nullish and provide a default value if it is.
- Optional chaining also short-circuits.
```JS
const person = { name: "Alice" };
console.log(person.name); // "Alice"
console.log(person.address.city) // TypeError: Cannot read properties of undefined
// person.address -> undefined; undefined.city -> Error
console.log(person.address?.city) // undefined
//Is address null or undefined? -> undefined
const foo = { someFooProp: "hi" };
console.log(foo.someFooProp?.toUpperCase() ?? "not available"); // "HI"
console.log(foo.someBarProp?.toUpperCase() ?? "not available"); // "not available"
// foo.someBarProp -> undefined
// Optional chaining sees undefined; It immediately returns undefined; It doesn't execute toUpperCase()
// undefined ?? "not available -> "not available"
```
# Functions
- JavaScript supports functions via the [function keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function) .

```Js
// function declaration
function getSum(a, b) {
  return a + b;
}

// function call
const result = getSum(60, 9);

console.log(result);
// 69
```
## Function Hoisting
- In JavaScript As long as a function is defined somewhere in the file, it can be called even before the definition.
- This works because JavaScript "hoists" the function declaration to the top of the file before the code is executed.
- JavaScript Hoisting refers to the process whereby the interpreter appears to move the declaration of functions, variables, classes, or imports to the top of their scope, prior to execution of the code.
```Js
console.log(getLabel(3));
// prints 'awful'

function getLabel(numStars) {
  if (numStars > 7) {
    return "great";
  } else if (numStars > 3) {
    return "okay";
  } else {
    return "awful";
  }
}
```
### Hoisting
- In colloquial terms, any of the following behaviors may be regarded as hoisting:
    - Being able to use a variable's value in its scope before the line it is declared. ("Value hoisting")
    - Being able to reference a variable in its scope before the line it is declared, without throwing a `ReferenceError`, but the value is always undefined. ("Declaration hoisting")
    - The declaration of the variable causes behavior changes in its scope before the line in which it is declared.
    - The side effects of a declaration are produced before evaluating the rest of the code that contains it.
- The four function declarations (`function, function*, async function, async function*`) are hoisted with type 1 behavior
- `var` declaration is hoisted with type 2 behavior;
- `let`, `const`, and `class declarations` (also collectively called `lexical declarations`) are hoisted with type 3 behavior; 
- `import` declarations are hoisted with type 1 and type 4 behavior.
## Multiple Return Values
- In JavaScript, multiple return values are not allowed
```JS
function getUser() {
  return "name@domain.com", 21, "active";
  // DON'T DO THIS
  // it only returns 'active'
}
```
- the JavaScript code above won't actually throw any sort of error, it will just silently return the "active" string.
- most developers return an object that contains the values they want to return.
## Functions As Values
- JavaScript supports first-class and higher-order functions.
- Functions can be treated like any other data type – such as numbers and strings and booleans
-  First-Class Function :- A  function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.

```JS
//Assigning a function to a variable

const foo = () => {
  console.log("foobar");
};
foo(); // Invoke it using the variable
// foobar

// Passing a function as an argument
function sayHello() {
  return "Hello, ";
}
function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}
// Pass `sayHello` as an argument to `greeting` function
greeting(sayHello, "JavaScript!");
// Hello, JavaScript!

//Returning a function
function sayHello() {
  return () => {
    console.log("Hello!");
  };
}

```

> [!note] Note
> - Even if your function was named, you can use the variable name to invoke it. 
> - Naming it will be helpful when debugging your code. But it won't affect the way we invoke it.
> - The function that we pass as an argument to another function is called a callback function. 
> - `sayHello()` is a callback function
> - A function that returns a function or takes other functions as arguments is called a higher-order function.

## Scope
- The scope is the current context of execution in which values and expressions are "visible" or can be referenced.
- If a variable or expression is not in the current scope, it will not be available for use. 
- Scopes can also be layered in a hierarchy, so that child scopes have access to parent scopes, but not vice versa.
- It can behave differently depending on the environment (such as a browser or Node.js). 
- There are four levels, from highest to lowest:
	- Global Scope:
		- Variables declared globally have the highest level of scope and can be accessed from anywhere in your code.
		- In browsers, global variables are properties of the window object. 
		- For example, `window.myGlobalVar = 'hello world'` defines a global variable.
        - In Node.js, global variables are properties of the global object: `global.myGlobalVar = 'hello world'`.
    - Module Scope:
	    - In ES modules (both in Node.js and modern browsers), variables declared at the top level of a module are scoped to that module. 
	    - They are not added to the global scope.
	    - In the browser, using `<script type="module">` creates a module scope for that script.
    - Function Scope:
	    - Variables declared with `var` (we try to avoid this) are limited to the function scope. 
	    - They are accessible only within that function and any nested functions.
	- Block Scope:
		- ES6 introduced block scope with the let and const keywords. 
		- A block is typically defined by curly braces `{}`, like in if statements, loops, and other blocks of code.
        - Variables declared with let and const are confined to their block, making them more predictable and reducing the chances of accidental variable hoisting.
        - 
