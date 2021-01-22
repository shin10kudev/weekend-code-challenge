# Problem

This problem was introduced by my good friend [justjohnd](https://github.com/justjohnd), and it goes like this:

Create a function that, given a non-negative integer, will return an array of arrays containing all the possible non-repeating combinations of integers from 1-n.

So, if we called the function `showMeTheCombinations(3)`, the expected output would be:

```js
[
  [1, 2],
  [1, 3],
  [2, 3],
];
```

Notice that the output includes all the numbers from `1` to `n`, `n` in this case being `3`. Note also that it all the possible combinations of numbers from `1-3` are also included without any duplicates.

## Further information

For the sake of simplicity, you only have to worry about combinations of doubles (i.e. `[1,2]`, `[1, 3]`, and so on.). You don't have to return `[1,2,3]` or any other combinations that are greater than 2.
