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

> **Info** An interesting thing here:
> Once a variable is assigned type `any`, typescript disables type-checking for any instruction which involves that variable. Therefore in the above code, the last line works
```ts
const n: number = obj;
```
