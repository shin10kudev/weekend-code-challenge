# Solution

One solution to this problem is similar to the solution if we were looking for dupes in a flattened array. That is, we would maintain a hash table of previously seen values. Going through that array, we might generate a hash table that looks like this, where the key is the item we encountered, and the value is the count of times we encountered it:

```js
const seenValues = {
  1: 1,
  2: 2,
  15: 4,
};
```

Applying this concept, we could loop through each item in the matrix, but how can we keep track of those combinations effectively?

I opted to us `matrixRow.join('')`, which will produce a stringified version of the combination. So my hash table ended up looking like this:

```js
seenStrings = {
  01101: 2,
  10010: 1,
};
```

If the count is equal to two, I can add that matrixRow to an output array, otherwise, just ignore, as the combination has already been added.

My code looks like this:

```js
function findDupesInMatrix(matrix) {
  const output = [];
  const seenStrings = {};

  for (let i = 0; i < matrix.length; i++) {
    const matrixRow = matrix[i];
    const matrixString = matrixRow.join('');

    if (!seenStrings[matrixString]) {
      seenStrings[matrixString] = 1;
    } else {
      seenStrings[matrixString]++;

      if (seenStrings[matrixString] === 2) {
        output.push(matrixRow);
      }
    }
  }

  return output;
}
```

I use a for loop to iterate through each item in the matrix. Then on each `matrixRow`, I tranform the array into a string of numbers.

By doing this, I'm able to keep track of which ones I have seen, and if the count for any values reaches `2`, I know I can add it to the ouput array.

In this way, even if there are multiple instances of the same combination, my output array will only ever have 1 instance of each duplicated combination.

## Conclusion

So what's the time complexity of this one? Seems like it would be O(n), but actually, using the join method, makes this algorithm O(n^2), because for each item in the matrix, the join method is internally looping through all the elements to concatonate them.

You can see this more easily if I actually used a second for loop:

```js
for (let i = 0; i < matrix.length; i++) {
  const matrixRow = matrix[i];
  const matrixRowLen = matrixRow.length;

  let matrixString = '';
  for (let j = 0; j < matrixRowLen; j++) {
    matrixString += matrixRow[j];
  }
  ...
}
```

This is a good lesson about how using built-in JavaScript methods can throw you off unless you take the time to think about how they may impact time complexity.
