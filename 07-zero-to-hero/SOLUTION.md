# Solution

Let's explore several possible solutions and think about what techniques we can use to optimize the space and time complexity with each attempt.

## Solution 1: Use additional arrays

One of the easiest solutions to this problem is to use some additional space to sort the numbers into two groups: zeros and non-zeros. Once that is done, you simply have to merge the two arrays, and you're done!

That would look a little something like this:

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

This solution will use linear O(n) time, but the space complexity is O(2n), since we have to allocate memory for the `zeros` and `nonZeros` array, which combined will be equal to the size of `n`, and when we return the final output, we're actually creating a new array of length `n`.

This is not bad for a first-pass solution because it gives us something to work with, but we can definitely do better in terms of space complexity.

## Solution 2: Swap with items at end of the array

This is a very popular solution to this problem. By keeping a pointer referencing the value at the end of the array, when we encounter a zero at the front, we can swap the values, and then move our ending pointer to the left by 1.

One thing to watch out for with this solution is the case where your end pointer is pointing at another zero, like in the case of `[0, 1, 0]`.

If you don't pay attention to this, then you'll end up swapping two zeros, and you'll be back to square, well...zero.

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

With this block of code here, I am moving this pointer to the left by one, until I have found a non-zero or I have moved past the front pointer, `i`:

```js
while (numbers[endIdx] === 0 && endIdx > i) {
  endIdx--;
}
```

It may appear that adding this nested while loop will increase the time complexity of this algorithm. However, the `endIdx` pointer is gradually moving to the left, ensuring we will never touch the same value more than once.

With this, we are able to maintain linear time, but also improve our answer to constant space complexity O(1), since we solving this problem in place, and not adding any additional arrays.

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

Basically, when we encounter a zero, we cut it out of the array, and then push it to the array's end. When we do this, our index position gets offset by one, so we need to do `i--`, which will allow us to take a step back.

Like the previous solution, I think it is good to move the end pointer to the left by one in the case where we add a zero to the end of the array, so we don't end up iterating over values we know to be zeros already.

Note that using `.splice()` will mutate the original array. But this is unavoidable if we are asked to do an in-place sort of the original array.

## Conclusion

This is an excellent problem to level up your skills because it strikes a great balance between having an interesting twist, being relatively easy to solve, and also a wide variety of possible answers.

Problems like this that have so many potential solutions are great for stretching your imagination and practicing a wider variety of techniques that you may usually try.

So whenever you come accross a relatively easy problem like this, be sure to come up with as many solutions you can think of to really get the most out of it.

The techniques you discover through this process will be available to you when you encounter other, even wildly different problems, and you will thank yourself for taking the time to understand these.
