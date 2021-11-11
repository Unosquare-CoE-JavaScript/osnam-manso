# "let" and "const"

- block scope

```jsx
const userName = "Max";
//userName = "Maximilian"; error
let age = 30;
age = 29;
if (age > 20) {
  let isOld = true;
}
// console.log(isOld); error
```

# Arrow Functions

```jsx
const add = (a: number, b: number) => a + b;

oconst printOutput= (output: string | number) => console.log(output)

oconst printOutput= (a: string | number) => void = output => console.log(output)

const button = document.querySelector('button')
if()
```

# Default Function Parameters

- Default parameters always to the end.

```jsx
const add = (a: number, b: number = 1) => a + b;
add(5);
```

# The Spread Operator (...)

```jsx
const hobbies = ["Sports", "Cooking"];
const activeHobbies = ["Hiking"];
activeHobbies.push(...hobbies);


cosnt person = {
name: 'Max',
age: 30
}

const copiedPerson = {...person}
```

# Rest Parameters

```jsx
const add = (...numbers: number[]) => {
  // code here
};
add(6, 34, 38, 34);
```

# Array and Object Destructuring

```jsx
const [hobby1, hobby2, ...remainingHobbies] = hobbies;

cosnt person = {
name: 'Max',
age: 30
}
const {name: userName, age} = person
```
