---
title: Advanced JavaScript
date: 2019-09-15 16:12:15
tags: [JavaScript]
---
# Question 1
```js
['1', '2', '3'].map(parseInt)

what & why ?
```

The `map()` method creates a new array with the results of calling a provided function on every element in the calling array. See more in [Array.prototype.map() | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

```js
const arr = [1, 2, 3];
arr.map((num) => num + 1); // [2, 3, 4]
```

For each iteration map, `parseInt()` passes two parameters: string and index. So the code actually executed is:
```js
['1', '2', '3'].map((item, index) => {
	return parseInt(item, index)
})
```
This will return
```js
parseInt('1', 0) // 1
parseInt('2', 1) // NaN
parseInt('3', 2) // NaN, 3 cannot be divided by 2
```
So:
```js
['1', '2', '3'].map(parseInt)
// 1, NaN, NaN
```
In order to achieve the function we want, we could do this:
```js
['1', '2', '3'].map(Number)

OR

['1','2','3'].map(n => parseInt(n,10))
```
