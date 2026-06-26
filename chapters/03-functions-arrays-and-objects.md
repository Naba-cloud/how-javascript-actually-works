# Functions, Arrays, and Objects

At the end of the previous chapter, we discovered something surprising.

```js
function greet() {}

console.log(greet instanceof Object);
```
``` Output:true ```

Wait...

Why is an array an object?

And if functions are objects, why doesn't JavaScript say `"object"`?

Instead, it says `"function"`.

At first glance, these results seem inconsistent.

But they're actually giving us an important clue about how JavaScript is designed.

Let's investigate.

## Mystery #1: Why Is an Array an Object?

When you first learn JavaScript, arrays and objects seem like two completely different things.

An object looks like this:

```js
const user = {
  name: "Aline"
};
```

An array looks like this:

```js
const skills = ["JavaScript", "React"];
```

Different syntax.

Different purpose.

So naturally, you'd expect them to be different types.

But JavaScript says otherwise.

```js
console.log(typeof user);
console.log(typeof skills);
```

Output:

```text
object
object
```

How can that be?

The answer is that an array **is** an object.

It's just a specialized object designed to store ordered data.

Think of it this way.

A backpack and a suitcase are different.

One is designed for carrying books.

The other is designed for traveling.

But both are still containers.

Arrays and plain objects have a similar relationship.

Both are objects.

They simply solve different problems.

Let's see if JavaScript treats an array like an object.

```js
const skills = ["JavaScript", "React"];

skills.author = "Aline";

console.log(skills.author);
```

Output:

```text
Aline
```

Interesting.

We just added a custom property to an array.

That's something we've only done with objects before.

The more we experiment, the more it feels like arrays really are objects.
## Mystery #2: Why Are Functions Objects?

Now things become even stranger.

Take a simple function.

```js
function greet() {
  console.log("Hello");
}
```

Let's try something unexpected.

```js
greet.language = "JavaScript";

console.log(greet.language);
```

Output:

```text
JavaScript
```

Wait...

We just attached a property to a function.

How is that possible?

Because functions are objects too.

Like arrays, functions are specialized objects.

They can store properties.

They can have methods.

But they also have one extra ability.

They can be called.
## Then Why Does `typeof` Return `"function"`?

This is probably the most common question.

If functions are objects...

Why doesn't JavaScript say:

```text
object
```

Instead, it says:

```text
function
```

The answer is simple.

Functions are such a special kind of object that JavaScript gives them their own `typeof` result.

Think of it like this.

Every function is an object.

But not every object is a function.

```text
Object
│
├── Plain Object
├── Array
└── Function
```

Functions inherit everything an object can do.

They just have one additional superpower.

They can be executed using `()`.
Still not convinced?

Let's ask JavaScript directly.

```js
function greet() {}

console.log(greet instanceof Object);
```

Output:

```text
true
```

JavaScript confirms it.

Functions are objects.

Now let's try the same thing with an array.

```js
const skills = [];

console.log(skills instanceof Object);
```

Output:

```text
true
```

Arrays are objects too.
At this point, we've uncovered something fascinating.

- Arrays are objects.
- Functions are objects.
- Plain objects are... objects.

But another mystery has appeared.

Take a look.

```js
const skills = [];

console.log(skills.push);
```

Output:

```text
ƒ push() { ... }
```

Hold on...

We never created a `push()` method.

So where did it come from?

The same thing happens here.

```js
const user = {
  name: "Aline"
};

console.log(user.toString);
```

We didn't create `toString()` either.

So who did?

That's exactly what we'll uncover in the next chapter, where we'll explore one of JavaScript's most powerful concepts:

**The Prototype Chain.**