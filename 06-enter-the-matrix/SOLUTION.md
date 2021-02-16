# Solution

I'd like to introduce two solutions, both of which provide good practice for thinking about time complexity and practicing techniques like using additional pointers.

## Solution 1: Using Array.prototype.join()

My first solution to this problem is similar to the approach we'd take if we were looking for duplicates in a flattened array. That is, we would maintain a hash table of previously seen values. Going through that array, we might generate a hash table that looks like this, where the key is the item we encountered, and the value is the count of times we encountered it:

```js
const seenValues = {
  1: 1,
  2: 2,
  15: 4,
};
```

Applying this concept, we could loop through each item in the matrix, but how would we keep track of the combinations we encounter effectively?

I opted to us `matrixRow.join('')`, which will produce a stringified version of the combination. So my hash table ended up looking like this:

```js
seenStrings = {
  01101: 2,
  10010: 1,
};
```

If the count is equal to two, I can add that matrixRow to an output array, otherwise, I can ignore it, as I know it means the combination has already been added.

My code looks like this:

```js
function findDupesInMatrix(matrix) {
  const dupes = [];
  const seenStrings = {};

  for (let i = 0; i < matrix.length; i++) {
    const matrixRow = matrix[i];
    const matrixString = matrixRow.join('');

    if (!seenStrings[matrixString]) {
      seenStrings[matrixString] = 1;
    } else {
      seenStrings[matrixString]++;

      if (seenStrings[matrixString] === 2) {
        dupes.push(matrixRow);
      }
    }
  }

  return dupes;
}
```

I use a for loop to iterate through each item in the matrix. Then on each `matrixRow`, I tranform the array into a string.

By doing this, I'm able to keep track of which ones I have seen, and if the count for any seen values reaches `2`, I know I can add it to the ouput array.

In this way, even if there are multiple instances of the same combination, my output array will only ever have 1 instance of each duplicated combination.

Note that if I didn't want to use `.join()`, I could easily use another for loop:

Since each of the rows have the same length, I can cache this value as `matrixRowLen`, and possibly this would be even faster than the built-in join method.

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

Expecially after refactoring to use the second for loop, it would appear that this solution is `O(n^2)`, but it's actually linear. To be more precise, it would be `O(w*h)` where `w` corresponds to the width, or length of the column, and `h` corresponds to the length or height of the rows.

I found a very interesting discussion about the time complexity of traversing a 2-D array here on this [stackoverflow question](https://stackoverflow.com/questions/30100847/what-is-the-time-complexity-of-traversing-a-2d-array).

## Solution 2: Using Array.prototype.flatten()

I wanted to think about this problem another way, so I thought about what I could do if I first flatten the matrix. Doing so, we lose the structure of the rows, but since the length of the sub arrays will be fixed, we can utilize some simple pointers to keep track of where we are.

I still want to utilize the previous method of keeping track of visited combinations using the hash table.

The additional pointers can be difficult if you are not accustomed to this technique, but it is good practice. I find that giving good names for the pointers, also helps clarify the useage, so I decided to call my pointers, `colIndex` and `rowIndex`, since I am using them to keep track of where I would have been in the column and row of the matrix if it weren't flattened:

```js
function findDupesInMatrix(matrix) {
  const dupes = [];
  const seenStrings = {};
  const limit = matrix[0].length;
  let colIndex = 0;
  let rowIndex = 0;

  const flattenedMatrix = matrix.flat();

  let combination = '';
  for (let i = 0; i < flattenedMatrix.length; i++) {
    combination += flattenedMatrix[i];
    colIndex++;

    if (colIndex === limit) {
      if (!seenStrings[combination]) {
        seenStrings[combination] = 1;
      } else {
        seenStrings[combination]++;

        if (seenStrings[combination] == 2) {
          dupes.push(matrix[rowIndex]);
        }
      }

      rowIndex++;
      colIndex = 0;
      combination = '';
    }
  }

  return dupes;
}
```

I was curious about the run time of `Array.prototype.flatten()`, and from what I could find it runs in `O(n)` time. With the for loop I use to iterate through the flattened array, I would have `O(2n)` or `O(n)` run time for short.
