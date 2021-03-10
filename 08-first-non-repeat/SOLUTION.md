# Solution

Let's take a look at how we can solve this problem.

## Solution 1: Brute force method

A brute force approach to this problem is to use two for loops and for each value in the string search the entire string again to see if there are other duplicates.

If we do this, we'll end up making a lot of duplicate comparisions and a solution that is O(n^2).

To optimize this solution, we can take advantage of memoization and construct a hash table of seen values for each item we encounter. This way, if we encounter a value we've already seen before, we can skip ahead to the next character instead of making a redundant 2nd pass through the string.

The solution would look like this:

```js
function firstNonRepeatingChar(str) {
  const seen = {};

  for (let i = 0; i < str.length; i++) {
    let currChar = str[i];

    // memoize to avoid checking rest of string if already seen char
    if (!seen[currChar]) {
      seen[currChar] = true;
    } else {
      continue;
    }

    for (let j = 0; j < str.length; j++) {
      if (j === i) continue;
      let compChar = str[j];

      if (currChar === compChar) break;

      if (j === str.length - 1) return currChar;
    }

    if (i === str.length - 1) return currChar;
  }

  return -1;
}
```

This memoization technique does more for us than simply optimizting the solution, it actually improves the time complexity significantly so that we are not dealing with an exponential solution anymore.

## Solution 2: Use a hash and a loop through twice

The first solution that comes to mind is the idea of keeping track of how many times youve encountered each of the values in the string in a hash table and then checking to see if there is a case where a character was only seen once.

One pitfall to this approach is looping through the string first and then looping through the hash table and returning the first entry that has a value equal to 1. The problem with that approach is that hash tables do not neccessarily preserve the order in which items appear in the string, so you'll end up returning the wrong value in some cases.

The solution to that is simple: After constructing the hash table, simply loop through the string again and lookup the count of encounters for each character. When you find a character that occurred once, you can return that value and that is your answer.

```js
function firstNonRepeatingChar(string) {
  const seen = {};
  for (let i = 0; i < string.length; i++) {
    const char = string[i];

    if (!seen[char]) {
      seen[char] = 1;
    } else {
      seen[char]++;
    }
  }

  for (let i = 0; i < string.length; i++) {
    const char = string[i];

    if (seen[char] === 1) return char;
  }

  return -1;
}
```

The time complexity for this solution would be O(2n) or O(n) if we simplify.
Space complexity will be a bit complicated because we are only storing one instance of a value, so even if `n` were `1,000`, if all the entries were `a` we'd only be storing 1 additional value. So average space complexity would be a calculation of n over how many duplicates of each value exist. But worst case, every single item would be a unique value, so worst case space complexity would be `O(n)`.
