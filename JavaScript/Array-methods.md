## Join
> The `Join()` method creatres and returns a new striing by concatenating all of the elements in an array(or an array-like object),
> separated by commas or a specified separator string. If the array has only one item, then that item will be returned without using the separator. 

```javascript
const elements = ['Fire', 'Air', 'Water']

console.log(elements.join());
//The result would be : "Fire, Air, Water"
console.log(elements.join(''));
//The result would be : "FireAirWater"
console.log(elements.join('-'));
//The result would be : "Fire-Air-Water"
```

<br />

## Concat 
> The `concat()` method is used to merge two or more arrays. This method `does not change the existing arrays,` but instead returns a new array. 

```js 
const array1 = ['a', 'b', 'c']
const array2 = ['d', 'e', 'f']
const array3 = array1.concat(array2)

console.log(array3)
//The result would be : ['a', 'b', 'c', 'd', 'e', 'f']
```

<br />

## Reverse 
> The `reverse()` method reverses an array `in place`. The first array element becomes the last, and the last array element becomes the first. 

```js 
const array1 = ['apple', 'banana', 'grape']
const reversed = array1.reverse()

console.log(reversed)
//The result would be : ['grape', 'banana' 'apple']
```

<br />

## Splice 
> The `splice()` method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place. 
> To access part of an array without modifying it, see `slice()`

```js 
//Case 1 : Remove zero elements before index 2, and insert new element 
let animals = ['dog', 'cat', 'rabbit', 'cow']
let update = animals.splice(2,0,'fox')

console.log(animals)
//The result would be : ['dog', 'cat', 'fox', 'rabbit', 'cow'] 

//Case 2 : Remove 1 element at index 3 
let removed = animals.splice(3,1)

console.log(animals)
//The result would be : ['dog', 'cat', 'fox', 'cow'] 
//The removed element is 'rabbit' which was at index 3 

//Case 3 : Remove 2 elements from index -2 
let deleted = animals.splice(-2, 2) 

console.log(deleted)
//The result would be : ['dog','cow']
//The removed elements are 'fox', 'cat' 

//Case 4 : Remove 2 2 elements from index 0, and insert new elements 
let removeAndAdd = animals.splice(0,2,'frog','horse')

console.log(animals)
//The result would be : ['frog', 'horse']
```

<br />

## Slice
> The `slice()` method returns a shallow copy of a portion of an array into a new array object selected from start to end(end not included)
> where start and end represent the index of items in that array. `The original array will not be modified.`
> A negative index can be used, indicating an offset from the end of the sequence. slice(2, -1) extracts the third element through the second-to-last element in the sequence.

```js
const animals = ['camel', 'duck', 'elephant', 'monkey', 'snake']

console.log(animals.slice(2))
//The result would be : ['elephant', 'monkey', 'snake']
// If it was -2, the result would be : ['monkey', 'snake'] **

console.log(animals.slice(2, 4))
//The result would be : ['elephant', 'monkey']
// * Since The end is not included *

console.log(animals.slice(2, -1))
// The result would be : ['elephant', 'monkey'] 
// The new array starts with from index 2 and ends -1 index which is snake, but the last one is not included. so the result is ['elephant', 'monkey'] 
```



