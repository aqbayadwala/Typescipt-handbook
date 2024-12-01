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
> 	The type names `String`, `Number` and `Boolean` (starting with capital letters) are legal, but refer to some special built-in types that will very rarely appear in your code. Always use `string`, `number` and `boolean`