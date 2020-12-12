# Solutions

Honestly, there are a ton of different ways to do this, and if you check stack overflow, you will find no end to people giving advice about the problem.

This is the solution I like. It is cool, easy to read, and utilizes a lot of bread and butter JavaScript techniques that are important for you to understand and know.

## Solution 1: The long way

```js
let ingredients = { lettuce: 0, bacon: 0, cheese: 1, meat: 4 };
let output = Object.keys(ingredients).reduce((acc, curr) => {
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
let ingredients = { lettuce: 0, bacon: 0, cheese: 1, meat: 4 };
let output = Object.keys(ingredients).reduce(
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
let ingredients = { lettuce: 0, bacon: 0, cheese: 1, meat: 4 };
let o = Object.keys(ingredients).reduce(
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
