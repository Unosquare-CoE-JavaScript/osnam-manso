# Core Types

The core primitive types in TypeScript are all lowercase!

<p align="left">
<img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7957ca77-c09d-446f-8dc4-52680ed930f5%2FScreen_Shot_2021-10-29_at_20.37.10.png?id=67ec4942-9cae-44e8-9f05-b48939346504&table=block&spaceId=39c865bd-d151-4ddd-bfd8-f2239f411ed9&width=2000&userId=cc2028a7-e873-4ae8-988c-88e12db2775f&cache=v2" />
</p>

# Type Assignment & Type Inference

```jsx
let number: number;
number = 7;
const number1 = 5;
const number2 = 2.8;
const printResult = true;
const resultPhrase = "Result is: ";
const result = add(number1, number2, printResult, resultPhrase);
```

# Object Types

```jsx
// object type no javascript object
const person: {
  name: string,
  age: number,
} = {
  name: "Maximilian",
  age: 30,
};

// best syntax
const person2 = {
  name: "Maximilian2",
  age: 31,
};
console.log(person2.name);
```

# Arrays and Tuple Type

```jsx
const person = {
  name: "Maximilian",
  age: 30,
  hobbies: ["Sports", "Cooking"],
  role: [number, string];
};
let favorityActivities: string[];
favoriteActivities = ["Sports"];
person.role.push('admin')
```

# Enum

```jsx enum Role {
  ADMIN,
  READ_ONLY,
  AUTHOR,
  AGE = 18,
}

const person = {
  name: "Maximilian",
  age: 30,
  hobbies: ["Sports", "Cooking"],
  role: Role.ADMIN,
};
```

# Any

```jsx
let favoriteActivities: any[];
favoriteActivities = ["Sports"];
```

# Union Types

```jsx
function combine(input1: number | string, input2: number | string) {
  let result;
  if (typeof input1 === "number" && typeof input2 === "number")
    result = input1 + input2;
  else result = input1.toString() + input2.toString();
  return result;
}

const combineAges = combine(30, 26);
console.log(combineAges);

const combineNames = combine("Max", "Anna");
console.log(combineAges);
```

# Literal Types

```jsx
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: "as-number" | "as-text"
) {
  let result;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  )
    result = +input1 + +input2;
  else result = input1.toString() + input2.toString();
  return result;
}

const combineAges = combine(30, 26, "as-number");
console.log(combineAges);

const combineNames = combine("Max", "Anna", "as-text");
console.log(combineAges);
```

# Type Aliases / Custom Types

```jsx
type Combinable = number | string;
type ConversionDescriptor = "as-number" | "as-text";

function combine(
  input1: Combinable,
  input2: Combinable,
  resultConversion: ConversionDescriptor
) {
  let result;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  )
    result = +input1 + +input2;
  else result = input1.toString() + input2.toString();
  return result;
}

const combineAges = combine(30, 26, "as-number");
console.log(combineAges);

const combineNames = combine("Max", "Anna", "as-text");
console.log(combineAges);
```

# Type Aliases & Object Types

Type aliases can be used to "create" your own types. You're not limited to storing union types though - you can also provide an alias to a (possibly complex) object type.

For example:

```jsx
type User = { name: string, age: number };
const u1: User = { name: "Max", age: 30 }; // this works!
```

This allows you to avoid unnecessary repetition and manage types centrally.

For example, you can simplify this code:

```jsx
function greet(user: { name: string, age: number }) {
  console.log("Hi, I am " + user.name);
}

function isOlder(user: { name: string, age: number }, checkAge: number) {
  return checkAge > user.age;
}
```

# Function Return Types & void

```jsx
function add(n1: number, n2: number): number {
  return n1 + n2;
}

function printResult(num: number): void {
  console.log("Result" + num);
}

function printResult_2(num: number): void {
  console.log("Result" + num);
  return;
}
function printResult_3(num: number): undefined {
  console.log("Result" + num);
  return;
}

printResult(add(5, 12));
```

# Functions as Types

```jsx
function add(n1: number, n2: number): number {
  return n1 + n2;
}

function printResult(num: number): void {
  console.log("Result" + num);
}

printResult(add(5, 12));

let combineValues: (a: number, b: number) => number;

combineValues = add;

console.log(combineValues(8, 8));
```

# Function Types & Callbacks

```jsx
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
  const result = n1 + n2;
  cb(result);
}

addAndHandle(10, 20, (result) => {
  console.log(result);
  return result;
});
```

# The "unknown" Type

```jsx
let userInput: unknown; // better that any
let userName: string;
userInput = 5;
userInput = "Max";
userName = userInput; // type 'unknown is not assignable to type 'string'

// You can this and works.
if (typeof userInput === "string") {
  userName = userInput;
}
```

# The "never" Type

```jsx
function generateError(message: string, code: number) {
  throw { message: message, errorCode: code }; // return "never": is nothing
  //   while (true) {}
}

const result = generateError("An error occurred!", 500);
```
