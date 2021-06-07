# The 7 JS Array Methods

### Array.map()
>This method takes a function that allows you to loop through all the items in the array and modify them.
>It will come in handy whenever you want to update all the items in an array and store it in a new one.
>So use it, when you want to modify the contents of an existing array and store the result as a new variable. 
<br>

```javascript
const laptops = [
  {brand:"Apple", price: 1500},
  {brand:"Samesung", price: 1300}
  ];
  
  const laptopsWithTax = laptops.map(laptopObj => {
    return {
    //Return the original laptop object
    ...laptopObj,
    //and also add a new value containing the price with tax
    priceWithTax : laptopObj.price * 1.2
    }
  });
  
  //The result would be
  [
  {brand:"Apple", price: 1500, priceWithTax:1800},
  {brand:"Samesung", price: 1300, priceWithTax:1560}
  ];
```

<br>

### Array.filter()
>Just like the .map() method, it will return a new array and leave the original array as it is. 
>Also it will allow you to remove items from an array that don't fit a certain condition/criteria.
>Each item of the array is checked to see if it fits the criteria, if it passes the test it is returned within the new array! 
<br>

```javascript
const laptopOrigins = [
  {brand:"Apple", price: 1500, origin:'USA'},
  {brand:"Samesung", price: 1300, origin:'South Korea'},
  {brand:"LG", price: 1200, origin:'South Korea'},
  {brand:"DELL", price: 1100, origin:'USA'}
 ];
  
const theCountryOfOrigin = laptopOrigins.filter(laptopObj => laptopObj.origin === "South Korea");

//The Result would be 
  [
  {brand:"Samesung", price: 1300, origin:'South Korea'},
  {brand:"LG", price: 1200, origin:'South Korea'}
  ];
```

<br>

### Array.reduce()
>The .reduce() method takes a callback function as its first parameter and an optional initial value as its second parameter.
>If an initial value is not supplied the first array value is used. 
>The callback function provides an accumulator and currentValue parameter used to perform the reduce calculation. 
<br>

```javascript
const numbers = [10, 26, 12, 64, 44, 23];
const total = numbers.reduce((accumulator,currentValue)=> accumulator+currentValue, 0);
const totalWithParam = numbers.reduce((accumulator,currentValue)=> accumulator+currentValue, 33);

//The result of total would be 10 + 12 + 23 + 26 + 44 + 66 >> 179
//The result of totalWithParam would be 33 + 10 + 12 + 23 + 26 + 44 + 66 >> 212
```

<br>

>Another way to use .reduce() method, is to flatten an array. 

<br>

```javascript
const flattened = [[0,1],[2,3],[4,5]].reduce((accumulator,currentValue)=>accumulator.concat(currentValue),[]);
//The result would be [0,1,2,3,4,5]
```

<br>

>Whenever you want to convert an array down to a single value by manipulating its values, you can use .reduce() method. 

<br>

### Array.forEach()
>This method works a lot like a regular for loop. It loops over an array and executes a function on each item.
>The first parameter of .forEach() is a callback function that includes the current value and index of the loop. 
>You should use .forEach() method, when you want to simply loop each item of any array without constructing a new array 
<br>

```javascript
const laptops = [
  {brand:"Apple", price: 1500},
  {brand:"Samesung", price: 1300}
  ];
  
laptops.forEach(laptop => {
  console.log(`The ${laptop.brand} will cost you ${laptop.price} pounds before taxes`);
});

//The result would be 
//"The Apple will cost you 1500 pounds before taxes"
//"The Samsung will cost you 1300 pounds before taxes"
```

<br>

### Array.find()
>Just like the .filter() method, you'll be able to pass a condition which the value of the array must match.
>The difference between the two, is that .find() will only return the first element that matches the condition you provided. 
>By using this method, you can get the first item of an array that passes an explicitly defined test. 

<br>

```javascript
const laptopOrigins = [
  {brand:"Apple", price: 1500, origin:'USA'},
  {brand:"Samesung", price: 1300, origin:'South Korea'},
  {brand:"LG", price: 1200, origin:'South Korea'},
  {brand:"DELL", price: 1100, origin:'USA'}
 ];
  
const laptopFromKorea = laptopOrigins.find(laptop => laptop.origin === "South Korea");

//The Result would be only 
  [
  {brand:"Samesung", price: 1300, origin:'South Korea'}
  ];
```

<br>

### Array.every()
>The .every() method will check if every elements in the array passes the provided condition.
>If all elements in the array pass the condition, the method will return true and if it's not, it will return false. 
>So you can confirm that every item of an array passes an explicitly defined condition. 

<br>

```javascript
const laptopOrigins = [
  {brand:"Apple", price: 1500, origin:'USA'},
  {brand:"Samesung", price: 1300, origin:'South Korea'},
  {brand:"LG", price: 1200, origin:'South Korea'},
  {brand:"DELL", price: 1100, origin:'USA'}
 ];
  
const laptopOverThousand = laptopOrigins.every(laptop => laptop.price >= 1000);

//The Result would be 'true'
```

<br>

### Array.some()
>Instead of returning true if all elements of an array pass the test like .every() method, 
>.some() method returns true if at least one element passes the test.
>If it finds a successful array element it stops and returns true. 
>However, if .some() method reaches the end of the array without success, it returns false. 
>So Use .some() method, If you need the first item of an array that passes an explicitly defined test. 

<br>

```javascript
const laptopOrigins = [
  {brand:"Apple", price: 1500, origin:'USA'},
  {brand:"Samesung", price: 1300, origin:'South Korea'},
  {brand:"LG", price: 1200, origin:'South Korea'},
  {brand:"DELL", price: 1100, origin:'USA'}
 ];
  
const laptopFromUSA = laptopOrigins.some(laptop => laptop.origin === "USA");
//The Result would be 'true'

const laptopFromUK = laptopOrigins.some(laptop => laptop.origin === "UK");
//The Result would be 'false'
```

<br>

* source:<https://medium.com/dailyjs/the-7-js-array-methods-you-will-need-in-2021-a9faa83b50e8>





