# Solution

For problems like these, there are dozens upon dozens of possible answers, each with their own merits and demerits, quirks and qualities. Since our input data is an `object`, I think it is the perfect chance to utilize the various methods that exist on the object prototype in JavaScript, and practice techniques for looping through object data.

In particular, I'd like to demonstrate how to solve this problem using `Object.keys()` + `Object.values()`, the `for... in` statement and also `Object.entries()`.

## Solution 1: Object.keys + Object.values

`Object.keys()` will give you an array of the keys (as strings) from the object, whereas `Object.values()` will give you an array of the values that exist for each key in the object.

With this, keys[index] maps directly to values[index], so you can then loop through the data and put the two pieces together as such:

```js
const convertObjToArrayOfObjects = (data) => {
  const values = Object.values(data);
  return Object.keys(data).map((id, index) => ({
    id,
    ...values[index],
  }));
  return output;
};
```

This is, well, not the most effecient way to do things... because you are looping through the object twice to construct the arrays of keys and values, plus you're looping again through the keys in order to get the final output. Although it is an interesting technique that demonstrates various concepts in JavaScript, we can do better in terms of time complexity.

## Solution 2: Using Object.entries()

The [Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) method will convert an object of key-value pairs into an array of arrays where the output data is structured like: `[[key, value], [key, value]]`. Since we now have an array, from here we can utilize JavaScript's powerful array methods, like `.map()` -- and with a little destructuring, we can create a niftly little function like so:

```js
const convertObjToArrayOfObjects = (data) => {
  return Object.entries(data).map(([key, values]) => {
    return {
      id: key,
      ...values,
    };
  });
};
```

Interestingly, as stated in the MDN Web docs for Object.entries():

> The order of the array returned by Object.entries() does not depend on how an object is defined. If there is a need for certain ordering, then the array should be sorted first, like Object.entries(obj).sort((a, b) => b[0].localeCompare(a[0]));.

Source: [Object.entries() | JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

For our purposes, the order is not important, so we'll ignore, but please keep this in mind when using this method.

## Solution 3: Using for... in + Object.entries()

Sticking with `Object.entries()` for a second, let's combine it with another method and see what we can do. The `for... in` statement allows us to loop through all the keys in an object. We can combine this with `Object.entries()` and use destructuring in a similar way to get our intended result.

```js
const convertObjToArrayOfObjects = (data) => {
  const output = [];
  for (const [key, values] of Object.entries(data)) {
    output.push({
      id: key,
      ...values,
    });
  }

  return output;
};
```

If you wanted to avoid using the `.push()` method, you could also do this:

```js
let output = [];
...
output = [
  ...output,
  {
    id: key,
    ...values,
  },
];
```

This solution is meant to show how we can combine object methods plus destructuring to get our intended result.

Still, I feel like this isn't as readable as the previous solution. Plus we have to set aside an empty array to prepare our output value. Although memory probably isn't a concern for JavaScript running on a web browser, the previous solution was definitely cleaner and more concise.

## Solution 4: Utilizing only for... in

We arrive at the last method I'd like to show today. And credit to [mwhitman189](https://github.com/mwhitman189) for coming up with the inspiration for this idea. This is the only solution here today that allows us to accomplish the given task in one pass, (although we still do have to set aside an empty array for our output).

As described above, `for... in` allows us to loop through all the keys in an object. And while we are doing that we can use destructuring to take what we like, and pass it to our output data. One example looks like this:

```js
const convertObjToArrayOfObjects = (data) => {
  const output = [];
  for (const key in data) {
    output.push({
      id: key,
      ...data[key],
    });
  }
  return output;
};
```

If we wanted to explicitly pass certain values to the output data, we could destructure `data[key]` like this:

```js
const { title, copy, link } = data[key];
...
output.push({
  id: key,
  title,
  copy,
  link,
});
```

## Summary

It's always good to consider multiple ways to solve a problem and also think about how methods can be combined to generate different types of solutions. For the solutions here, it was helpful for me to see how other people were solving the same problem to come up with other solutions that I didn't think of. So hopefully, comparing your answers to this document or to solutions from your peers, will yield similar results.
