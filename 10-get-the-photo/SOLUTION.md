# Solution

TBD

## Other solutions

## The reduce method - (mwhitman189)[https://github.com/mwhitman189]

The JS 'reduce' method suits this problem well:
```js
function getLargePhotos1(photos) {
  return photos.reduce((acc, curr) => {
    const { large } = curr;
    return [...acc, large ];
  }, []);
}
```

However, as [shin10kudev](https://github.com/shin10kudev) pointed out, this can be cleaned up using object destructuring:
```js
function getLargePhotos(photos) {
  return photos.reduce((acc, { large }) => {
    acc.push(large);
    return acc;
  }, []);
}
```

Or even further using a one line solution with implicit return (thanks [shin10kudev](https://github.com/shin10kudev)):
```js
const getLargePhotos = (photos) =>
  photos.reduce((acc, { large }) => [...acc, large], []);
```
Something to notice is that in this solution, 'acc.push()' will not work, because the 'acc.push()' would be returned, rather than the 'acc'.
