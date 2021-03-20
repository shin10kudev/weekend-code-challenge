# Solution

Let's take a look at a brute force solution and a more optimal one.

## Solution 1: The brute force approach

One way we could certainly go about solving this problem is to go through the array 3 times. During each pass, we would need to find the current largest number, and we'd also need to handle the case where there are duplicates of the largest numbers.

Rather than keeping track of the count of duplicate large numbers, which could get really confusing, we could keep track of the index of where we found the largest number and either remove it from the array or skip that index.

First, let's create a helper method that we can use to find the largest number in an array, and since we want to be able to skip over numbers we've seen before, we'll also pass an array of indexes that we want to skip.

```js
function getLargestNumber(numbers, skipIdxs) {
  let max = numbers[0];
  let maxIdx = 0;

  for(let i = 1; i < numbers.length; i++) {
    if (skipIdxs.includes(i)) continue;

    const val = numbers[i];

    if (val > max) {
      max = val;
      maxIdx = i;
    }
  }

  return [max, maxIdx];
}
```

For the main part of the function, we just need to call this helper 3 times. During each iteration, we'll keep track of the index we need to skip the next time around, as well as the largest value we found.

```js
function findBigThree(numbers) {
  const largest = [];
  const skipIdxs = [];
  let count = 0;

  while (count < 3) {
    const [maxVal, maxValIdx] = getLargestNumber(numbers, skipIdxs);
    skipIdxs.push(maxValIdx);
    largest.unshift(maxVal);
    count++;
  }

  return largest;
}
```

Since we loop through all the numbers 3 times, the time complexity of this solution is `O(3n)` or `O(n)`. While this solution is technically linear, it is quite inefficient considering we have to loop through the array 3 times. It is possible to do this task in one pass, so let's take a look at how that can be done.

## Solution 2: The single pass

Some of this code will look a little familiar from the previous solution, but the technique we use it quite different.

Essentially what we'll do is sort the array as we go along and any time the array gets larger than 3, we'll chop of the first value. This will ensure that we find the 3 largest values and that during our placement of each value, we are always dealing with a fixed time operation of comparing up to 3 values.

```js
function findBigThree(array) {
  const largest = [];

  for (let i = 0; i < array.length; i++) {
    const val = array[i];
    let currIdx = 0;

    while (currIdx < largest.length) {
      if (val < largest[currIdx]) break;
      currIdx++;
    }
    largest.splice(currIdx, 0, val);

    if (largest.length > 3) {
      largest.shift();
    }
  }

  return largest;
}
```

We could optimize the placement logic by utilizing a binary search sort of technique, but there are only 3 values, so it is not going to affect time complexity in any meaningful way.

With this, we have a linear solution and we only had to make one pass.


## Solution 3: The triple variable

This solution sets variables for the three highest values, and updates them
as a new high value is encountered while looping through the values. This reduces actual runtime by eliminating the 'while' loop for each value in the array.

```js
function getBigThree(array) {
  const largest = [null, null, null];
  
  for (const num of array) {
    if(num > largest[2] || !largest[2]) {
      largest[0] = largest[1];
      largest[1] = largest[2];
      largest[2] = num;
    } else if(num > largest[1] || !largest[1]) {
      largest[0] = largest[1];
      largest[1] = num;
    } else if(num > largest[0] || !largest[0]) {
      largest[0] = num;
    }
  }
  
  return largest;
}
```

One consideration, though, would be how to implement this with a non-static number of returned high values. (e.g. getBigThree(array, numberOfValues))

## Conclusion

Hopefully, by now you have gotten a lot more comfortable with utilizing a mix of for loops and while loops, and also making use of reference pointers of all kinds to keep track of where you are and what you need in order to make various logical decisions within your algorithms.

These techniques are incredibly powerful and allow you to solve an ever greater number of problems, so even if you aren't quite comfortable with it yet, trust me that with more practice it will become increasingly natural for you to reach for these techniques and relish the features they unlock.
