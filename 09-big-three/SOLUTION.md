# Solution

Let's take a look at a brute force solution and a more optimal one.

## Solution 1: The brute force approach

One way we could certainly go about solving this problem is to go through the array 3 times. During each pass, we would need to find the current largest number, and we'd also need to handle the case where there are duplicates of the largest numbers.

Rather than keeping track of the count of duplicate large numbers, which could get really confusing, we could keep track of the index of where we found the largest number then simply remove it from the array so as not to encounter it again on the second pass.

Finally, we'd need to do a quick sort of the array of largest numbers before we return it.

Some may have opted to write out 3 for loops, but I think it is a bit cleaner to use a while loop, like this:

```js
function findBigThree(array) {
  const largest = [];
  let index = 0;
  let currMax = array[0];
  let currMaxIdx = 0;

  while(largest.length < 3) {
    let val = array[index];

    if(val > currMax) {
      currMax = val;
      currMaxIdx = index;
    }

    index++;

    if(index > array.length) {
      largest.push(currMax);
      array.splice(currMaxIdx, 1);
      index = 0;
      currMaxIdx = 0;
      currMax = array[0];
    }
  }

  return largest.sort((a, b) => a - b);
}
```

There are quite a few variables we need to keep track of, but it does get the job done. The time complexity of this solution would be `O(3n)` or `O(n)`, since we need to loop through the array 3 times to get the answer. The final sort will only be of 3 numbers, and it is not significant in terms of its impact on time complexity.

While this solution is linear, it is possible to do this in one pass. Let's take a look at how that would be done.

## Solution 2: The single pass

Some of this code will look a little familiar from the previous solution, but the technique we use it quite different.

Essentially what we'll do is sort the array as we go along and any time the array gets larger than 3, we'll chop of the first value. This will ensure that we find the 3 largest values and that during our placement of each value, we are always dealing with a fixed time operation of comparing up to 3 values.

```js
function findBigThree(array) {
  const largest = [];

  for (let i = 0; i < array.length; i++) {
    let index = 0;
    let val = array[i];

    while (index < largest.length) {
      if (val < largest[index]) break;
      index++;
    }

    largest.splice(index, 0, val);

    if (largest.length > 3) {
      largest.shift();
    }
  }

  return largest;
}
```

We could optimize the placement logic by utilizing a binary search sort of technique, but there are only 3 values, so it is not going to affect time complexity in any meaningful way.

With this, we have a linear solution and we only had to make one pass.

## Conclusion

Hopefully, by now you have gotten a lot more comfortable with utilizing a mix of for loops and while loops, and also making use of reference pointers of all kinds to keep track of where you are and what you need in order to make various logical decisions within your algorithms.

These techniques are incredibly powerful and allow you to solve an ever greater number of problems, so even if you aren't quite comfortable with it yet, trust me that with more practice, it will become more and more natural for you to reach for these techniques.
