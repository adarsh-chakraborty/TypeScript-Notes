
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
