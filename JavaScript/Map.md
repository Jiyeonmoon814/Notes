## 🗺️ Map 
`Array.prototype.map()` The <b>map()</b> method <b>creates a new array</b> populated with the results of calling a provided function on every element in the calling array.

```js
// Arrow function
Array.map((el, idx, array) => { ... } )

// Callback fn
Array.map(callbackFn, thisArg)

// Inline callback fn
map(function(el, idx, array) { ... }, thisArg)
```

> `map` calls a provided callbackFfn function once for each element in an array, in order, and `constructs a new array from the results.`
> callbackFn is invoked only for indexes of the array which have assigned values(*including undefined). <br/>
> It is not called for missing elements of the array; that is : indexes that have never been set or have been deleted. <br/>
> ❌ When not to use map() <br />
> Since map builds a new array, using it when you aren't using the returned array is an anti-pattern, use forEach or for ... of instead.
> You shouldn't be using map if you're not using the array it returns or not returning a value from the callback. 

<br />

#### 📝 Using map to reformat objects in an array 

```js
let keyValArr = [
  {key : 1, val : 10},
  {key : 2, val : 20},
  {key : 3, val : 30}
]

let reformattedArr = keyValArr.map( obj => {
  let reObj = {}
  reObj[obj.key] = obj.val
  
  return reObj
})

// reformattedArr is now [{1 : 10}, {2 : 20}, {3 : 30}]

// keyValArr still keeps the initial state 
```
