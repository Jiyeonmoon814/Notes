## 🥄 Splice 
`Array.prototype.splice(start, [deleteCount], [item1], [item2])` 

> The `splice()` method <b>changes the contents of an array by removing existing elements or adding new elements.</b> <br/>
> splice() method modifies the original array and it returns an array containing the deleted elements. 

```js
var exapmle = ['A', 'B', 'C', 'D']

example.splice(2,0,'E') // remove 0 item, and insert 'E' at 2-index 
console.log(example) // ['A', 'B', 'E', 'C', 'D'] 

var copy = example.splice(2,1) // remove 1 item at 2-index position, *return the deleted item*
console.log(copy) // return ['E'] which is the deleted item 

var copy2 = example.splice(2,0,'E')
console.log(copy2) // returns [], since there was no deleted item 
console.log(example) // returns ['A', 'B', 'E', 'C', 'D'], since splice method modify the original array 

```

#### 😵 simulate shift() with splice()

> First, the `shift()` method <b>the first element from an arry and returns that removed element.</b> This method changes the length of the array. 

```js
function shiftArray(arr) {
  // remove 1 item at 0-index position, return the deleted item 
  return arr.splice(0,1)
}

const array = [1,2,3]
const firstEl = array.shift()

console.log(firstEl) // returns 1 which was the first element of array 
console.log(array) // returns [2, 3], since the first element of the original array was removed. 

const deletedEl = shiftArray(array)

console.log(array) // returns [3]
console.log(deletedEl) // returns [2] 
```

`shift() method returns removed element, but splice() method returns an array containing the deleted elements. `

## 🔪 Slice
`Array.prototype.slice(begin,end) || String.prototype.slice(begin,end)` 
> Slice() is a method of Array Object and String Object. It's non-destructive since it returns `a new copy and doesn't change the content of input array.`
> When it's an array, The slice() method returns a shallow copy of a portion of an array into a new array object selected from `begin to end (end is not included)`. 
> The original array will not be modified. When it's a String, it extracts a secton of a string and returns it `as a new string.` 
> So slice method returns <b>an array or a string containing the extracted elements.</b>

```js 
var array = ['A', 'B', 'C', 'D'] 
var slicedArray = array.slice(1,3) // slice start from 1-index to 3-index, but * not include 3-indx * 

console.log(array) // returns ['A', 'B', 'C', 'D'] , since slice method doesn't modify the original array 
console.log(slicedArray) // retuns ['B', 'C'] 

var text = 'ABCD' 
var slicedText = text.slice(1,3) // slice process the slice as the array 

console.log(text) // 'ABCD' 
console.log(slicedText) // 'BC' 
``` 

## 🍴 Split 
`String.prototype.split([separator[, limit]])`
> The split() method splits a String object into an array of stirngs by separating the string into substrings,
> using a specified separator string to determine where to make each split. It doesn't modify an original array and creates a new array
> which is an array of strings split at each point where the separator occurs in the given string. 

```js 
var string = 'Hello world. Nice to meet you.' 

var splits = string.split(' ',3) // split method looks for spaces in a string and returns the first 3 splits that it finds. 
console.log(splits) // returns ['Hello', 'world', 'Nice'] 

var chars = string.split('') 
console.log(chars) // returns ['H','e','l','l','o', ... , '.']
console.log(chars[4]) // returns 'o' 

var copy = string.split()
console.log(copy) // returns ['Hello world. Nice to meet you.'] 
```

#### 🌟 Check if the string is palindrome 
```js
var string = 'abcba' 

function isPalindrome(string) {
  // split the string to array, reverse the array, then join the array with '' 
  return str === str.split('').reverse().join('')
}

isPalindrome(str) // returns true 
```

#### 👭 Join
`Array.prototype.join(separator)`
> The join() method creates and returns `a new string by concatenating all of the elements in an array(or an array-like-object)`, 
> separated by commas or a specified separator string. If the array has only one item, then that item will be returned without using the separator. 

```js 
const elements = ['Fire', 'Water', 'Air'] 

console.log(elements.join()) // returns 'Fire,Water,Air' 
console.log(elements.join('')) // returns 'FireWaterAir' 
```

ref: https://medium.com/@jeanpan/javascript-splice-slice-split-745b1c1c05d2

