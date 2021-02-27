# Solution

Let's explore several different solutions, and think about what techniques we use to optimize the space and time complexity.

## Solution 1: Use additional arrays

I think one of the easiest solutions to this problem is to sort the zeros and non-zeros into separate arrays and then merge the arrays at the end before returning.

```js
function zeroToHero(numbers) {
  const zeros = [];
  const nonZeros = [];

  for (let i = 0; i < numbers.length; i++) {
    const val = numbers[i];
    if (val === 0) {
      zeros.push(val);
    } else {
      nonZeros.push(val);
    }
  }

  return [...nonZeros, ...zeros];
}
```

This solution will use linear O(n) time, but the space is O(2n), since we have to allocate memory for the `zeros` and `nonZeros` array, which combined will be equal to the size of `n`, and when we return the final output, we're actually creating a new array of length `n`.

This is not bad for a first-pass solution, because we have something to work with, but we can do better in terms of space complexity.

## Solution 2: Swap with items at end of the array

This 2nd solution is a popular approach to this problem. By keeping a pointer referencing the value at the end of the array, when we encounter a zero, we can swap the values and then move our end pointer to the left by 1.

However, one thing to watch out for with this solution is the case where your end pointer is pointing at another zero, like in the case of `[0, 1, 0]`. If you don't pay attention to this, then you'll end up swapping two zeros, and you'll be back to square, well -- square zero.

Let's see how we can overcome this:

```js
function zeroToHero(numbers) {
  let endIdx = numbers.length - 1;

  for (let i = 0; i < endIdx; i++) {
    while (numbers[endIdx] === 0 && endIdx > i) {
      endIdx--;
    }

    const curr = numbers[i];

    if (curr === 0) {
      const tail = numbers[endIdx];
      numbers[i] = tail;
      numbers[endIdx] = curr;
      // Pre-emptively move 1 left to avoid entering the while loop above at least once
      endIdx--;
    }
  }

  return numbers;
}
```

As you can see, at each iteration I'm checking if my end pointer is pointing at a zero, and, if so, I move the `endIdx` to the left, with this block here:

```js
while (numbers[endIdx] === 0 && endIdx > i) {
  endIdx--;
}
```

It may appear that by adding this nested while loop that we are increasing the time complexity of this algorithm. However, the `endIdx` pointer is gradually moving to the left as we replace the items at the end of the array with zeros, and even with these minor nudges we will only end up touching all of the items in the array once in the worst-case scenario.

With this, we are able to maintain linear time, but also improve our answer to constant space complexity O(1), since we solving this problem in place, and not initializing any additional arrays.

One caveat is that this solution will also end up changing the order of the non-zero items in the array. Is there a way we can avoid that, too?

## Solution 3: Using splice() and push()

In this solution we can do linear time with constant space while also preserving the order of the non-zero items:

```js
function zeroToHero(numbers) {
  let endIdx = numbers.length - 1;

  for (let i = 0; i < endIdx; i++) {
    if (numbers[i] === 0) {
      numbers.splice(i, 1);
      numbers.push(0);
      i--; // The index has shifted so take one step back
      endIdx--; // End value will be a zero, so move to the left
    }
  }

  return numbers;
}
```

Basically, when we encounter a zero, we cut it out of the array, and then push it to the array's end. When we do this, our index position gets offset by one, so we need to do `i--`, which will let us take one step back.

Like the previous solution, I think it is good to update the end pointer one in the case where we add a zero to the end of the array, so we don't end up iterating over values we know to be zeros already.

## Conclusion

I hope you had as much fun with this problem as I did! Problems that have so many potential solutions are great for helping to practice techniques you may not usually apply, so whenever you come accross a relatively easy problem, be sure to come up with as many solutions you can think of to get the most out of it.
