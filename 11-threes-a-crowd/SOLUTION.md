# Solution

## Solution 1: Simple Array Manipulation

This is perhaps the most straightforward solution to this problem. We simply sort the array and remove the first and last unit using `.pop()` and `.shift()`. The two values are pushed into an new array, and this array is subsequently added to our final array of arrays.

A `while loop` can be used to keep track of the length of the initial array. When a single value remains, this is added to the last-created two-value array.

The `spread operator` is not essential, but prevents the original array from mutating.

```js
const array = [3, 7, 10, 5, 4, 4, 1];

function PairingFunction(array) {
  const sortedArray = [...array.sort((a, b) => a - b)];

  let pairedArray = [];
  let arrayOfPairs = [];

  while (sortedArray.length > 1) {
    const minNumber = sortedArray[0];
    const maxNumber = sortedArray[sortedArray.length - 1];
    pairedArray.push(minNumber);
    pairedArray.push(maxNumber);
    arrayOfPairs.push(pairedArray);
    pairedArray = [];
    sortedArray.pop();
    sortedArray.shift();
  }

  if (sortedArray.length === 1) {
    arrayOfPairs[arrayOfPairs.length - 1].push(sortedArray[0]);
  }
  return arrayOfPairs;
}

PairingFunction(array);
```

## Conclusion

## Other solutions
