
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
---

# Advance Types

### Intersection Type

Intersection types allow us to combine other types.Use `&` to combine types.
```javascript

type Admin = {
    name: string,
    privileges: string[];
}

type Employee = {
    name: string;
    startDate: Date;
};

type ElevatedEmployee = Admin & Employee; 
```

ElevatedEmployee is a new type that must have both properties of Admin and Employee.

```javascript
const e1: ElevatedEmployee = {
    name: 'Max',
    privileges: ['create-server'],
    startDate: new Date()
}
```

Same thing we could have achived with interfaces by:

1. Create Interface of Admin and Employee 
2. Implement both Interfaces for ElevatedEmployee OR we can create a 3rd interface which extends Admin,Employee and use that Instead.  

For Objects, Intersection will have all common fields and for Union types, the common type is used.

For Example:
```javascript
type Combinable = string | number; // String OR Number Union
type Numeric = number | boolean; // Number OR BOOL Union

type Universal = Combinable & Numeric; // number; cuz it's common.

```

--- 

### Discriminated Union Type 

It's a special kind of TypeGuard which helps us Implementing TypeGuards Easier.

We know these two typeguards 

1. Typeof Operator
2. Instance of Operator (Does not work with Interface)
3. Using *if (prop in obj)* check, to check if a property exists on object before accessing it.

We can set a custom property for each type and set it's value to a string literal so we can do actions based on the type property. 

```javascript
interface Bird {
    type: 'bird'; // literal type, exact value as type
    flyingSpeed: number; 
}

interface Horse {
    type: 'horse'; // literal type, exact value as type
    flyingSpeed: number; 
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal){
    switch(animal.type) {
        // animal.type is a string literal type.
    }
}
```
---

### Typecasting

Typecasting tells typescript is some value of something specific type where typescript is not able to infer it on it's own.

**For example:** Getting an element from DOM by the tagName, TypeScript is able to tell which element it is but when Element is selected by className or ID, TypeScript is atleast able to infer it as HTMLElement or NULL.

It does not which specific Element it is, so most of the fields would work fine but some elements like input, accessing input field can make typescript go crazy.

```javascript
const input = document.querySelector('#id')! as HTMLInputElement;
// OR
const input = <HTMLInputElement> document.querySelector('#id')!;

```

---

### Index Properties 

Index Properties allows us to create flexible objects where we aren't sure about how many or which properties we will have but we know their value type.

**Syntax:** Use square brackets to create the property name and it's type which can be of type String, Number or Symbol. Values such as boolean are not allowed and then colon : and the Type of value that the key will store.

```javascript
interface ErrorContainer {
 [key: valueTypeStringNumberSymbol]: string;
}
// ErrorBag can be a empty object as well.
const errorBag: ErrorContainer = { 
    email: "Must be a valid email."
}
```

**Restriction:** While creating a index property, all the properties (if there are any other than index type) must share the same valueType. 

Object containing string and numbers are not allowed if index properties are present.

Object of type containning Index properties can be a empty object and later we can add more properties.

---

### Function Overload

Function overload is written just above the the original function with the same name 

```javascript
type Combinable = string | number;

function add(a:number, b: number) : number; // Number, Number returns Number
function add(a:string, b: string) : string; // string,string returns string
function add(a: Combinable, b: Combinable) {
    // logic of adding based on type here
    return a+b;
}

const result1 = add(2,4); // number
const result2 = add('Max', 2) // string
```
---
## Generics
A Generic type is a type which is kind of connected to a other type and is flexable about what that other type is.

For example, an Object is a type of it's own but it can contain strings and numbers. Array is a type and can store strings, numbers or objects.

Generic in the similar way, Is a type which knows what other kind of data it will work with.

We use **<>** brackets to define type we will evantually end up with.

```javascript
const names: string[]; 
// the above line can also be written as
const names2: Array<string> []; 
```

Here, we said names is an array which will contain strings. So for any data that gets stored in that array of string, we are sure that we can always apply string methods on it's elements.

OR, If the Generic type is a Promise and we tell it in advance what kind of data it will evantually yield to, we can access it's respective properties and methods.

Here, are some basic Generic examples -
Array<string>
Promise <Unknown>
Promise <string>


```typescript
const promise: Promise<number> = new Promise((resolve, reject) => {
    resolve(24);
});

promise.then(data => {
 // data is a string 
 data.split(' '); // ERROR CUZ data is a number
});
```

#### Function Generics 

Let's say we have a function that merges two objects.

```typescript
function merge(a: object, b: object){
    return {...a,...b};
}

const merged = merge({name: 'Adarsh'},{age: 24});
merged.name; // TypeScript does not know about these properties
merged.age; // No support 
```

Here,TypeScript knows merge function will receive an object but it does not know the properties it'll contain hence when we try to access the properties on the returned result (merged) object, we get no typescript support.

This is because, object itself is kind of very basic information.

We can turn the same function into a Generic function by: 

```typescript
function merge<T,U>(a: T, b: U){
    return {...a,...b};
}

const merged = merge({name: 'Adarsh'},{age: 24});
merged.name; // TypeScript knows these properties
merged.age; // Full TypeScript support 

// We can also set type on a function call 
// But it's redundant because typescript infers it  based on the data anyway.
const result2 = merge<{name: string}>({name: 'Adarsh'}); // Generic on function call (REDUNDANT)
```
*Here, T and U are Identifiers and can be anything but single characters from T is a good naming convention.*

Here, we're telling TypeScript that merge function we set two Identifiers, **T** and **U** and in the function parameters we mentioned these identifiers as type for the function parameters.

We are working with two different object hence we used two identifiers.

If no return type is mentioned it will infer the return type based on the data returned as usual.

---

In short, we pass extra information to function so we can work better with the result even tho we don't know much about the data before-hand, that's one explaination of Generics.

**Constraints:**
With the following code we were able to merge two objects but we're basically saying merge whatever we get in the function because in some way the Type of T and U is set to any.

For Generic types we can set certain constraints regarding the Generic type is based on, we do this using the extends keyword.

```typescript
function merge<T extends object,U extends object>(a: T, b: U){
    return {...a,...b};
}

const merged = merge({name: 'Adarsh'},{age: 24});
merged.name; 
merged.age;  
```

With that change, we're saying that the type T and U has to be an object and nothing else.

We can extend any type whether it's string, number, union or even our own custom type.

```typescript
// String and array both have length property.
interface Lengthy {
    length: number;
}
function countAndDescribe<T extends length>(element: T) : [T, string]{

    // return type is tuple bcuz exactly two elements of specific type
    let descriptionText = 'Got no value';
    if(element.length > 0){
        descriptionText = `Got ${element.length} elements`;
    }
    return [element, descriptionText];
}

countAndDescribe("Hello"); // length property exists
countAndDescribe(['cooking', 'coding']); // length property exists
countAndDescribe(30); // Error no length property exists
```
In the above function, we don't care about the exact type, we only care about the fact that it has a length property, even if it's an array (behind the scenes array is just another object in javascript).

The Idea is to not being specific about type of data we will get, we just care it does have the property we're going to work with whether it's string or an array. We don't want to lock the type to a single type.

So it Instead of writing bunch of overloads or union types, generics are used.

---

### keyof constraint

Consider the following function:

```typescript
function getValue(obj: object, key: string){
    return obj[key];
}

getValue({}, 'name');
```

In the above function, we're trying to extract value from an object based the key that we passed to the function.

But, TypeScript does not gurantee that the key is present on the object hence it throws error.

To fix this, we can use another constraint known as **keyof**.

```typescript
function getValue<T extends object,U extends keyof T>(obj: T, key: U){
    return obj[key]; // No Error
}

getValue({}, 'name'); // Error name does not exists
getValue({name: 'Adarsh'}, 'name'); // Adarsh; OK
```

Now, the TypeScript can check if the key is present on the object and throw error otherwise.

---

### Generic Classes 

let's consider a class for data storage.

```typescript
class DataStorage {
    private data = [];

    addItem(item){
        this.data.push(item);
    }

    removeItem(item){
        this.data.splice(this.data.indexOf(item), 1);
    }

    getItems(){
        return [...this.data];
    }
}
```

the above class throws error about data being of type any.

but, we don't really care about the type of data we're gonna store whether it's number, string or anything else, for that we can turn this function into a generic function


```typescript
class DataStorage<T extends string | number | boolean> {
    private data: T[] = [];

    addItem(item: T){
        this.data.push(item);
    }

    removeItem(item: T){
        if(this.data.indexOf(item) === -1){
            // -1 == Not found, and -1 remove last object in splice
            return;
        }
        this.data.splice(this.data.indexOf(item), 1);
    }

    getItems(){ 
        // infered return type as T
        return [...this.data];
    }
}

const textStorage = new DataStorage<string>();
textStorage.addItem(19); // Error 
textStorage.addItem('Adarsh'); // OK
textStorage.getItems(); // OK

const numberStorage = new DataStorage<number>();
numberStorage.addItem('Adarsh'); // Error
numberStorage.addItem(23); // OK

const combinedStorage = new DataStorage<number | string>();
combinedStorage.addItem('Adarsh'); // OK
combinedStorage.addItem(23); // OK
```

So now by using Generic, we giving extra information about the data being stored but still being flexible about it and get full support of TypeScript.

The above code only work with primitive types hence setting constraints is a good idea.

--- 

### Generic Utility Types 

TypeScript ships with some builtin Utility types that helps and only exists in the development phase before compilation.

```typescript
interface CourseGoal {
    title: string;
    description: string;
    completeUntil: Date;
}

function createCourseGoal(title: string, description: string, date: Date): CourseGoal{
    // Now returning like this is fine
    return {
     title: title,
     description: description,
     completeUntil: date
    }

}
```

Returning an object with all the mandatory field works but what if for some reason we don't have all the fields, like for example fetching from API or doing validation of data for each field.

then the code would look like something like this

```typescript
function createCourseGoal(title: string, description: string, date: Date): CourseGoal{
    // Now returning like this is fine
    let courseGoal = {};
    // the below lines throws error because property does not exists on the object
    courseGoal.title = title; // Error 
    courseGoal.description = description; // Error
    courseGoal.completeUntil = date; // Error
    
    return courseGoal;
}
```

We see error, because TypeScript does not like modifying an unknown properties of object, so if we set type of object to `CourseGoal` then 
```javascript
  let courseGoal: CourseGoal = {}; // Error
```

because courseGoal is of type CourseGoal and cannot be an empty object. 

So to overcome this, we can some a Generic Utility called **Partial**.

```typescript
function createCourseGoal(title: string, description: string, date: Date): CourseGoal{
    // Now returning like this is fine
    let courseGoal: Partial<CourseGoal> = {};
    // the below lines throws error because property does not exists on the object
    courseGoal.title = title; // Error 
    courseGoal.description = description; // Error
    courseGoal.completeUntil = date; // Error
    
    return courseGoal as CourseGoal; // bcuz returnType Partial != CourseCoal
}
```

So by using Generic type, we are telling TypeScript that courseGoal will evantually hold a CourseGoal.

#### Readonly Utility Type 
Readonly type as the name suggests marks the type as readonly and any attempt to modify or delete it will result in an error. It works for objects as well.

For Example:
```typescript
const names: Readonly<string[]> = ['Adarsh', 'Max'];
names.push('OK'); // Error
names.pop(); // Error

```
---




