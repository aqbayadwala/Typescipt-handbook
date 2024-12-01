# Everyday Types

- In this chapter, we'll cover some of the most common types of values you'll find in JavaScript code and explain the corresponding ways to describe it in TypeScript.
- Types can also appear in more places than just type annotations.
- We will also learn about places where we can refer to these types to form new constructs.
- We will start by reviewing the most basic and common types you might encounter when writing JavaScript or TypeScript code. These will later form the core building blocks of more complex types.

## The primitives: `string`, `number` and `boolean`

- JavaScript has three very commonly used primitives: `string`, `number` and `boolean`.
- Each has a corresponding type in TypeScript. (they have the same name as in js)
	- `string` - values like `"hello world"`
	- `number` - values like `42`. JS doesn't have special runtime value for integers - meaning there is no `int` or `float`. Everything is simply `number`.
	- `boolean` - values like `true` or `false`
	
> The type names `String`, `Number` and `Boolean` (starting with capital letters) are legal, but refer to some special built-in types that will very rarely appear in your code. Always use `string`, `number` and `boolean`.


## Arrays
- To specify the type of an array like [1, 2, 3], you can use the syntax `number[]`.
- This syntax works for any type (e.g. `string[]` is an array for strings and so on).
- `Array<number>` - this syntax is also available but will be covered in the _generics_ topic.

> Note that `[number]` is a different thing; refer to the section of [tuples](https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types).


## `any`

- TypeScript also has a special type, `any`, that you can use whenever you don't want to assign a type.
- When a value is of type `any`, you can access any properties of it (which will be in turn be of type `any`).
	- call it like a function, assign it to or from a value of any type, or pretty much anything else that is syntactically legal.
	- it simply means do anything with it until its syntactically correct.
	- Basically, it falls back to JavaScript's default behavior

```ts
let obj: any = {x: 0};
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed
// you know the environment better than TypeScript
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

> **Info**:
> An interesting thing here, once a variable is assigned type `any`, typescript disables type-checking for any instruction which involves that variable. Therefore in the above code, this last line works where a `string` is assigned to a type `number`. 
```ts
const n: number = obj;
```

- The `any` type is useful when you don't want to write out a long type just to convince TypeScript that a particular line of code is okay.

### `noImplicitAny`

- When you don't specify a type and TypeScript can't infer it from context, the compiler will typically default to `any`.
- You usually want to avoid this though, because `any` isn't type-checked.
- Use the compiler flag `noImplicitAny` to flag any implicit `any` as an error.

## Type Annotations on Variables

- When you declare a variable using `const`, `var` or `let`, you can optionally add a type annotation to explicitly specify the type of the variable:

```ts
let myName: string = "Alice";
```

> TypeScript doesn't use "types on the left"-style declarations like `int x = 0`; Type annotations will always go _after_ the thing being typed.

- In most cases, a variable is not needed to be typed.
- Wherever possible, TypeScript will try to automatically infer the types in your code.
- For example, the type of a variable is inferred based on the type of its initializer (the value by which the variable is initialized, simply meaning the value given to the variable):

```ts
// No type annotation needed -- 'myName' inferred as type 'string'
let myName = "Alice";
```

- For most part, you don't need to explicitly learn how TypeScript infers types.
- If you're starting out, try using fewer types and you will be surprised how few you need for TypeScript to fully understand what's going on.

## Functions

- Functions are the primary means of passing data around in JavaScript.

> I tried to understand this statement intensely. 
> What it means is, JS is a language which is built in a way that all of its tasks are accomplished by functions.
> Other languages like Python, C, C++ have functions, but they also have Classes and other programming paradigms.


- 