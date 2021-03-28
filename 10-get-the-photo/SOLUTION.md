# Solution

We take a break from the classic algorithm-type problems of the last few challenges to do a task that often comes up in the day to day work of a developer: namely, converting a data structure from one type to another.

The reason I do this is to show how the techniques we employ to solve algorithm problems can come in handy when we need to do something practical.

## Solution 1: Loops and while loops

Let's exercise our JavaScript syntactic skills and do this problem using a standard `for loop`, a while loop, and finally a `for...of` loop, which was introduced in [ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of).

### The for loop

```js
function getLargePhotos(photos) {
  const output = [];

  for (let i = 0; i < photos.length; i++) {
    const { large } = photos[i];
    output.push(large);
  }

  return output;
}
```

### The while loop

```js
function getLargePhotos(photos) {
  const output = [];
  let idx = 0;

  while (idx < photos.length) {
    const { large } = photos[idx];
    output.push(large);
    idx++;
  }

  return output;
}
```

### The for...of loop

```js
function getLargePhotos(photos) {
  const output = [];

  for (const photo of photos) {
    const { large } = photo;
    output.push(large);
  }

  return output;
}
```

Each of these solutions run in linear time. Nothing special here -- but it's great to practice writing these out to flex your JavaScript muscles beyond built-in array methods.

I think that of the 3, the `for...of` loop is the easiest to parse visually, and perhaps the easiest to write out. The while loop is also nice, although we have to keep updating the `idx` just like we do in the `for loop`.

While it's good to always know how to whip up a `for loop`, if you don't need to do anything special with indexes, the `for...of` loop is great for keeping the messy wires of the classic `for loop` tucked underneath the desk, so to speak.

## Solution 2: Using map()

Having said that, an array method like `.map()` has one great benefit in addition to the semantic sugar it provides: it returns a new array.

```js
function getLargePhotos(photos) {
  return photos.map(({ large }) => large);
}
```

Of course, this means that we don't have to initialize an empty array or use `.push()` to add new items to that array.

## Conclusion

When you encounter problems like these, using the full spectrum of JavaScript looping methods reveals a lot about what makes each method great. And we can fully understand the appeal of a function like `.map()` when compared to its `for loop` predecessor.

Perhaps in a future challenge we can try to write a polyfill for the map function to get a greater understanding of how it works under the hood.

## Other solutions

Check out other solutions to this problem

### Using reduce - Submitted by [mwhitman189](https://github.com/mwhitman189)

The `.reduce()` method is a perfect candidate for solving this problem:

```js
function getLargePhotos(photos) {
  return photos.reduce((acc, curr) => {
    acc.push(curr['large']);
    return acc;
  }, []);
}
```

And we can clean this up by utilizing es6 [object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) and the [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax), like so:

```js
function getLargePhotos(photos) {
  return photos.reduce((acc, curr) => {
    const { large } = curr;
    return [...acc, large];
  }, []);
}
```

Finally, we can take this even further to create a one line solution by using the [implicit return feature of es6 arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions). Thanks to [shin10kudev](https://github.com/shin10kudev) for pointing this out:

```js
const getLargePhotos = (photos) =>
  photos.reduce((acc, { large }) => [...acc, large], []);
```

Something to notice is that in the one line solution, `acc.push()` will not work because the `acc.push()` would be returned rather than the `acc`.
