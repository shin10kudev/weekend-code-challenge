# Problem

I got this problem from a course I'm working on learning React Native.

Your task is to create a function that, given a number greater than `0`, will return an array of `6` random numbers where the sum of a few of those values will equal the value originally passed to the function.

Okay, that might sound a little confusing, so here is a quick example to show you what I mean.

Given the following function:

```js
addUpToOneNumber(10);
```

One sample expected result would be:

```js
result = [1, 4, 6, 3, 8, 9];
```

Note, that `1 + 3 + 6` will add up to `10`. Incidentally, `4 + 6` and `9 + 1` will also add up to `10`. There is no issue with having multiple routes to the number supplied to the function. However there needs to be at least one route in the returned array.

For this reason, you may also want to create a secondary helper function, which you can use to quickly validate your answer. For example:

```js
validateAnswer(10, [1, 4, 6, 3, 8, 9]); // returns true
validateAnswer(14, [6, 3, 2, 14, 4, 6]); // returns false
```

## Edge cases

Please consider how you will handle cases where the number is `1` or `0` or a negative number -- or any other edge cases you can think of.

You don't have to worry about duplicate numbers in the returned array, unless you want to make that a requirement to return an array of unique numbers.
