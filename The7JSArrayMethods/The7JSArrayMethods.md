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

