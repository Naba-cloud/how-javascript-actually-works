# Primitives vs Objects

In the previous chapter, we learned why changing `copy` can unexpectedly change `user`.

The reason was simple:

Both variables were pointing to the same object in memory.

But that raises an interesting question.

Why does this happen with objects, but not with strings?

Take a look at this example:

```js
let firstName = "Aline";
let copy = firstName;

copy = "Ali";

console.log(firstName);
```

What do you think the output will be?

If you said:

```js
"Aline"
```

you're absolutely right.

Changing `copy` doesn't affect `firstName`.

Now compare that with an object:

```js
const user = {
  name: "alina"
};

const copy = user;

copy.name = "Ali";

console.log(user.name);
```

This time the output is:

```js
"Ali"
```

Wait a second.

Why are these examples behaving differently?

In both cases we created a copy.

In both cases we changed the copy.

Yet one leaves the original value untouched, while the other changes it.

At first glance, JavaScript seems inconsistent.

But once you understand the difference between primitive values and objects, everything starts to make sense.

Let's investigate.

---

## Meet the Primitive Values

#### Before We Continue, What Does "Primitive" Actually Mean?

We've been using the word primitive, but what does it actually mean?

In simple terms, a primitive is a single value.

It isn't made up of smaller values.

For example:

```
const name = "Aline";
```

Each variable contains one simple value

A string.

A number.

A boolean.

Nothing more.

## Types

JavaScript has seven primitive types:

These include:

```js
String
Number
Boolean
Undefined
Null
Symbol
BigInt
```

These values are simple.

JavaScript can store them directly.

When we write:

```js
let a = "alina";
let b = a;
```

JavaScript copies the actual value.

```text
a ───► "alina"
b ───► "alina"
```

Notice something important.

Both variables have their own copy.

There is no connection between them.

So when we execute:

```js
b = "Ali";
```

JavaScript simply changes the value inside `b`.

```text
a ───► "alina"
b ───► "Ali"
```

Nothing happens to `a`.

This behavior is called **copy by value**.

---

## Objects Play By Different Rules

Now let's revisit our earlier example.

```js
const user = {
  name: "Alina"
};

const copy = user;
```

Unlike strings and numbers, objects are not copied directly.

Instead, JavaScript copies a reference.

```text
user ───► Object

copy ───► Same Object
```

Both variables point to the same object.

That's why changing the object through one variable affects the other.

```js
copy.name = "Ali";
```

The object changes.

Since both variables point to it, both variables see the update.

This behavior is called **copy by reference**.

---

## A Simple Rule To Remember

Whenever you're confused, ask yourself one question:

> Am I working with a primitive value or an object?

If it's a primitive:

```js
const a = "Hello";
const b = a;
```

JavaScript copies the value.

If it's an object:

```js
const a = {};
const b = a;
```

JavaScript copies the reference.

## Meet the Second Category: Non-Primitive Types Or Objects

So far, we've talked about primitive values.

But not every value in JavaScript is a primitive.

Whenever you need to store multiple pieces of information together, JavaScript gives you objects.

```js
const user = {
  name: "Alina",
  age: 15
};
```

Unlike primitives, objects can contain multiple values and even other objects.

Arrays and functions also belong to this category.

```js
const skills = ["JavaScript", "React"];

function greet() {
  console.log("Hello");
}
```

```js
console.log("typeof skills",typeof skills)
console.log("typeof greet",typeof greet)
console.log("greet",greet instanceof Object)
```
### Output
```
Object
function
true
```

At this point, we've uncovered something interesting.

Functions aren't just pieces of code that can be executed.

They're also objects.

```js
function greet() {}

console.log(greet instanceof Object); // true
```

That might seem strange at first, but it opens the door to some fascinating questions.

How can something be both a function and an object?

What special powers do functions have that regular objects don't?

And why does JavaScript treat them differently?

Let's find out.

In the next chapter, we'll take a closer look at functions and discover what actually makes them special.
