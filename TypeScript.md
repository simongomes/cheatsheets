# TypeScript Basics

TypeScript is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale. It adds static typing for JavaScript.

## Primitive or Basic Types

### Boolean

```ts
let completed: boolean = false;
```

### String

```ts
let name: string = "Sally";
```

### Array

```ts
let ages: number[] = [21, 30, 70];

// equivalent but less common in real-world apps
let ages: Array<number> = [21, 30, 70];
```

### Enum

```ts
enum Gender {
  Male,
  Female,
  Unspecified,
}

let gender: Gender = Gender.Female;

// By default value starts from 0, we can override the enum values also
enum Gender {
  Male = 1,
  Female = 2,
  Unspecified = 3,
}
```

### Any

_For interacting with JavaScript code or situations where the type is simply unknown and you want to avoid compile-time errors._

```ts
let something: any = 1;
something = "a string"; // assigning string
something = false; // assigning boolean

// all work because type is "any"
```

### Use of `any` for arguments

```ts
function handleData(age: any) {
  console.log(age); // age could be anything!
}
```

### Use of `any` for return values

```ts
function handleData(age: any): any {
  if (age !== undefined) {
    let message = `you are ${age} years old`;
    return message;
  }
  return false;
  // we know it's a string but we could return anything
}
```

_**Pro Tip:** Try to avoid over-use of any as it somewhat defeats the point (and benefits) of using Typescript in the first place!_

### Void

_Mostly used in function declarations, to indicate that the function does not return a value._

```ts
function log(message: string): void {
  console.log(message);
}
```

### Object

_Anything which isn't a primitive (isn't a number, boolean, string etc.);_

```ts
function callApi() {
  let thing: object = backend.get("/api/users");
  thing = 42; // error: can't assign a number
}

thing = {
  name: "bob",
}; // works, we've assigned an object
```

### Template literals (aka Template strings)

_A handy way to concatenate strings without the usual ugly syntax._

```ts
// without Template strings
let greeting = "Hello " + name + ", how are you today?";

// with Template strings
let greeting = `Hello ${name}, how are you today?`;
```

_The difference is that subtle backtick used instead of an apostrophe._

### Interfaces

```ts
interface IUser {
  firstName: string;
  lastName: string;
}
```

Interfaces make it easy to declare the "shape" of an object.

Objects defined using the interface will trigger compile errors if you a) don't specify values for required f ields or b) try to assign a value to a field which isn't defined.

```ts
// doesn't compile because firstName, LastName missing
let me: IUser = {};

// works

let me: IUser = { firstName: "Gambit", lastName: "Hilton" };

// doesn't compile because IUser has no an 'age' property
me.age = 20;
```

### Required vs optional properties

```ts
interface IUser {
  firstName: string;
  lastName: string;
  age?: number;
}
```

_Here `age` is optional. Everything else is required._

### Union Types

Sometimes, an object can be one of several types.

You can let the compiler know that multiple types are valid by declaring a union type.

```ts
let ambiguous: string | number;
ambiguous = "Not any more";
ambiguous = 21;
```

### Functions

For the most part functions in TypeScript work just as functions in Javascript do with the obvious difference being that you can specify argument and return types

Here's a named function with it's argument and return types specified.

```ts
function subtract(x: number, y: number): number {
  return x - y;
}
```

And here's an anonymous equivalent.

```ts
let subtract = function (x: number, y: number): number {
  return x - y;
};
```

_Note that we haven't explicitly defined the type of subtract here. The Typescript compiler has inferred it from the type of our function._

### Optional arguments

**With default values**

```ts
let add = function (x: number, y: number, z: number = 0): number {
  return x + y + z;
};

add(1, 2); // 3
add(10, 10, 10); //30
```

_If you specify a default value for an argument then don't pass in a value for it when you call the function, the default will be used._

**Without default values**

```ts
let greet = function (firstName: string, lastName?: string): string {
  if (lastName) {
    return firstName + " " + lastName;
  } else {
    return firstName;
  }
};
```

_Here no default will be used, if you don't pass the optional parameter in, the function needs to check whether it has a value and act accordingly._

### Rest Parameters

```ts
function greetTheRoom(...names: string[]) {
  console.log("hello " + names.join(", "));
}
// 'hello Jon, Nick, Jane, Sam'
greetTheRoom("Jon", "Nick", "Jane", "Sam");
```

_When you call a function with a Rest parameter, you can specify as many values as you like (or none)._

### Classes

Javascript is very much a functional language but in recent times has introduced the concept of "classes" as a way to structure your code and objects.

Typescript supports this too...

```ts
class Calculator {
  private result: number;
  constructor() {
    this.result = 0;
  }
  add(x: number) {
    this.result += x;
  }
  subtract(x: number) {
    this.result -= x;
  }
  getResult(): number {
    return this.result;
  }
}

// use it
let calculator = new Calculator();
calculator.add(10);
calculator.subtract(9);
console.log(calculator.getResult()); // 1
```

## Modifiers

Everything in a Typescript class defaults to public.

### Private modifier

You can mark class members as private and this will prevent it being accessed outside of the class.

```ts
class Person {
  private passportNumber: string;
  constructor(passportNumber: string) {
    this.passportNumber = passportNumber;
  }
}

let person = new Person("13252323223");

// error: passportNumber is private
console.log(person.passportNumber);
```

### Readonly modifier

Readonly members must be initialized when they're declared, or in the constructor

```ts
class Person {
  private readonly passportNumber: string;

  constructor(passportNumber: string) {
    this.passportNumber = passportNumber;
  }

  changePassport(newNumber: number) {
    // error: cannot assign to passportNumber because readonly
    this.passportNumber = newNumber;
  }
}
```

### Parameter properties

If you find yourself declaring fields, passing values in via constructors and assigning those values to the f ields, you can use a handy shorthand to save writing so much boilerplate code.

```ts
class Person {
  constructor(private readonly passportNumber: string) {}
}
```

This is exactly the same as..

```ts
class Person {
  private readonly passportNumber: string;

  constructor(passportNumber: string) {
    this.passportNumber = passportNumber;
  }
}
```

_When we add accessibility modifiers ( private , public , readonly etc.) to our constructor parameters, Typescript automatically declares a class member and assigns the parameters' value to it._
