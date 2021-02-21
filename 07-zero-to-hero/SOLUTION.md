# Solution

Let's explore the various solutions and think about how we can go from our first-pass solution to something more optimized

## Solution 1: Use additional arrays

I think one of the easiest solutions to this problem is to separate the zeros from the out of the array and then add them to the end before returning.

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

This solution will be linear O(n) time, but the space is O(n), since we have to allocate memory for the `zeros` and `nonZeros` array which combined will be equal to the size of `n`. It's honestly not that bad, but we can do better in terms of space complexity.

## Solution 2: Swap with items at end of the array

This is a really popular solution to this question. By keeping a pointer, referencing the end value at the end of the array, when we encounter a zero, we can swap the values and then move the pointer 1 to the left.

One thing to watch out for with this solution is the case where your end pointer is point at another zero. Like in the case of `[0, 1, 0]`. If you don't pay attention to this, then you'll end up swapping two zeros, and you are back to square, well, `zero`.

Let's see how this can be overcome:

```js
function zeroToHero(numbers) {
  let endIdx = numbers.length - 1;

  for (let i = 0; i < numbers.length; i++) {
    if (numbers[endIdx] === 0) {
      while (numbers[endIdx] === 0 && endIdx > i) {
        endIdx--;
      }
    }

    if (i === endIdx) break;

    if (numbers[i] === 0) {
      const temp = numbers[i];
      numbers[i] = numbers[endIdx];
      numbers[endIdx] = temp;
    }
  }

  return numbers;
}
```

As you can see, before proceeding to the check if whether the current item is a zero or not, I have this block, which makes sure I'm on a `zero`.

```js
if (numbers[endIdx] === 0) {
  while (numbers[endIdx] === 0 && endIdx > i) {
    endIdx--;
  }
}
```

This will check if the current number is a zero or not and if so will move 1 to the left.
With this, we are able to maintain linear time, but also improve our answer to constant space complexity O(1), since we are working with the original array.

One caveat to this solution, is that it will also change the order of the non-zero items. Is there a way we can avoid that, too?

## Solution 3: Using splice() and push()

In this solution we can do linear time with constant space while also preserving the order of the non zero items:

```js
function zeroToHero(numbers) {
  let endingIndex = numbers.length;

  for (let i = 0; i < numbers.length; i++) {
    if (i === endingIndex) break;

    if (numbers[i] === 0) {
      numbers.splice(i, 1);
      numbers.push(0);
      i--;
      endingIndex--;
    }
  }

  return numbers;
}
```

Basically, when we encounter a zero, we cut it out of the array and then push it to the end.
When we do this, our index position gets offset by one, so we need to do `i--`, which will let us step back one.

However, we have to be careful because we can easily generate an infinite loop without keeping track of the ending index. So I utilize an `endingIndex` variable that will move to the left as we proceed. The function will stop when the two pointers meet.

## Conclusion

I hope you had fun with this one! Problems that have so many potential solutions that you can come up with quickly are great for helping you practice techniques you may not usually apply, so whenever you come accross a relatively easy problem, be sure to come up with as many solutions you can think of to get the most out of it!
