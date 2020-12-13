## Solution

There are two solutions I'd like to share. One is a kind of brute-force, quick and dirty answer. Whereas the other solution breaks the problem down into 2 steps, providing a much more optimal one.

## Solution 1: Quick and dirty

Assuming you are already looping through the array of posts to display them on a page, for each post, you can loop through the likes array and see if the current post id is found in the array.

```js
{posts.map(({ id: currentPostId, content }) => {
  const isLiked = likes.find(({ postId }) => currentPostId === postId);
  ...
})}
```

As you can see, for each post you are looping through the array of likes again and again, which is costly in terms of the number of duplicate operations that need to be performed. How can we optimize this?

## Solution 2: Generate a hash

The other solution is to use the likes array to generate a hash map of postIds. Although this requires looping through the likes data once, after that, you can use the hash to give you constant look-up time when checking if the post is liked or not.

First, generating the hash map:

```js
const myLikes = likes.reduce((acc, curr) => {
  return {
    ...acc,
    [curr.postId]: curr.id,
  };
}, {});
```

This will give you a hash map which uses the `postId` as the key, and the `likeId` as the value:

```js
const myLikes = {
  1: 456,
  2: 789,
  10: 888,
};
```

Second, now you can use this hash map to quickly look up if the current post is liked or not.

```js
{posts.map(({ id: currentPostId, content }) => {
  const isLiked = myLikes[currentId];
  ...
})}

```

Assuming, you don't have any postIds of `0`, if the key exists in the `myLikes` has, it will return `truthy` value. Otherwise it will return `undefined`.

## Comparing solutions

For the first solution, the number of repeated operations is quite large, because for each post you have to also loop through the array of likes. Worst case scenario, none of the posts exist in the likes array, but you'd still have to loop through the entire array each time to the end of the array for each post. If the data set increased, that could be pretty costly.

For the second solution, things are much better. You still have to loop through the likes array once in order to generate the hash map. But after that, you benefit from constant look-up time when you check to see if the current post id exists in the hash map or not.

Certainly there are other ways to optimize this even more. Can you think of any other ways to solve it?
