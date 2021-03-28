# Solution

- TBD

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
