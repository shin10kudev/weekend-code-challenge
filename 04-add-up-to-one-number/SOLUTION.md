# Solution

I know I said there is a practical application for this algorithm -- and there is! It can be used to create a game where, given a target value and a selection of random numbers, the user can try to find the combination of numbers to reach the target value.

Of course we need to make sure that there is a possible route to the target, and also make it a challenge by throwing in a few random numbers. Thus we have the requirement to generate a random array of numbers, where some of those numbers can be added up to get the target value.

## Helper methods

To start, I want to create a simple helper function for generating a random number, which will prove useful when working on various solutions.

```js
const getRandNum = (max = 100) => Math.floor(Math.random() * max) + 1;
```

Now, let's get into the solutions.

## Solution 1: Subtraction method

Although it is counterintuitive given the name of the problem, the first method I'll introduce will involve substracting bit by bit from the target value until we have an array of random values that will definitely add up to the target value, possibly with a few extra random numbers to throw the user off.

```js
const addUpToOneNumber = (targetValue, arrayLength = 6) => {
  const output = [];

  Array.from({ length: arrayLength }).reduce((acc, curr, index) => {
    const randNum = getRandNum(targetValue / 2);
    const remainder = acc - randNum;

    if ((remainder > 0 && index < arrayLength - 1) || acc === 0) {
      output.push(randNum);
    } else {
      output.push(acc);
    }

    return remainder;
  }, targetValue);

  return output;
};
```

The reason this works is because of the block of code right here:

```js
if ((remainder > 0 && index < arrayLength - 1) || acc === 0) {
  output.push(randNum);
} else {
  output.push(acc);
}
```

I will push a random number to the array as long as the `acc` is greater than zero and there is at least one more number to add to the array. The other case is where the `acc` has reached zero, meaning I've generated a path to the targetValue. Why do I still add a random number to the array? Well, because I still need to add some more random numbers to the array -- although I no longer have to worry if they will help me reach the target value or not.

Otherwise, I will push the current value of `acc`, which will always be the ramaining value I need in order to get the target value.

Other than that, there are a few takeaways from this method that I'd like to point out.

### Takeaways

- `.reduce(...` | I decided to use `.reduce()` because of the way it allows us to keep track of a changing value, but I could have just as easily used `.map()`. In that case I wouldn't need the `output` array, but I would need to initialize a `count` variable to keep track of the remainder.
- `arrayLength = 6` | As you may know, you can set a default value to a function argument. Although the problem doesn't ask for this argument, since it is used as a constant within the function, I thought it would be good to make this a parameter anyway.
- `Array.from({ length: arrayLength }).` | This is a quick way to initialize an empty array with a set length. Combining this with `.map()` or `.reduce()` is a handy technique to keep in mind for future programming needs.

## Solution 2: Addition method

This solution will do the opposite of what is done in the first method: start with zero and add up along the way.

The algorithm ends in a similar fashion -- that is, by calculating the last number needed to reach the target value. In all other cases, we can simply add the new random number to the array.

```js
const addUpToOneNumber = (targetValue, arrayLength = 6) => {
  let runningTotal = 0;

  return Array.from({ length: arrayLength }).map((_, index) => {
    const randNum = getRandNum(targetValue / 2);
    runningTotal += randNum;

    if (
      runningTotal > targetValue ||
      (index === arrayLength - 1 && runningTotal < targetValue)
    ) {
      return targetValue - (runningTotal - randNum);
    }

    return randNum;
  });
};
```

It may seem like `runningTotal > targetValue` is the only condition we need to watch for. However, there is one edge case where the first 5 values do not add up to the target value. In this case we only have 1 last chance to provide the number that will allow us to reach the target value.

Similar to my comment earlier, I could have easily used `.reduce()` instead here, but I went with map to show how that would look.

### Solution 3: Find the missing number

Another solution that comes to mind, which I will save for you to try to code out, is perhaps a bit more expedient. Basically you could generate 6 random numbers, then go through and see which one or two numbers you would need to change to generate the target value.

## Validation helpers

I said in the problem statement that it would be a good idea to create some helper functions to validate and test your solutions, and these are the helpers I came up with.

```js
const validateAnswer = (result, targetValue) => {
  let count = 0;

  for (let j = 0; j <= result.length - 1; j++) {
    count = count + result[j];

    if (count === targetValue) return true;
  }

  return false;
};
```

This method will iterate through the array and return `true` if the target value is reachable by summing the values.

One caveat is about methodology: this validation process won't detect dynamic routes to the target value. This is because in both solutions, I'm using a bottom up approach, so I know that I will always be able to reach an answer by summing the values left to right.

A second piece to this validation, is to be able to test the answer many times to see if it ever fails. We can use a for-loop for that, and keep track of the result of calling our method each time.

```js
const testResult = (count = 10) => {
  let testResult = {
    pass: 0,
    fail: 0,
  };

  for (let i = 0; i < count; i++) {
    const result = addUpToOneNumber(target);
    const isValid = validateAnswer(result, target);

    if (isValid) {
      testResult['pass']++;
    } else {
      testResult['fail']++;
    }
  }

  return testResult;
};

testResult(1000);
```

If you wanted this method to fail fast and give you the array of numbers it failed on, you could modify the logic slightly as follows:

```js
  let testResult = {
    pass: 0,
    fail: null,
  };

  ...
  else {
    testResult['fail'] = result;
    return testResult
  }
```

## Conclusion

Hopefully this problem gave you a chance to stretch your JavaScript skills and your imagination in new, unforseen directions. Algorithms, well programming in general, are part skill, part art. And giving yourself a moment to think about the various solutions and tehniques are a perfect opportunity to grow.
