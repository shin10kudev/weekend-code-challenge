# Problem

I got this problem from a course I'm working on learning React Native.

Your task is to create a function that, given an number greater than zero, will return an array of 6 random numbers where the sum of a few of those values will equal the value originally passed to the function.

Okay, that might sound a little confusing, so here is a quick example to show you what I mean.

Given the following function:

```js
addUpToOneNumber(10);
```

One sample expected result would be:

```js
result = [1, 4, 6, 3, 8];
```

Note, that `1 + 3 + 6` will add up to `10`. Incidentally, `4 + 6` will also add up to `10`, as well. There is no issue with having multiple routes to the given, number, however there needs to be at least one route.

## Edge cases

Please consider how you will handle cases where the number is `1` or `0` or a negative number -- or any other edge cases you can think of.
