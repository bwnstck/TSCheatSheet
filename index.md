# Typescript in vanilla JS

#### Ressources:

[TypeScript for JS Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

---

## Types

## [Interface](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#defining-types)

```TS
interface User {
  name: string;
  id: number;
}

const user: User = {
  name: "Hayes",
  id: 0,
};
```

- _If you provide an object that doesn’t match the interface you have provided, TypeScript will warn you_
- _You can use interfaces to annotate parameters and return values to functions:_

```TS
function getAdminUser(): User {
  //...
}

function deleteUser(user: User) {
  // ...
}
```

### [Primitive Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)

**JS**

- boolean
- bigint
- null
- number
- string
- symbol
- object
- undefined

TypeScript extends this list with a few more, such as:

**TS**

- any (_allow anything_)
- unknown (_ensure someone using this type declares what the type is_)
- never (_it’s not possible that this type could happen_)
- void (_a function which returns undefined or has no return value_)

> You’ll see that there are two syntaxes for building types: **Interfaces** and **Types**. You should **prefer interface**. Use type when you need specific features.

## How to find the right type?

> To learn the type of a variable, use typeof:

| Type      | Predicate                        |
| --------- | -------------------------------- |
| string    | typeof s === "string"            |
| number    | typeof n === "number"            |
| boolean   | typeof b === "boolean"           |
| undefined | typeof undefined === "undefined" |
| function  | typeof f === "function"          |
| array     | Array.isArray(a)                 |

> ### About Number, String, Boolean, Symbol and Object
>
> It can be tempting to think that the types Number, String, Boolean, Symbol, or Object are the same as the lowercase versions recommended above. These types do not refer to the language primitives however, and almost never should be used as a type.<br>
> Instead, use the types number, string, boolean, object and symbol.

```TS
function reverse(s: string): string {
  return s.split("").reverse().join("");
}

reverse("hello world");
```

---

### Array

```TS
let list: number[] = [1, 2, 3];

is the same like

let list: Array<number> = [1, 2, 3];
```

### Tuple

```TS
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
Type 'number' is not assignable to type 'string'.
Type 'string' is not assignable to type 'number'.
```

### Object

```TS
declare function create(o: object | null): void;

// OK
create({ prop: 0 });
create(null);

create(42);
Argument of type '42' is not assignable to parameter of type 'object | null'.
```

### Enum

> enum is a way of giving more friendly names to sets of numeric values.

```TS
enum Color {
  Red = 1,
  Green,
  Blue,
}
let a: Color = Color.Red; // $ "Red"
let b: Color = Color[2];  // $ "Green"
```

### Unknown

> We may need to describe the type of variables that we do not know when we are writing an application. These values may come from dynamic content – e.g. from the user – or we may want to intentionally accept all values in our API.

```TS
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

### Any

> In some situations, not all type information is available or its declaration would take an inappropriate amount of effort. These may occur for values from code that has been written without TypeScript or a 3rd party library. In these cases, we might want to opt-out of type checking. To do so, we label these values with the any type:

```TS
declare function getValue(key: string): any;
// OK, return value of 'getValue' is not checked
const str: string = getValue("myString");
```

> After all, remember that all the convenience of any comes at the cost of losing type safety. Type safety is one of the main motivations for using TypeScript and you should try to avoid using any when not necessary.

### Void

> void is a little like the opposite of any: the absence of having any type at all. You may commonly see this as the return type of functions that do not return a value:

```TS
function warnUser(): void {
  console.log("This is my warning message");
}
```

### Never

> The never type represents the type of values that never occur. For instance, never is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns.

> The never type is a subtype of, and assignable to, every type; however, no type is a subtype of, or assignable to, never (except never itself). Even any isn’t assignable to never.

```TS
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

### Type assertions

> Sometimes you’ll end up in a situation where you’ll know more about a value than TypeScript does. Usually, this will happen when you know the type of some entity could be more specific than its current type.

#### as Syntax

```TS
let someValue: unknown = "this is a string";

let strLength: number = (someValue as string).length;
```

#### angle-bracket Syntax

```TS
let someValue: unknown = "this is a string";

let strLength: number = (<string>someValue).length;
```

## [ Composing Types](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#composing)

#### [Unions](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#unions)

```TS
type MyBool = true | false;

or

type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

> Unions provide a way to handle different types too. For example, you may have a function that takes an array or a string:

```TS
function getLength(obj: string | string[]) {
  return obj.length;
}
```

### [Generics](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#generics)

> Generics provide variables to types. A common example is an array. An array without generics could contain anything. An array with generics can describe the values that the array contains.

```TS
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;

// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();

// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
Argument of type 'number' is not assignable to parameter of type 'string'.
```
