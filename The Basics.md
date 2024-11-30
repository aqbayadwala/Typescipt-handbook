Each and every value in js has a set of behaviors you can observe from running different operations

```
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

```
const message = 'Hello world!;'
```

- Here, `message.toLowerCase()` will return same string in lowercase
- But, the `message()` call will not work and will return

```
TypeError: message is not a function
```

It'd be great if we could avoid mistakes like this

Now here two concepts in this context
- What is a type?
	- A  *type* is the concept of describing which values can be passed and which will crash.
- What is dynamic typing?
	- Running the code and checking what happens

#### Static type-checking

- Most people don't like errors when running their code 
<br>
- If we add just a bit of code, save the file and re-run, and get an error and debug it, that is easy and the problem can be fixed quickly.
- But we might not have tested the feature thoroughly enough, so we might never actually come across the error
- Or if we are lucky enough to see the error, we might end up doing a lot of refactoring and adding a lot of different code to fix the problem for moment.
<br>
- If we have a tool that helps us find these bugs before our code runs, it would be so good.
- That's what a static type checker is and does.
- Static type systems describe the shapes and behaviors of what our values will be when we run our programs.
- A type-checker like TypeScript uses that information (shape and behavior) and tells us when things might go off the rails.

```
const message = "hello!";

message();
> This expressions is not callable.
> Type 'String' has no call signatures
```

Running this code with TypeScript will give us an error message before we run the JavaScript code in the first place.

#### Non-exception failures

- So far, we have discussed runtime errors - meaning js's runtime telling us when it thinks something is nonsensical.
- ECMAScript specification defines what is an error and not an error in JavaScript
- In ECMAScript specs
	- calling something which isn't callable throws an error
	- but calling a property which doesn't exist returns `undefined`
	
- JavaScript
	
```
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
| Property 'location' does not exist on type '{ name: string; age: number;}'
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

