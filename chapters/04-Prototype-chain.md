What is Prototype Chain in javascript?

```js 
const skills = [];

skills.push("JavaScript");
```
So,we have created that skills array but we havenot created the 
method push,now the question arises in mind?
Where did it come from,does it belongs to our skills array or not?
Let's ask javascript

```js 
console.log(skills.hasOwnProperty("push"));
```
Output:
```text
true
```
Now we know where push() lives.

Not inside the array.

Inside its prototype.

