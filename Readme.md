
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

**Exclude**
To exclude any file we add `exclude` option in the `tsconfig.json` file which holds an array of filenames, paths, wildcards of the files to exclude from TypeScript compilation.

```
"exclude" : [
    "analytics.js", // Ignore file named analytics.js
    "*.dev.ts", // Ignore any file with .dev.ts extension
    "**/*.dev.ts", // In any Folder having a dev.ts file should be ignored
    "node_modules", // Ignore the node_modules folder (Default)
]
```

**Note: If exclude option is not set, node_modules is automatically excluded as default setting.**

**Include**

When include option is set, we will have to mention all the files that has to be compiled, it will not watch for all ts files in the project anymore.

```
"include" : [
    "analytics.js", // Include file named analytics.js
    "app.ts", // Include file app.ts
    "src", // folder containing the ts file
]
```

If any anything that is part of include and also mentioned in exclude then it will be filtered and whatever mentioned in exclude will do get excluded in the compilation process.

So we can say, compilation = Include - Exclude

**Files option**
Files option is similar to includes option but here we can't specify whole folder but instead only the files. Good for small projects
```
"files" : [
    "analytics.js", // Include file named analytics.js
    "app.ts", // Include file app.ts
]
```

---

## TypeScript and Modern JavaScript

#### Classes

```javascript
// Class in javascript
class Department {
 name: string = 'Department Default Name';
 id: number;

 constructor(n: string){
     this.name = n;
 }

 describe(){
     console.log("Department: ", this.name);
 }
}


const accounting = new Department('Accounting');
accounting.describe() // Accounting

const accCopy = {describe: accounting.describe}; // pointer to the function

accCopy.describe(); // undefined
```

accCopy gives undefined because the **this** now refers to accCopy instead of the accounting Object. 

Because, this always refers to the object it's called upon, so in this case it's accCopy it is called on.

To prevent this in typescript, we can write the same function...


```javascript
// Class in javascript
class Department {
 name: string = 'Department Default Name';
 id: number;

 constructor(){
   this.name = n;
 }

 describe(this: Department){
     console.log("Department: ", this.name);
 }
}


const accounting = new Department('Accounting');
accounting.describe() // Accounting

const accCopy = {describe: accounting.describe}; // pointer to the function

accCopy.describe(); // undefined
```

In the above function, we are passing **this** to the function but the function can still be called without any parameters and along with **this** we mention the type of class this should refer to, So in this case, Department.

So with that, TypeScript will make sure that whenever we work with this keyword it should refer to the object with all same fields as **Department class**.

---

#### Private and Public Access Modifiers

Values of members of any object can be 
```javascript
class Department {
 name: string = 'Department Default Name';
 employees: string[] = [];

 constructor(){
   this.name = n;
 }

 addEmployee(n: string){
     this.employees.push(n);
 }
}

const department = new Department('Adarsh');
department[1] = 'Max'; // Direct access shouldn't be allowed as data isn't validated/sanitized.

```

Direct access to class members shouldn't be allowed as data isn't validated/sanitized.

We can mark properties and methods private by 

```javascript
class Department {
 name: string = 'Department Default Name';
 private employees: string[] = []; // Field Private Now

 constructor(){
   this.name = n;
 }

 addEmployee(n: string){
     this.employees.push(n);
 }
}

const department = new Department('Adarsh');
department[1] = 'Max'; // Direct access shouldn't be allowed as data isn't validated/sanitized.

```

**Private fields can only be modified and accessed by the class methods only.**

**Short Hand Initialization**

Instead of declaring the members in the class body and again in the constructor, we can simply declare them in the constructor parameter like:

```javascript
class Department{
    constructor(private name: String, public departmentId: number){
        // name and departmentId property created on this class.

    }
}
```

So, for every parameter of constructor function which starts with access modifier, property of the same name is created for the class.


**(TS Only) Read-Only Property**

Along with Private and Public properties, there can be readonly properties which once initialized with the object, cannot be changed.

```javascript
class Department{
    private readonly name: string,
    public readonly departmentId: number
    
    // OR IN THE CONSTRUCTOR
    constructor(private readonly variableName: type){
        
    }
}
```

In JavaScript, there are 3 kind of access modifiers:

**public:** when you want to access the members everywhere in your application.  
**private:** when you want to access the members only inside the class.  
**protected:** when you want to access the members inside the class and its subclasses.

---

### Getter and Setter
We can create getters and setters to access private/protected members outside the class through these functions.

These functions are declared with the following syntax:

```javascript
class SomeClass {
get anyNameRelatedToProperty(){
    // return the value publicly

    if(this.property){
        return this.property
    }

    throw new Error("Property Not found");
}

set SomePropertyName(value: String){
    this.addReport(value);
}

addReport(value){
    this.property = value;
}

}

const obj = new SomeClass();
obj.anyNameRelatedToProperty // Don't execute ()
obj.SomePropertyName = "NEW VALUE"; // Still don't execute ()
```

These methods are accessed like normal members and not executed with () like a method.


### Abstract Classes 

An abstract class is mostly used to provide a base for subclasses to extend and implement the abstract methods and override or use the implemented methods in abstract class.

We use `abstract` keyword to declare such classes, methods or member variables.

```javascript
abstract class Department {
    abstract Employees: string[] = [];
    static fiscalYear = 2023;

    constructor(){}

    abstract printDepartment(this: Department): void
}

class ITDept extends Department {
    // Implement all the abstract methods or variables here
}
```

---

### Singleton Private Constructor 
Singleton means that we can only have one Instance of the class. To Enforce this we make use of private constructor.

We achieve this by making the constructor private so it can't be called with new keyword and making a static method and static Instance variable on the class to check if a Instance is present return it if not create one and return it.

```javascript

class NewITDepartment {
    private static myInstance: NewITDepartment;

    private constructor(id: string){
        this.id = id;
    }

    static getInstance(){
        // this.myInstance === NewITDepartment.myInstance
        if(this.myInstance){
            return this.myInstance;
        }
        this.myInstance = new NewITDepartment('d2');
        return this.myInstance
    }
}

const IT = NewITDepartment.getInstance(); // Singleton
```

## (TS Only) Interfaces 

We can use Interface to describe the structure of an object or function, we create intefaces with `interface` keyword. It is used for typechecking objects.

```javascript
interface Person{
    name: string;
    age: number;

    // method(arguement: type): returnType
    greet(phrase: string): void; 
}

let user1: Person;

user1 = { // must have same fields as Person
 name: 'Adarsh',
 age: 24,
 greet(phrase: string) {
     console.log(phrase, this.name);
 }
}; 
    
```

Similar thing can be achived with `type` like

```javascript
type Person = {
    name: string;
    age: number;

    // method(arguement: type): returnType
    greet(phrase: string): void; 
}
```

So, why we have interface then?

They are not exactly the same, there are some differences 

1. Interface can only be used to describe structure of object.
2.  An Interface can be implement in a class 

```javascript
interface Greetable {
    name: string;
    // Readonly possible in Interfaces
    readonly id: number; 
    // Optional: Add ? after the variablename
    outputName?: string;
    greet(phrase: string): void;
}

// Interface can be extended using extend keyword, interface i2 extends i1
// Multiple interfaces can also be implemented. class name implements i1,i2
class Person implements Greetable {
    // Must implement all fields of Greetable
    name: string;
    id: number;
    constructor(n: string, id: num){
        this.name = n;
        this.id = num
    }

    greet(phrase: string){
        console.log(phrase, this.name);
    }
}
```

So Interfaces can be shared among files so all classes share the same structure.

It's similar to abstract classes but it has no implementation details at all where in abstract classes we have can have implementation details. (Function overriding etc)

```javascript
let user1: Greetable;
user1 = new Person("Adarsh"); // Possible bcuz Person Implements Greetable
```

Interface for function structure 
```javascript
interface AddFn {
    // Function without name
    (a: number,b: number): number;
}

let add: AddFn;
add = (a: number,b: number) => {} // do something
```

**Add ? after property name to make it optional in interfaces or classes**

```javascript
class Person{
    //Optional
    name?: string;
    constructor(n?: string){
        if(n){
        this.name = n;
        }
    }
}

const person = new Person(); // No parameter cuz optional
```
