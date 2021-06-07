# The 7 JS Array Methods

###Array.map()
>This method takes a function that allows you to loop through all the items in the array and modify them.
>It will come in handy whenever you want to update all the items in an array and store it in a new one.
>So use it, when you want to modify the contents of an existing array and store the result as a new variable. 
<br>
<pre>
<code>
```javascript
const laptopsWithPrice = [
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
</code>
</pre>
