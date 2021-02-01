# Solution

This problem presents a great opportunity to think not only about how to generate a brute force approach, but also about analyzing time complexity, and how it can leave you with some surprising results.

I will go through the brute force method of solving this problem first, then try to get to a linear solution.

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

  for (let i = 1; i <= number; i++) {
    for (let j = i + 1; j <= number; j++) {
      output.push([i, j]);
    }
  }

  return output;
};
```

> Kudos to [mwhitman189](https://github.com/mwhitman189) for helping me realize that we don't need to generate an array of consecutive numbers to run the for loop. This helps us improve space complexity, which is why I think for loops are favored in this brute force approach.

This will return the expected output of `[[1, 2], [1, 3], [1, 4]...]`. Note that if we wanted, we could check the length of the returned array, and it would give us the number of unique 2-digit combinations for the given number, which is pretty cool.

When analyzing this solution, a question that quickly arises is if the nested for loop is quadratic -- an O(n^2) answer, as it were. Notably, as we iterate through the first loop, the number of iterations required by the second for loop decreases by `1` until we reach `0`. Although this seems significant, in the grand scheme of things this solution is O(n^2). See [this stackoverflow question](https://stackoverflow.com/questions/362059/what-is-the-big-o-of-a-nested-loop-where-number-of-iterations-in-the-inner-loop) for an interesting discussion about this.

So that makes me wonder, is there a way to solve this problem in a single pass?

## Solution 2: The linear solution

In order to find a linear solution, I tried to examine my output again and see what other data structures I may be able to use. Looking at each number, one could list the possible combinations like so:

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

The second for loop goes through that array of combinations and generates our output. The code becomes a bit tricky, but looks like this:

```js
const findTheCombinations2 = (number) => {
  const combinations = [];
  const countOfCombinations = [];
  const output = [];
  const numbers = Array.from({ length: number }, (_, i) => i + 1);

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

Running this several times for each of the functions, I got a pretty consistent reading. Whereas my second solution averaged about `25` milliseconds, my first solution averaged around `15` milliseconds, which was a little surprising! Nested for loops are supposed to be slow, right?

But before tugging on this thead more, I should mention that both of these functions require very heavy operations, and if you enter a number like `10,000`, it will be very slow, and may even crash the browser tab. Why is that? It's because calculating the number of unique combinations for a given number involves factorials.

Specifically, we can calculate this number with the equation `n! / 2!(n - 2)!`, where `n` is the given number, and `2` is the max number of combinations considered. So, for the number 5, we would do `5 * 4 * 3 * 2 * 1 / (2 * 1 * 3 * 2 * 1)`, to get `120 / 12` -- `10`.

This number increases linearly, but can get quite large. For an input of `100` we would need to generate an array of `4,950` items. For `100,000` it would be `4,999,950,000`. So you can imagine why Chrome may not want to keep going.

I think there are a number of factors that explain why the first solution runs faster, but it seems to me that in the second solution we have to loop over an array that is `n! / 2!(n - 2)!`.

## Conclusion

This was a very interesting problem and got me to study many concepts in detail that I may have forgotten about or maybe only understood in passing. I wasn't able to come up with a better solution than these two, however, so I'm very curious to know if others have a different answer.

I think these solutions clearly illustrate that time complexity is not necessarily an indicator for faster program speed when evaluating fixed inputs that are relatively small. But there is another point here as well.

Although using a nested for loop can often mean an O(n^2) solution, and two for loops are often simplified to be O(n), like conflating speed and time complexity, this thinking is also not accurate. That's because the technique you use doesn't map 1-to-1 with a certain time complexity even if there is a seeming pattern between the two. Evaluating time complexity comes from looking at the relationship between how the runtime increases relative to the size of the input.

Both of these points are really important to keep in mind when assessing your algorithm solutions, and which kinds of solutions you try to pursue.
