# Solution

## Solution 1: Use a hash

Time complexity would be O(2n) or O(n) if we drop the constants
Space complexity will be ??

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

## Solution 2: Sort and then look ahead

Time complexity will be O(nlog(n)) because we are sorting the string first
Space complexity will be O(n) because we need to convert the string to array for sorting

```js
function firstNonRepeatingChar(str) {
  const characters = str.split('');
  characters.sort();

  let refChar = characters[0];
  let matches = 0;

  for (let i = 1; i < characters.length; i++) {
    let currChar = characters[i];

    if (refChar === currChar) {
      matches++;
    }

    if (refChar !== currChar) {
      if (matches > 0) {
        matches = 0;
        refChar = currChar;
      } else {
        return refChar;
      }
    }
  }

  return -1;
}
```
