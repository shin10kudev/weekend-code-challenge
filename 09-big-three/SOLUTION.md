# Solution

Let's take a look at a brute force solution and a more optimal one.

## Solution 1: The brute force approach

One way we could certainly go about solving this problem is to go through the array 3 times. During each pass, we would need to find the current largest number, and we'd also need to handle the case where there are duplicates of the largest numbers.

Rather than keeping track of the count of duplicate large numbers, which could get really confusing, we could keep track of the index of where we found the largest number so we can be sure to skip that number on a subsequent pass.

First, let's create a helper method which we can use to find the largest number in an array and pass an array of indexes that we want to skip.

```js
function findBigThree(numbers, skipIdxs) {
  let maxVal = numbers[0];
  let maxValIdx = 0;

  for (let i = 1; i < numbers.length; i++) {
    if (skipIdxs.includes(i)) continue;

    const val = numbers[i];

    if (val > maxVal) {
      maxVal = val;
      maxValIdx = i;
    }
  }

  return {
    maxVal,
    maxValIdx,
  };
}
```

For the main part of the function, we just need to call this helper 3 times. During each iteration, we'll keep track of the index we need to skip the next time around, as well as the largest value we found.

```js
function findBigThree(numbers) {
  const largest = [];
  const skipIdxs = [];
  let count = 0;

  while (count < 3) {
    const { maxVal, maxValIdx } = getLargestNumber(numbers, skipIdxs);
    skipIdxs.push(maxValIdx);
    largest.unshift(maxVal);
    count++;
  }

  return largest;
}
```

Since we loop through all the numbers 3 times, the time complexity of this solution is `O(3n)` or `O(n)`. While this solution is technically linear, it is inefficient considering we have to loop through the array 3 times. It is possible to do this task in one pass, so let's take a look at how that can be done.

## Solution 2: The single pass

Some of this code will look a little familiar from the previous solution, but the technique we use it quite different.

Essentially what we'll do is keep a running sort of the three largest numbers we've encountered so far, and to keep the time and space complexity in check, we'll chop of the first value any time the array gets larger than 3.

This will ensure that we find the 3 largest values and that during our placement of each value, we are always dealing with a fixed time operation of comparing up to 3 values.

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

We could try to optimize the placement logic by utilizing a binary search technique, but there are only 3 values, so it is not going to affect time complexity in any meaningful way, although we could save one check each time.

## Conclusion

Hopefully by now you've gotten a lot more comfortable with utilizing a mix of for loops and while loops, and also making use of reference pointers of all kinds to keep track of where you are and what you need in order to make various logical decisions within your algorithms.

These techniques are incredibly powerful and allow you to solve an ever greater number of problems, so even if you aren't quite comfortable with it yet, trust me that with more practice it will become increasingly natural for you to reach for these techniques and the benefits they unlock.

## Other solutions

## The triple variable - [mwhitman189](https://github.com/mwhitman189)

This solution sets variables for the three highest values, and updates them
as a new high value is encountered. This optimizes the solution by eliminating the need for an inner while loop as we iterate through the numbers in the array.

```js
function findBigThree(array) {
  let highest = 0;
  let nextHighest = 0;
  let thirdHighest = 0;

  for (const num of array) {
    if (num > highest || highest === 0) {
      thirdHighest = nextHighest;
      nextHighest = highest;
      highest = num;
    } else if (num > nextHighest || nextHighest === 0) {
      thirdHighest = nextHighest;
      nextHighest = num;
    } else if (num > thirdHighest || thirdHighest === 0) {
      thirdHighest = num;
    }
  }

  return [thirdHighest, nextHighest, highest];
}
```

You can also do this by initializing an array of length 3 (shoutout to [shin10kudev](https://github.com/shin10kudev) for suggesting this over using three separate variables). The three `null` values are placeholders for the three highest values, and we update them in a similar way to the previous answer whenever we encounter a new higher value.

```js
function findBigThree(array) {
  const largest = [null, null, null];

  for (const num of array) {
    if (num > largest[2] || !largest[2]) {
      largest[0] = largest[1];
      largest[1] = largest[2];
      largest[2] = num;
    } else if (num > largest[1] || !largest[1]) {
      largest[0] = largest[1];
      largest[1] = num;
    } else if (num > largest[0] || !largest[0]) {
      largest[0] = num;
    }
  }

  return largest;
}
```

One consideration would be how we could adapt this if asked to make the number of returned highest values dynamic (e.g. `getBigKthNumbers(array, k)`, where `k` could be any value from 0 to `n`).

This is not necessarily an issue given the stated problem, however it is good to think about how we can adapt our solutions if the problem is ever changed.
