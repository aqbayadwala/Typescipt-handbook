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

> [!info]
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

> [!info] 
> I tried to understand this statement intensely. What it means is, JS is a language which is built in a way that all of its tasks are accomplished by functions. Other languages like Python, C, C++ have functions, but they also have Classes and other programming paradigms.


- TypeScript allows you to specify the types of both the input and the output values of functions.
### Parameter Type Annotations

- When you declare a function, you can add type annotations after each paramter to declare what types of parameters the function accepts.
- Parameter type annotations go after the parameter name.

```ts
// Parameter type annotation
function greet(name: string){
	console.log("Hello, " + name.toUpperCase() + "!!");
}
```

> [!Info]- My Thoughts on TypeScript vs. C or any low level languages
> - TypeScript adds type annotations **on top of JavaScript**, which feels like **backwards work** since JavaScript was originally designed without type-checking.  
> - In **C**, type specification is **mandatory** from the start, ensuring type safety upfront, which I feel is a **cleaner and more structured** approach to programming.  
> - TypeScript feels like it’s **patching a gap** in JavaScript rather than being a foundational solution.  
> - **Modern languages** with more abstraction seem aimed at **making coding easier for newcomers**, but this sometimes **removes the depth** that helps programmers **understand core concepts** like memory management and type systems.

- When a parameter has type annotation, arguments to that function will be checked:

```ts
// Would be a runtime error if executed!
greet(42);

> Argument of type 'number' is not assignable to parameter of type 'string'.
```

> Even if you don't have type annotations on your parameters, TypeScript will still check that you passed the right number of arguments.

### Return Type Annotations

- You can also add return type annotations.
- Return type annotations appear after the parameter list:

```ts
function getFavouriteNumber(): number {
	return 26;
}
```

- Much like variable type annotations, you usually don't need a return type annotation because TypeScript will infer type based on its `return` statements.
- Some codebases will explicitly specify a return value for documentation purposes, to prevent accidental changes or just for personal preference.

### Functions which return Promises

>[!info]- Promise and pizza shop analogy
>- Imagine the guy on the counter taking orders as JavaScript.
>- Every pizza order is a synchronous instruction in an asynchronous function.
>- When you place an order to the guy (JavaScript), it passes that order to the kitchen and continues to take next orders.
>- There is only one person taking orders because JavaScript is single threaded.
>- The kitchen staff provides an acknowledgement receipt with a status of the order i.e. order placed, preparing food or out for delivery.
>- This acknowledgment receipt is a Promise.
>- A promise has three statuses: pending, resolved and rejected.
>- The kitchen staff is the browser API's to which JavaScript delegates tasks.
>- When an order is complete (i.e. the fetch call is done), the `.then` calls are executed.
>- We can imagine the `.then` calls as either dine in, takeaway or home delivery etc.
>- The `await` keyword is just syntactic sugar.
>- It means whatever instructions comes after await are to be completed only if the promise is resolved or rejected.

- If you want to annotate the return type of a function that returns a promise, you should use the `Promise` type:

```ts
async function getFavouriteNumber(): Promise<number> {
	return 26;
}
```

>[!info]- `async` functions always return a `promise`
>- Every `async` function returns a promise.
>- Even though if it's returning a plain value, it wraps that value in a promise.
>- Basically, it gives a ready pizza but with a receipt.

### Anonymous function

- Anonymous functions are a little bit different from function declarations (normal functions).
- When a function appears in a place where TypeScript can determine how it's going to be called (what the function is used for), the parameters of the function are automatically given types.

```ts
const names = ["Alice", "Bob", "Eve"];

// Contextual typing for function - parameter s inferred to have type string
names.forEach(function (s) {
	console.log(s.toUpperCase());
});
// TypeScript knows that s is a string because names is an array of strings

// Contextual typing also applies to arrow functions
names.forEach((s) => {
	console.log(s.toUpperCase());
});
```

- This is called contextual typing.
- Similar to inference rules, you don't need to learn how this happens, but understanding that it does can help you notice when type annotations are not needed.
- Later we'll see more examples how the context that a value occurs in can affect type.

## Object types

- Apart from primitives, the most common sort of type you'll encounter is an object type.
- This refers to any JavaScript value with properties, which is almost all of them.

> [!info]- Apart from primitives, every thing is an object in JavaScript
> 
> The primitives are `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, and `bigint`.
> 
> Here are some examples of data types in JavaScript (apart from primitives) that have properties:
> 
> 1. **Object**  
>    Example:  
>    ```js
>    const person = {
>      name: "Alice",
>      age: 25
>    };
>    // person has properties: name and age
>    ```
> 
> 2. **Array**  
>    Arrays are objects, and they have properties like `length` and methods like `.push()` or `.pop()`.  
>    Example:  
>    ```js
>    const fruits = ["apple", "banana"];
>    // fruits has a property 'length' and array methods like 'push'
>    ```
> 
> 3. **Function**  
>    Functions in JavaScript are also objects, and they have properties such as `name`, `length`, or custom properties you add.  
>    Example:  
>    ```js
>    function greet() {}
>    greet.description = "This is a greeting function.";
>    // greet has a property 'description'
>    ```
> 
> 4. **Date**  
>    The `Date` object has properties and methods related to date and time.  
>    Example:  
>    ```js
>    const today = new Date();
>    console.log(today.getFullYear()); // 'today' has methods like getFullYear()
>    ```
> 
> 5. **RegExp**  
>    Regular expressions in JavaScript are objects with properties like `lastIndex` and methods like `.test()` and `.exec()`.  
>    Example:  
>    ```js
>    const pattern = /hello/;
>    // pattern has a property 'lastIndex'
>    ```
> 
> 6. **Error**  
>    The `Error` object has properties like `message` and `name`.  
>    Example:  
>    ```js
>    const error = new Error("Something went wrong");
>    console.log(error.message); // 'error' has the property 'message'
>    ```
> 
> These types are all considered objects, so they have properties and methods that can be accessed and manipulated.

- To define an object type, we simply list its properties and their types.
- For example, here's a function that takes a point-like object.

```ts
// The parameter's type annotation is an object type
function printCoord(pt: {x: number; y:number}){
	console.log("The coordinate's x value is " + pt.x);
	console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 3, y: 7 });
```

- Here, we annotated the parameter with a type with two properties - `x` and `y` - which are both of type `number`.
- You can use `,` or `;` to separate the properties and the last operator is optional either way.
- The type part of each property is also optional. If you don't specify a type, it will be assumed to `any`.

#### Optional Properties

- Object types can also specify that some or all of their properties are optional.
- To do this, add a `?` after the property name:

```ts
function printName(obj: {first: string, last?: string}){
	// ...
}
// Both OK
printName({first: "Bob"});
printName({first: "Alice", last: "Alisson"});
```

- In JavaScript, if you access a property that doesn't exist, you'll get a value `undefined` rather than a runtime error.
- Because of this, when you read from an optional property, you'll have to check for `undefined` before using it.

```ts
function printName(obj: {first: string, last?: string}){
	// Error - might crash if 'obj.last' wasn't provided
	console.log(obj.last.toUpperCase());

> 'obj.last' is probably undefined

	if (obj.last !== undefined){
		// OK
		console.log(obj.last.toUpperCase());
	}

	// A safe alternative using modern JavaScript syntax:
	console.log(obj.last?.toUpperCase());
}
```

## Union Types

- TypeScript's type system allows you to build new types out of existing ones using a large variety of operators.
- Now that we know how to write a few types, it's time to start combining them in interesting ways.

#### Defining a Union Type

- The first way to combine types you might see is union types.
- A union type is a type formed from two or more types, representing values that may be any one of those types.
- We refer each of these types as the union's members.
- Let us write a function that can operate on strings and numbers.

```ts
function printId(id: number | string){
	console.log("Your ID is:" + id);
}
// OK
printId(101);
// OK
printId("202");
// Error
printId({myId: 22342});
> Argument of type '{myID: number}' is not assignable to parameter of type 'string | number'.
```

> The separator of the union members is allowed before the first element, so you could also write this:
> 
> ```ts
> function printTextOrNumberOrBool(
>   textOrNumberOrBool:
>     | string
>     | number
>     | boolean
> ) {
>   console.log(textOrNumberOrBool);
> }
> ```



#### Working with Union Types

- It's easy to provide a value matching a union type - simply provide a type matching any of the union's members.
- If you have a value of a union type, how do you work with it?
- TypeScript will only allow operations valid for every member of the union.
- For example, if you have the union `string | number`, you can't use methods that are only available on `string`:

```ts
function printId(id: number | string){
	console.log(id.toUpperCase());
}
> Property 'toUpperCase' does not exist on type 'string | number'.
> 	Property 'toUpperCase' does not exist on type 'number'.
```

- The solution is to _narrow_ the union with code, the same as you would do in JavaScript without type annotations.
- _Narrowing_ occurs when TypeScript deduce a more specific type for a value based on the structure of the code.

> [!info]
> ###### Why do I need narrowing or any other means so that TypeScript can figure out types?
> - If I help TypeScript better understand types, it helps me in turn to identify errors in my code.

- For example, TypeScript knows only a `string` value can have `typeof` value `string`.

```ts
function printId(id: number | string){
	if (typeof id === 'string'){
		// In this branch, id is of type 'string'
		console.log(id.toUpperCase());
	} else {
		// Here, id is type 'number'
		console.log(id);
	}
}
```

- Another example is to use a function `Array.isArray`:

```ts
function welcomePeople(x: string[] | string){
	if (Array.isArray(x)){
		// Here 'x' is an 'string[]'
		console.log("Hello, " +  x.join(" and "));
	} else {
		// Here 'x' is a 'string'
		console.log("Welcome lone traveler " + x);
	}
}
```

- Notice that in the `else` branch, we don't need to do anything special - if `x` wasn't a `string[]`, then it must have been a `string`.
<br>
- Sometimes you'll have a union where all the members have something in common.
- For example, both arrays and strings have a `slice` method.
- If every member in a union has a property in common, you can use that property without narrowing:

```ts
// Return type is inferred as number[] | string
function getFirstThree(x: number[] | string){
	return x.slice(0,3);
}
```


