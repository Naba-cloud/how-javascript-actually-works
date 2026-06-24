# Chapter 1: Memory and References

## Introduction

Many JavaScript developers know that objects are copied by reference, but few understand what that actually means.

---

## The Problem

Consider the following code:

```js
const user = {
  name: "Aline"
};

const copy = user;

copy.name = "Ali";
```
# Why Does Changing `copy` Also Change `user`?

Imagine you're working on a production application.

You create a copy of an object so you can safely modify it.

```js
const user = {
  name: "Aline"
};

const copy = user;

copy.name = "Ali";

console.log(user.name);
```

What do you expect to see?

Is your answer is:

```js
"Aline"
```

After all, we changed `copy`, not `user`.

But JavaScript prints:

```js
"Ali"
```

Wait...

How did changing `copy` affect `user`?

Did JavaScript secretly update both variables?

Did it create some hidden connection between them?

To answer that question, we need to look inside JavaScript's memory.

---

## Let's Explore 

When JavaScript executes:

```js
const user = {
  name: "Aline"
};
```

it creates an object somewhere in memory.

For simplicity, imagine JavaScript stores that object at address `0x001`.

### Heap

```text
0x001
┌─────────────┐
│{            │
│ name:Aline  │
│}            │
└─────────────┘
```

The variable itself is stored separately.

Instead of storing the entire object, it stores only the object's address.

### Stack

```text
user ──────► 0x001
```

Think of it like a house address.

The object is the house.

The variable only stores the address of that house.

---

## The Mistake We Make

When they see:

```js
const copy = user;
```

many of you imagine this:

```text
user ──────► Object A

copy ──────► Object B
```

Two separate objects.

Two separate locations in memory.

But that's not what happens.

JavaScript does not create a new object.

It simply copies the address.

### Stack

```text
user ──────► 0x001
copy ──────► 0x001
```

Both variables now point to exactly the same object.

There is only one object in memory.

---

## The Moment Everything Makes Sense

Now JavaScript executes:

```js
copy.name = "Ali";
```

It follows the address stored inside `copy`.

```text
copy ──────► 0x001
```

and updates the object found there.

### Heap

```text
0x001
┌─────────────┐
│ {           │
│ name:Ali    │
│ }           │
└─────────────┘
```

But `user` points to that exact same object.

```text
user ──────► 0x001
```

So when we read:

```js
console.log(user.name);
```

JavaScript finds:

```js
"Ali"
```

Suddenly the behavior is no longer strange.

There were never two objects.

There was only one.

---

# Creating a New Object

So how do we create a new object?

Most of use the spread operator.

```js
const copy = {
  ...user
};
```

Now JavaScript creates a second object.

### Stack

```text
user ──────► 0x001
copy ──────► 0x002
```

### Heap

```text
0x001               0x002

{                    {
  name:"Aline"          name:"Aline"
}                    }
```

Perfect.

Problem solved.

Or is it not?

---

# The Hidden Trap

Let's make the object slightly more realistic.

```js
const user = {
  name: "Aline",
  address: {
    city: "Karachi"
  }
};

const copy = {
  ...user
};
```

Question:

What happens if we execute:

```js
copy.address.city = "Lahore";
```

Will `user.address.city` stay `"Karachi"`?

Most people confidently answer:

> Yes, because we created a copy.

Unfortunately, JavaScript has another surprise waiting for us.

---

# Looking Deeper Into Memory

This time the memory looks like this:

### Stack

```text
user ──────► 0x001
copy ──────► 0x002
```

### Heap

```text
0x001
{
  name:"Aline",
  address ───► 0x100
}

0x002
{
  name:"Aline",
  address ───► 0x100
}

0x100
{
  city:"Karachi"
}
```

Notice something interesting.

The outer objects are different.

```text
0x001 ≠ 0x002
```

But the nested object is shared.

```text
address ───► 0x100
```

Both objects point to the same nested object.

This is called a **shallow copy**.

The first level is copied.

Nested objects are not.

---

# The Bug Appears

Now JavaScript executes:

```js
copy.address.city = "Lahore";
```

It follows:

```text
copy
 ↓
0x002
 ↓
address
 ↓
0x100
```

and updates the shared object.

```text
0x100
{
  city:"Lahore"
}
```

Since `user.address` also points to `0x100`, it sees the update too.

```js
console.log(user.address.city);
```

Output:

```js
"Lahore"
```

Even though we never modified `user`.

---

# Why `structuredClone()` Exists

We kept running into this problem.

They wanted a copy that duplicated everything.

Not just the first level.

Not just the outer object.

Everything.

That's why JavaScript introduced:

```js
const copy = structuredClone(user);
```

Now JavaScript creates completely separate nested objects.

```text
user ──────► 0x001
copy ──────► 0x002


0x001.address ──────► 0x100

0x002.address ──────► 0x200
```

Different outer objects.

Different nested objects.

No shared references.

No accidental mutations.

No surprises.

And that's the real difference between a shallow copy and a deep copy.

---
## Quick Challenge

Without running the code, predict the output:

```js
const user = {
  profile: {
    city: "Karachi"
  }
};

const copy = { ...user };

copy.profile.city = "Lahore";

console.log(user.profile.city);