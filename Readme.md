
# TypeScript

## Core Types 

### number

`number:` `1, -2, 3.5`

---

### string 

`string:` 
```javascript
"hello"
'hello'
`hello`
```
---
### boolean

`true` `false`

---

### object

```javascript
// OR INFERRED TYPE

const person = {
    name: 'Adarsh Chakraborty',
    age:24
}

// OR MORE EXPLICIT

const person: {
    name: string,
    age: number
} = {
    name: 'Adarsh Chakraborty',
    age:24
}

// OR NO EXPLICIT (NOT RECOMMENDED, LOSES TS FEATURES)
const person: object = {
    name: 'Adarsh',
    age: 24
}
```

---


### array

Syntax: `primitiveType[]`
```javascript
let hobbies: string[];
// Array of Strings
hobbies = ['Coding', "Anime"];
```

---

### tuple

Tuple is like an array of fixed length and fixed type.

```javascript
const person = {
    name: 'Adarsh',
    age: 24,
    role: [2, 'author']
}
```

Here, role can hold either numeric and string and the following operations can be allowed:

person.role.push('admin');
person.role[1] = '10';

Because TypeScript only cares about the role being having the allowed types and for us to be explicit about it we can do the following changes:

```javascript
const person: {
    name:string,
    age: number,
    hobbies: string[],
    role: [number, string]
} = {
    name: 'Adarsh',
    age: 24,
    role: [2, 'author']
}
```
Now the following code will be marked for error:
`person.role[1] = '10';`

Because, we explicitly told typescript that the first item of the array would be a number and second item will be a string. 

Hence the error.

This is called Tuple.
An Array having specific length, specific type and exactly as in that order.

**Note:** push method is allowed even tho we set the tuple to have an exact items, This is something we have to be aware of.


---


### enum type  

Enum is like a list of global constants of predefined numerical values having special human readable Identifiers. 

Syntax: `enum {USER, ADMIN}`

```
// Example with Enum
enum Role {ADMIN, READ_ONLY, AUTHOR};
// the above identifers will have the values 0 - 2

enum Role {ADMIN = 5, READ_ONLY, AUTHOR};
// the above identifers will have the values 5 - 7

enum Role {ADMIN = 5, READ_ONLY = 100, AUTHOR= 30};
// the above identifers will have the values 5,100, 30 respectively

/* WE CAN ALSO HAVE ENUMS OF MIXED VALUES */
enum Role {USER = 1, READ_ONLY = "READ_ONLY", AUTHOR= 30, SUPERUSER = "SUPER_USER"};

```

Approach without using Enum:

```javascript

// Global constants (without enum)
const ADMIN = 0;
const USER = 1;
const AUTHOR = 2;

const person = {
    name: 'Adarsh',
    age: 24,
    role: USER
}

// common practice without enums
if(person.role === USER){
    // do something
}
```
Example with Enum:
```javascript
enum Role {ADMIN, READ_ONLY, AUTHOR};
// the above identifers will have the values 0 - 2

enum Role {ADMIN = 5, READ_ONLY, AUTHOR};
// the above identifers will have the values 5 - 7

enum Role {ADMIN = 5, READ_ONLY = 100, AUTHOR= 30};
// the above identifers will have the values 5,100, 30 respectively

const person = {
    name: 'Adarsh',
    age: 24,
    role: Role.ADMIN
}

if(person.role === Role.ADMIN){
    // do something
    // no need to manage global constants
}
```


**"Human readable and Mapped Values"**

---

### any Type 

It is the most flexible type, It basically means we can store any kind of values. TypeScript will never

Syntax: `let var_name: any`

```javascript
// Variable that can store any value
let variable_name: any = "...";

// Array of any type, can store any kind of values
const favoriteActivies = any[];
```

Not really RECOMMENDED. Loses typescript features.

---
### union type
Union type can accept more than one type. We use pipeline operator to mention more than one type. It makes the variable more flexible.

Example:
```javascript
let person: number | string = "SOME VALUE"; 

// OR

function add(a: number | string, b: number | string){
    return a+b;
}
```

Here, the person variable can now be a number or a string.


### literal type 

A TypeScript string literal type defines a type that accepts specified literal value. It is more like locked on the value instead of the type.

Example:

```javascript
const p = 2.8;
```

Here, p is a constant and the type of the p will be 2.8 which is derived from number type but is more specific type. 

```javascript
function doSomething(action: 'ADD' | 'SUB'){
    // do something amazing with action
}
```

Here, the action is a string and not just any string, a specific value of the string.

Literal types based on core types but with specific version. Here we used union with string to create a literal type. 

---

### Type Aliases Custom Types 

We can create a type alias with `type` keyword. It can be used to create our own custom type. Just the name shouldn't be be same as any existing type such as Date type.

```javascript
type Age = number; // number 
type Combinable = number | string; // union
type Result = 'as-text' | 'as-number'; // Literal
type User = { name: string; age: number }; // object

const u1: User = { name: 'Max', age: 30 }; // User

function (personAge: Age, input: Combinable, result: Result){
 // do something
}

// This allows you to avoid unnecessary repetition and manage types centrally.
// For example, you can simplify this code:

function greet(user: { name: string; age: number }) {
  console.log('Hi, I am ' + user.name);
}

function isOlder(user: { name: string; age: number }, checkAge: number) {
  return checkAge > user.age;
}

// To

type User = { name: string; age: number };

function greet(user: User) {
  console.log('Hi, I am ' + user.name);
}

function isOlder(user: User, checkAge: number) {
  return checkAge > user.age;
}
```
---
#### Function Return Type 
TypeScript generally infers the return type based on the value returned but we can also explicitly mention a return type of a function.

```javascript
// Automatically Inferred by TypeScript
function add(n1: number, n2: number){
    return n1+n2;
}

// Return type set explicitly
function add(n1: number, n2: number) : number{
    return n1+n2;
}

// Void type
function printResult(value: number){
    console.log('Result', num);
    // Nothing returned, Void
}
```
If you have no reason to explicitly set the return type, It's better to leave this job to typescript to infer it automatically.

**Difference between void and undefined.**

With void, we use or don't use return statements but if the function's return value is undefined then there should atleast be a return statement in the code.
```javascript
function printResult(value: number){
    console.log('Result', num);
    return; // optional in void
}

// RARE, void is mostly used instead that too inferred.
function printResult(value: number): undefined{
    console.log('Result', num);
    return; // return statement is necessary
}
```
