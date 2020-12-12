## Problem

Given an array of posts and an array of your likes, how could you effeciently check if you liked a particular post or not?

## Example data

```js
const posts = [
  {
    id: "RrnuzUt7kFLrK9NOW--p4",
    authorId: 1,
    content: 'abcd',
  },
  {
    id: "iP7DBEcWgtyGS1gm69ndw",
    content: 'efg',
    authorId: 10,
  }
  ...
];

const likes = [
  {id: "GKePk5jyMvq4_WK_bspQg", postId: "RrnuzUt7kFLrK9NOW--p4"},
  {id: "jrW6r4Ta2Z4ftNRB2oaow", postId: "c4lnwlqqNosRXcuTFumH3"},
  {id: "gwyuvYhZp0NklcvYo5uQB", postId: "DGNynY1-V2pYDZqYf__Vg"},
];
```

### Notes

- The `posts` array may contain posts from various users
- The `likes` array only contains **your** likes
- It's possible that many different users can like the same post
