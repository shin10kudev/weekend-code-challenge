# Solution

## Other solutions - [mwhitman189](https://github.com/mwhitman189)
This seems like a problem screaming for '.reduce', so here it is:

```js
function getPhotos(arrObject) {
  const photos = arrObject.reduce((acc, curr) => {
    acc.push(curr['large']);
    return acc;
  }, []);
  return photos;
}
```

This solution sets an initial accumulator value of an empty array, and when each object in the original array is encountered, the 'large' key's value is pushed to the accumulator.
