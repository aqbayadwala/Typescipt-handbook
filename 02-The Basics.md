# The Basics
Each and every value in js has a set of behaviors you can observe from running different operations

```ts
// Accessing the property 'toLowerCase'
// on 'message' and then calling it
message.toLowerCase();

//calling message
message()
```

- Two things are happening
	- Calling `toLowerCase()` property on message
	- Calling `message()` itself
- Now assuming we don't know the value of message, we can't say what this code will return, here are the problems
	- Is `message()` callable?
	- Does `message()` have a property `toLowerCase()` on it?
	- if it does, is  `toLowerCase()` even callable?
	- If both of these values are callable, what do they return?
- The answer to these questions are usually things we keep in our heads when we write JavaScript, and we hope we got all the details right?

Let's say `message` was defined in the following way

```ts
const message = 'Hello world!;'
```

- Here, `message.toLowerCase()` will return same string in lowercase
- But, the `message()` call will not work and will return

> TypeError: message is not a function

It'd be great if we could avoid mistakes like this

Now here two concepts in this context
- What is a type?
	- A  *type* is the concept of describing which values can be passed and which will crash.
- What is dynamic typing?
	- Running the code and checking what happens

## Static type-checking

- Most people don't like errors when running their code.
- If we add just a bit of code, save the file and re-run, and get an error and debug it, that is easy and the problem can be fixed quickly.
- But we might not have tested the feature thoroughly enough, so we might never actually come across the error
- Or if we are lucky enough to see the error, we might end up doing a lot of refactoring and adding a lot of different code to fix the problem for the moment.
- If we have a tool that helps us find these bugs before our code runs, it would be so good.
- That's what a static type checker is and does.
- Static type systems describe the shapes and behaviors of what our values will be when we run our programs.
- A type-checker like TypeScript uses that information (shape and behavior) and tells us when things might go off the rails.

```ts
const message = "hello!";

message();

> This expression is not callable.
> Type 'String' has no call signatures
```

Running this code with TypeScript will give us an error message before we run the JavaScript code in the first place.

## Non-exception failures

- So far, we have discussed runtime errors - meaning js's runtime telling us when it thinks something is nonsensical.
- ECMAScript specification defines what is an error and what is not an error in JavaScript.
- In ECMAScript specs
	- calling something which isn't callable throws an error.
	- but calling a property which doesn't exist returns `undefined`.
	
- JavaScript
	
```javascript
const user = {
	name: "Daniel",
	age: 26,
};

user.location; // returns undefined in JavaScript
```

- TypeScript

```ts
const user = {
	name: "Daniel",
	age: 26,
};
user.location;

> Property 'location' does not exist on type '{ name: string; age: number;}'
```

- While this is a trade-off in what you can express, the intent is to catch legitimate bugs in our programs.
- For example: typos,

```ts
const announcement = "Hello World!"

// How quickly can you spot the typos?
announcement.toLocaleLowercase()
announcement.toLocalLowerCase()

// We probably meant to write this...
announcement.toLocaleLowerCase()
```

uncalled functions,

```ts
function flipCoin(){
	// Meant to be Math.random()
	return Math.random < 0.5;
> Operator '<' cannot be applied to types '() => number' and 'number'. 
}
```

or basic logic errors,

```ts
const value  = Math.random() < 0.5 ? "a" : "b";
if (value != "a"){
	// ...
} else if (value === "b"){
> The comparison appears to be unintentional because the types '"a"' and '"b"' have no overlap
	// Oops, unreachable
}
```

### Types for tooling

- Typescript can catch bugs when we make mistakes but it can also prevent us from making those mistakes in the first place.
- This topic is talking about **autocomplete** or **IntelliSense**
### `tsc`, the TypeScript compiler

- Command to install `tsc`
```bash
npm tsc -g typescript
```
> This installs the TypeScript compiler `tsc` globally.
> You can use `npx` or similar tools if you'd prefer to run `tsc` from a local `node_modules` package instead

- It teaches to write our first TypeScript program: `hello.ts`:
```ts
# hello.ts
// Greets hello world.
console.log("Hello world!");
```
- Now the command to run it
```bash
tsc hello.ts
```
- What just happened? Nothing showed on the console.
- Well, there were no type errors, so we din't get any output on our console since there was nothing to report.
- But check again - we got some file output instead.
- If we look in our current directory, we'll see a `hello.js` file next to `hello.ts`.
- That's the output from our `hello.ts` file after the `tsc` *compiles* or *transforms* it into a plain JavaScript file.
- And if we check the contents of `hello.js`, we'll see what TypeScript spits out after it processes a `.ts` file:
```js
// Greets hello world.
console.log("Hello world!");
```
- In this case, there was very little for TypeScript to transform, so it looks identical to what we wrote in the `hello.ts` file.
- The compiler tries to emit clean readable code that looks like something a person would write.
- Even for complex JavaScript code, and with all the advanced TypeScript features (like types, interfaces and decorators), converting them to **simple**, **clean** JavaScript can be tricky. Despite these challenges, TypeScript strives to:
	1. Indent consistently: Ensure that the generated code is neatly formatted.
	2. Handle multi-line code gracefully.
	3. Preserve comments.
	4. In summary, TypeScript tries to make the compiled output look as if a person wrote it.
- Let's see code with type-checking error:
```ts
// This is an industrial-grade general-purpose greeter function
function greet(person, date){
	console.log(`Hello ${person}, today is ${date}`);
}
greet("Brenden");
```
- If we run `tsc hello.ts` again, notice that we get an error on the command line.

> Expected 2 arguments, but got 1
- TypeScript is telling us we forgot to pass an argument to the greet function
- Even though we wrote plain JavaScript, type-checking was still able to find problem with our code
#### Emitting with Errors

- One interesting thing here is, though `tsc` reported an error, it still created a `hello.js` file
- This is because, it prioritizes not blocking the development process rather than stopping the execution when error is found.
- This highlights that TypeScript compiles to JavaScript even if there are type errors, focusing on producing valid JavaScript rather than stopping at every type-checking issue.
- Now, earlier we said, type-checking code limits the sorts of programs you can run, and so there's a trade-off on what sorts of things a type-checker finds acceptable.
- Most of the time, that's okay.
- For example, imagine yourself migrating JavaScript code over to TypeScript and introducing type-checking errors.
- Eventually you'll get around to cleaning things up for the type-checker, but the original JavaScript code was already working!
- Why should converting it over to TypeScript stop you from running it?
- So TypeScript doesn't get in your way.
- But if you want to make TypeScript act a bit more strictly.
- In that case, you can use `noEmitOnError` compiler option.
- Try running `hello.ts` file with that flag:

```shell
tsc --noEmitOnError hello.ts
```

- You'll notice that `hello.js` never gets updated.

> **Note**
> Emit means creating a `.js` file. When we use `--noEmitOnError` , it does not create a new `.js`

#### Explicit Types

- Up until now, we haven't told TypeScript in what `person` or `date` are.
- Edit the code to tell TypeScript that `person` is a `string`, and a `date` of type `Date` object.
- We'll also use `toDateString()` method on date.

```ts
// This is an industrial-grade general-purpose greeter function
function greet(person, date){
	console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
```

- What we did was add _type annotations_ on `person` and `date` to describe what types of values `greet` can called with.
- You can read that signature as "`greet` takes a `person` of type `string` and a date of type `Date`".
- With this, TypeScript can tell us about other cases where `greet` might have been called incorrectly. For example:

```ts
// This is an industrial-grade general-purpose greeter function
function greet(person, date){
	console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
greet("Maddison", Date());
> Argument of type 'string' is not assignable to parameter of type 'Date'
```

- Why did TypeScript reported an error on our second argument?
- Calling `Date()` in JavaScript returns a `string`. On the other hand, constructing a `Date` with `new Date()` actually gives us what we were expecting.
- Let's change the code:

```ts
// This is an industrial-grade general-purpose greeter function
function greet(person, date){
	console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
greet("Maddison", new Date());
```

- We don't have to always write explicit type annotations.
- In many cases, TypeScript can even infer the types for us if we omit them.

![Screenshot](./assets/Screenshot%20from%202024-12-01%2011-17-50.png)

- Even though we din't tell TypeScript that `msg` had the type `string`, it was able to figure that out.
- That's a feature, and it is best to not add a type annotation when the type system would end up inferring the same type anyway.

#### Erased Types

- Let's take a look at what happens when we compile the above function `greet` with `tsc` to output JavaScript:

```js
"use strict"
// This is an industrial-grade general-purpose greeter function
function greet(person, date) {
    console.log("Hello ".concat(person, ", today is ").concat(date.toDateString()));
}
greet("Maddison", new Date());
```

- Notice two things here:
	- Our `person` and `date` parameters no longer have type annotations.
	- Our "template string" - that string the use backticks (the `` ` `` character) - was converted to plain strings with concatenations.
- Basically, TypeScript cannot run in JavaScript compiler, so it needs to convert the code into plain JavaScript so that it will run in JavaScript runtime.
- Basically, TypeScript is not a language, it's just a type-checking for JavaScript, hence it erases the type annotations.

#### Downleveling

- One other difference from the above was that our template string was rewritten from

```ts
`Hello ${person}, today is ${date.toDateString()}!`;
```

to

```js
"Hello ".concat(person, ", today is ").concat(date.toDateString(), "!")
```

###### Why did this happen?

- Template string are a feature from a version of ECMAScript 2015 a.k.a. ECMAScript 6, ES6 etc..
- TypeScript has the ability to rewrite code from newer versions of ECMAScript to older versions such as ECMAScript 3 or 5.
- This process of moving from higher version to lower version is sometimes called _downleveling_.

- By default, TypeScript converts to ES3, an extremeley old version of ECMAScript.
- We could choose any version by using `target` option.
- Running with `--target es2015` changes the emitted file to ECMAScript 2015.
- This feature is so that the TypeScript emitted file can run on older browsers as well.
#### Strictness
###### Different users want different things with TypeScript as a type-checker.
- Some people are looking for more loose opt-in experience which can help validate some parts of their program, and still have decent tooling.
- This is the default experience with typescript where types are optional, they are inferred leniently and there's no checking for potentially `null`/`undefined` values.
- `tsc` emits with errors.
- If you're migrating from JavaScript to TypeScript, this might be a desirable step.

###### In contrast, a lot of users prefer
- to have TypeScript validate as much as it can straight away and that's why the language provides strictness settings as well.
- These strictness settings are not like a switch (either on or off), rather they are like a dial.
- The further you turn this dial up, the stricter TypeScript will check for errors.
- This can require a little extra work, but generally speaking it pays off for itself in the long run, and enables more thorough checks and more accurate tooling.
- When possible, a new codebase should always turn these strictness checks on.

###### Typescript has several type-checking strictness flags
- than can be turned on or off, and all of our examples will be written with all of them enabled unless otherwise stated.
- The `strict` flag in the CLI, or `"strict": true` in a `tsconfig.json` toggles them all on simultaneously, but we can opt out of them individually.
- The two biggest ones you should know about are `noImplicitAny` and `strictNullChecks`.
#### `noImplicitAny`
- Recall that TypeScript in some places doesn't try to infer types for us and instead falls back to the most lenient type: `any`. 
- That isn't the worst thing - after all, falling back to `any` is just the plain JavaScript experience anyway.
- However, using `any` often defeats the purpose of using TypeScript in the first place.
- The more typed your program is, the more validation and tooling you'll get, meaning you'll run into fewer bugs as you code.
- Turning on the `noImplicitAny` flag will issue an error on any variables whose type is implicitly inferred as `any`.
#### `strictNullChecks`
- By default, values like `null` and `undefined` are assignable to any other type.
- This can make writing some code easier, but forgetting to handle `null` and `undefined` is the cause of countless bugs in the world - some consider it [a billion dollar mistake](https://youtu.be/ybrQvs4x0Ps?si=Y0ElZ38ttjbjoZ1f)
- The `strictNullChecks` flag makes handling `null` and `undefined` more explicit, and spares us from worrying about whether we forgot to handle `null` and `undefined`.