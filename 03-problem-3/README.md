# Problem

This is going to be a data structure problem, the kind of problem we often encounter with the data we have: namely, that the data structure is not in the format that we want! 😡 So, what to do?

Given the following data:

```js
data = {
  something: { title: 'something', copy: 'something', link: 'something' },
  another: { title: 'another', copy: 'another', link: 'another' },
};
```

Create a helper function that will allow you to conveniently convert your data into the following format:

```js
output = [
  {
    id: 'someting',
    title: 'something',
    copy: 'something',
    link: 'something',
  },
  {
    id: 'another',
    title: 'something',
    copy: 'something',
    link: 'something',
  },
];
```

The goal of this exercise is to convert your data into an array of objects where the `id` key for each object maps to the keys of the original object.

## Notes

- As the original data is an object, all keys will be unique.
- The data could be incredibly long or short, so be mindful, as always, of time complexity.

## Tips

- There are many different ways you can solve this problem, and it is useful to explore each
- Try to consider a multitude of solutions, get them working and then think about what you like about each answer
- After you have your answer, think about how you can clean it up. That is, how can you make the code more readable while also being concise?
