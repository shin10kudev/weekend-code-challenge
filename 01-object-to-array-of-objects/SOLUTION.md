# Solution

Honestly, there are a ton of ways to do this, and if you check stack overflow you will find no end to people giving advice about the problem.

This is the solution I happen to like. It is cool, easy to read, and utilizes various of "bread and butter" JavaScript techniques, which are important for you to learn and know.

## Solution 1: The long way

```js
const ingredients = { lettuce: 0, bacon: 0, cheese: 1, meat: 4 };

const output = Object.keys(ingredients).reduce((acc, curr) => {
  acc.push({
    name: curr,
    count: ingredients[curr],
  });
  return acc;
}, []);
```

## Solution 2: Use concat to shorten this up

This will apply the same methodology as before, but we can tighten things up by using the `.concat()` array method:

```js
const ingredients = { lettuce: 0, bacon: 0, cheese: 1, meat: 4 };

const output = Object.keys(ingredients).reduce(
  (acc, curr) =>
    acc.concat([
      {
        name: curr,
        count: ingredients[curr],
      },
    ]),
  [],
);
```

## Solution 3: Use the spread operator

Another short solution to keep the code clean and short:

```js
const ingredients = { lettuce: 0, bacon: 0, cheese: 1, meat: 4 };

const output = Object.keys(ingredients).reduce(
  (acc, curr) => [
    ...acc,
    {
      name: curr,
      count: ingredients[curr],
    },
  ],
  [],
);
```
