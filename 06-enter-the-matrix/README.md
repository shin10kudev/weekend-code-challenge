# Problem

Given a matrix of zeros and ones like the following:

```js
const matrix = [
  [1, 0, 0, 1, 0],
  [1, 0, 1, 0, 0],
  [1, 0, 1, 0, 1],
  [1, 0, 1, 0, 1],
  [1, 0, 1, 1, 0],
];
```

...write a function that will return an array of any sequences that have a matching sequence in the matrix.

For the above matrix, the correct answer would be `[[1, 0, 1, 0, 1]]`, since this sequence appears twice, once at `matrix[2]` and again at `matrix[3]`.

## Notes

- All of the sub arrays will have the same length.
- It's possible there could be multiple repeated combinations, and you should return an array with all the duplicated combinations.
- It's possible that the same sequence could appear more than twice, but in the output you should only include it once.
