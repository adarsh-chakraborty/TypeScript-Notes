
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
---
### Function as return Type

Function Types is a type that describe a function.

1. Function type
With `Function` type, It is can store any kind of function in the variable but it's not useful for most cases.

```javascript
// This is allow any function to be stored
let calculateAge: Function;
```

2. Function with arguements and return type.

Syntax: `() => returnType`

```javascript
let greetAll: () => string;
let greetWithName: (name: string) => string;
```

Here, without using curly braces `{}` we simply **declare an arrow function pointing to the return type** of the function.

We can also specify what kind of arguements the function will will accept by the following syntax.

Syntax: `(a: number, b: number ) => returnType`

The parameter names doesn't have to be the same as the function that it will evantually be stored in the variable. Only the type of the arguements matter.

3. Callback Function Type 

We can declare a parameter of the function to be a function that will be a evantually called with arguements.

```javascript
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
    const result = n1+n2;
    cb(result);
}

// cb will accept 1 arguement which should be a number and does not return anything so void.

// Let's call the function and pass a function

addAndHandle(2,4,(result) => {
    console.log(result)
});
```
Name of function arguements type doesn't have to be same. `num` is declared in the callback type but `result` variable is used.

--- 

### unknown Type 

unknown type is different from `any` type in TypeScript. It sure does allow us to store any kind of in the variable of type unknown.

```javascript
let userInput: unknown;

userInput = 5; // allowed
userInput = 'Max'; // allowed

let userName = 'Admin';

userName = userInput; // ERROR
// ERROR: Unknown is not assignable to string userName. (Any type would still work)
```

It allowed to store string and number just like the `any` type but is not assignable to variable of other core types. 

To assign unknown type variables to other variables of core type we will have to do a if check.

```javascript
let userInput: unknown;

userInput = 5; // allowed
userInput = 'Max'; // allowed

let userName = 'Admin';

if(typeof userInput ==='string'){
    userName = userInput; // Allowed after if check
}

```

TypeScript will detect the if statement before assignment of the unknown variable to a core variable and the error will be gone.


---

### never type 

never is another type functions can return just like a void but for some specific function which never yields to a return value.

Example:
```javascript
function AppError(message: string, code: number){
    throw {
        message: message,
        errorCode: code
    }
}

// Invoking the above function
generateError("An Error occured", 500);
```

Here, after the throw statement the further execution of the function is stopped so it `never` yields to a return statement even if we try to put it in a variable.

It will not even log `undefined` because after the throw statement, the further code is unreachable.

Such kind of function can be said to have a return type of `never`.

```javascript
function AppError(message: string, code: number): never {
    throw {
        message: message,
        errorCode: code
    }
}

// Invoking the above function
generateError("An Error occured", 500);
```

It is actually similar to void and void was used before the `never` type as it's fairly new compared to other core types. But it's always better to be more specific.

Another kind of function type never will be a function containing a infinite loop.


---

## TypeScript Configuration

### Compile a TS file

```shell
tsc file_name.js
```

### Enable Watch Mode for a single file

```shell
tsc file_name.js --watch
tsc file_name.js -w
```

### Enable Watch Mode for the entire project

In the root project folder:
```shell
tsc --init
```

The above command will initialize the project as a typescript project and creates a `tsconfig.json` file.

Now we can use tsc command without pointing to any file to compile all typescript files.
```shell
tsc
```

#### Compile with Watch Mode
To compile and watch for typescript file changes in our entire project, we can simply

```shell
tsc -w
// OR
tsc --watch
```

## Including and Excluding Files

