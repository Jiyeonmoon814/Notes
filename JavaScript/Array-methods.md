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

## Flat 
> The `flat(depth)` method creates a new array with all sub-array elements concatenated into it
> recursively up to the specified depth. The depth level specifying how deep a nested array structure should be flattened
> and its default to 1. It returns a new array with the sub-array elements concatenated into it. 

```js
const arr1 = [0, 1, 2, [3, 4]]
const arr2 = [0, 1, 2, [[[3, 4]]]]
const arr3 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]]
const arr4 = [1, 2, , 4, 5]

console.log(arr1.flat())
// expected output : [0, 1, 2, 3, 4]

console.log(arr2.flat(2))
// expected output : [0, 1, 2, [3, 4]]

arr3.flat(Infinity);
// expected output : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// *** The flat method removes empty slots in arrays 
arr4.flat() 
// expected output : [1, 2, 4, 5]
```

#### Alternative 1 : reduce and concat 

```js
const arr = [0, 1, 2, [3, 4]]

// To flat single level array 
arr.flat()

// is equivalent to
arr.reduce((acc, val) => acc.concat(val), []) // [1, 2, 3, 4]

// or with decomposition syntax
const flattened = arr => [].concat(...arr)
```

#### Alternative 2 : reduce + concat + isArray + recursivity 

```js
const arr = [1, 2, [3, 4, [5, 6]]]

// to enable deep level flatten use recursion with reduce and concat 
function flatDeep(arr, d = 1) {
  return d > 0 ? arr.reduce((acc, val) => acc.concat(Array.isArray(val) ? 
          flatDeep(val, d - 1) : val), [])
  }
  
  flatDeep(arr, Infinity)
  // Expected output : [1, 2, 3, 4, 5, 6] 
```

#### Alternatvie 3 : Use a stack 
> Non recursive flatten deep using a stack. Note that depth control is hard/inefficient 
> as we will need to tag EACH value its own depth also possible w/o reversing on shift/unshift,
> but array OPs on the end tends to be faster 

```js
function flatten(input) {
  const stack = [...input]
  const res = []
  
  while(stack.length){
    // pop value from stack 
    const next = stack.pop()
    if(Array.isArray(next)) {
      // push back array items, won't modify the original input 
      stack.push(...next)
    }else{
      res.push(next)
    }
  }
  
  // reverse to restore input order 
  return res.reverse()
}

const arr = [1, 2, [3, 4, [5, 6]]
flatten(arr)
```

<br />

## Shift
> The `shift()` method removes the first element from an array and returns that removed element.
> If the array is empty, it will return undefined. This method changes the length of the array.

```js 
const arr1 = [1, 2, 3]

const firstEl = arr1.shift()

console.log(firstEl) // 1 
console.log(arr1) // [2, 3]
```

#### Using shift() method in while loop 
> The shift() method is often used in condition inside while loop. In the following example every iteration
> will remove the next element from an array, until it's empty. 

```js 
var fruits = ['apple', 'orange', 'melon', 'watermelon']

while( typeof (i = fruits.shift()) !== 'undefined') {
  console.log(i)
}

// apple, orange, melon, watermelon 
```

<br />

## Unshift
> The `unshift()` method adds one or more elements to the beginning of an array and returns the new length of the array. 

```js
const arr = [1, 2, 3]

arr.unshift(4)
arr.unshift(5) // expected output is 5 which is the new length of the array 

console.log(arr) // [5, 4, 1, 2, 3] 

arr.unshift(-2, -1) // arr is [-2, -1, 5, 4, 1, 2, 3] 
arr.unshift([-3, -4]) // arr is [[-3, -4], -2, -1, 5, 4, 1, 2, 3] and new length is 8 
```

<br />

## indexOf 
> The `indexOf(searchEl, fromIndex)` method returns the first index at which a given element can be found in the array, 
> or -1 if it is not present. <br />
> fromIndex is the index to start the search at. If the index is greather than or equal to the array's length, -1 is returned,
> which means the array will not be searched. If the provided index value is a negative number, 
> it is taken as the offset from the end of the array. 💡 Note : If the provided index is negative, the array is still 
> searched from front to back. If the provided index is 0, then the whole array will be searched. Default : 0 

```js 
const colours = ['red', 'orange', 'yellow', 'green']

console.log(colours.indexOf('yellow'))
// expected output : 2 

console.log(colours.indexOf('blue'))
// expected output : -1 

console.log(colours.indexOf('red',-1))
// expected output : -1, since it finds element from index -1 which is 'yellow' , so it returns -1 

console.log(colours.indexOf('red',-3))
// expected output : 0 

```

<br />

## findIndex 
> The findIndex() method returns the index of the first element in the array that satisfies the provided testing function. 
> Otherwise, it returns -1, indicating that no element passed the test. 

```js 
const array = [5, 2, 8, 30, 7]

const isLargerNum = (ele) => ele > 10 

console.log(array.findIndex(isLargerNum))
// expected output : 3 

// find Index using arrow function 
const fruits = ['apple', 'oragne', 'banana', 'melon'] 

const index = fruits.findIndex(fruit => fruit === 'orange')

console.log(index) // 1 
console.log(fruits[index]) // orange 

// find a prime number in an array 
const arr2 = [4, 6, 8, 9, 12]
const arr3 = [4, 6, 7, 9, 12]

const isPrime = num => {
  for(let i=2; num > i; i++) {
    if(num % i === 0) {
      return false
    }
  }
  
  return num > 1 
}

console.log(arr2.findIndex(isPrime)) // -1, not found
console.log(arr3.findIndex(isPrime)) // 2, arr3[2] is 7 which is a prime number 

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



