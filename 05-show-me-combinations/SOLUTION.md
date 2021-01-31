# Solution

This problem presents a great opportunity to think not only about how to generate a brute force approach, but also about analyzing time complexity, and how it can leave you with some surprising results.

I will go through the brute force method of solving this problem first, then try to get to a linear solution.

## Helper methods

Since we will need to generate an array of consecutive numbers from the input parameter, it is useful to create a helper method that we can reuse in the different solutions we try:

```js
const consecutiveRangeOfNumbers = (start, stop) =>
  Array.from({ length: stop }, (_, i) => i + start);
```

I created this function with some inspiration from the [MDN Web Docs on Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from). As you can see, this method takes a `start` and `stop` parameter, and with that will return an array of numbers like `[1, 2, 3, 4, 5]`;

## Solution 1: Brute force method

I started off thinking about this problem by using a simple yet reliable technique, which is to take a sample input and examine the process of outputting the solution by hand.

The problem asks us to return all of the non-repeating combinations of integers. So, given the input of `5`, we would generate an array `[1, 2, 3, 4, 5]`, and return the following combinations:

```md
1 -> 1,2, 1,3, 1,4, 1,5
2 -> 2,3, 2,4, 2,5
3 -> 3,4, 3,5
4 -> 4,5
```

As you can see, one way to get the answer is to start with the first item in the array, and then combine it with each subsequent element, before moving on to the next item in the array. We repeat this process until we get to `5` because at that point there are no other combinations to create.

This reveals the brute force solution, which is to use a nested-for-loop. You use the first for loop to loop through each item in the array, and the second for loop to combine that number with each subsequent number, starting at `i + 1`, until reaching the end of the array.

One example of that approach looks like this:

```js
const findTheCombinations1 = (number) => {
  const output = [];
  const numbers = consecutiveRangeOfNumbers(1, number);

  for (let i = 0; i < numbers.length; i++) {
    for (let j = i + 1; j < numbers.length; j++) {
      output.push([numbers[i], numbers[j]]);
    }
  }

  return output;
};
```

We could also utilize the splice method for another interesting solution. It's really similar, however, and the difference in performance is not significant:

```js
for (let i = 1; i < numbers.length; i++) {
  const pairs = numbers.slice(i); // e.g. for index 1, this would be [2, 3, 4, 5]

  for (let j = 0; j < pairs.length; j++) {
    output.push([i, pairs[j]]);
  }
}
```

Both of these approaches will return the expected output of `[[1, 2], [1, 3], [1, 4]...]`.

Notice that the number of iterations required in the second for loop decreases to `0` as we move toward the end of the first loop, but this is a quadratic (O(n^2)) solution. See [this stackoverflow question](https://stackoverflow.com/questions/362059/what-is-the-big-o-of-a-nested-loop-where-number-of-iterations-in-the-inner-loop) for an interesting discussion about this.

So it would be nice to try to solve this problem in another way.

## Solution 2: The linear solution

In order to find a linear solution, I tried to examine my output again and see what other data structures I may be able to use. Looking at each number, one could list the possible combinations, like so:

```js
1 -> 2, 3, 4, 5
2 -> 3, 4, 5
3 -> 4, 5
4 -> 5
```

It is then possible to format this as an object, where the keys are each item in the array, and the value is an array of permitted combinations:

```js
{
  1: [2, 3, 4, 5],
  2: [3, 4, 5],
  3: [4, 5],
  4: [5],
}
```

We could also use an array of arrays, keeping in mind that to map the index to the number we need to think, `i = i + 1`. That is, index `0` actually refers to number `1`, and so on.

```js
[[2, 3, 4, 5], [3, 4, 5], [4, 5], [5]];
```

The problem with this is that with an array of arrays or an object with keys that are arrays, we are back to square one: we'd still have to do some kind of nested for loop in order to map these combinations to the expected output.

So I had the idea that I could create a flattened array where I push all the combination integers, like so:

```js
[2, 3, 4, 5, 3, 4, 5, 4, 5, 5];
```

With a flattened array, we can avoid a nested for loop. But we still need two for loops to get the whole job done.

The first for loop will go through the integers and generate 2 lists -- one which contains all the combinations for each number, and a second to keep track of the count for each number we need to combine these with (since unlike with a hash, we lose this dimension when using a flattened array).

The second for loop goes through that array of combinations and generates our output. The code becomes a bit convoluted, but looks like this:

```js
const findTheCombinations2 = (number) => {
  const combinations = [];
  const countOfCombinations = [];
  const output = [];
  const numbers = consecutiveRangeOfNumbers(1, number);

  for (let i = 1; i < numbers.length; i++) {
    let pairs = numbers.slice(i);
    countOfCombinations.push(...pairs);
    combinations.push(pairs.length);
  }

  let matchCount = combinations[0];
  let match = 1;

  for (let i = 0; i < countOfCombinations.length; i++) {
    output.push([match, countOfCombinations[i]]);
    matchCount--;

    if (matchCount === 0) {
      match++;
      matchCount = combinations[match - 1];
    }
  }

  return output;
};
```

So which of the above solutions is better? The nested for loop or the combination of for loops? The answer is not necessarily as obvious as it may seem...

## Assessing our solutions

It is tempting to conflate "time complexity" of an algorithm and the speed of a program. They are not really the same thing, and involve two completely different conversations.

In terms of time complexity, the first solution has O(n^2), whereas the second one, with much simplification, is O(n). So, from this angle, one could say that the second solution has better time complexity.

In terms of speed, though, it's a different story. To test the speed of these two functions I created a helper function that uses the [Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance) to run the program 100 times with a fixed input and calculate the average operation time in milliseconds. There are more robust and technical ways to test performance, but I wanted something quick and fast that I can control, so this is what I came up with:

```js
const averagePer = (func) => {
  let av = 0;

  for (let i = 0; i < 100; i++) {
    var start = performance.now();
    func(1000);
    var duration = performance.now() - start;

    av += duration;
  }
  return av / 100;
};
```

Running this several times for each of the functions, I got a pretty consistent reading. Whereas for my second solution averaged about `25 - 27` milliseconds, my first solution averaged around `15 - 17` milliseconds, which was quite surprising! Nested for loops are supposed to be slow!

But before talking more about this, I should mention that both of these functions require very heavy operations, and if you enter a number like `10,000`, it will be very slow, and may even crash the browser tab. Why is that? It's because calculating the number of unique combinations for a given number involves factorials.

Specifically, we can calculate this number with the equation `n! / 2!(n - 2)!`, where `n` is the given number, and `2` is the max number of combinations considered. So, for the number 5, we would do `5 * 4 * 3 * 2 * 1 / (2 * 1 * 3 * 2 * 1)`, to get `120 / 12` -- `10`.

This number increases linearly, but can get quite large. For an input of `100` we would need to generate an array of `4,950` items. For `100,000` it would be `4,999,950,000`. So you can imagine why Chrome may not want to keep going.

I think there are a number of factors that explain why the first solution runs faster, but it seems to me that in the second solution we have to loop over an array that is `n! / 2!(n - 2)!`.

## Conclusion

This was a very interesting problem and got me to study a ton of stuff I may have forgotten or only understood in passing. I wasn't able to come up with a better solution than these two, so I'm very curious to know if others have a different answer.

I think these solutions clearly illustrate that time complexity does not translate equally to speed. But there is another point here as well.

Although using a nested for loop can often mean an O(n2) solution, and two for loops are often simplified to be O(n), this thinking is also not effective. That's because when assessing time complexity, the technique you use doesn't guarantee the time complexity -- rather the way the output grows in relation to the input is what we are concerned with.

Both of these points are really important to keep in mind when assessing your algorithm solution.
