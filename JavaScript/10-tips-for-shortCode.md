## 1️⃣ Conditionally add properties to Object
```js
const condition = true;

const person = {
  id : 1,
  name : 'Andy Uz',
  ...(condition && { age : 28 }),
};
```

<br>

>The && operator returns the last evaluated expression if every operand evaluates to true.
>So an object { age : 16 } is returned, which is then spread to be part of the person object. 
>If condition is false the JS will do something like this 

```js
const person = {
  id : 1,
  name : 'Andy Uz',
  ...(false), //evaluates to false
};

//spreading false has no effect on the object
console.log(person); // {id:1, name 'Andy Uz'}
```

<br>

## 2️⃣ Check if a property exists in an Object 
```js
const person = { name : 'Andy Uz', salary : 3000 };

console.log('salary' in person); //returns true
console.log('age' in person); //returns false
```

<br>

## 3️⃣ Dynamic property names in Objects 
>You can set an object property using the keyname notation to add the properties

```js
const dynamic = 'flavour';

var item = {
  name : 'IceCream',
  [dynamic]:'Mint'
}

console.log(item); //{name:'IceCream', flavour:'Mint'}
```

<br>

>The same trick can also be used to reference object properties with a dynamic key 

```js
const keyName = 'name';
console.log(item[keyName]); // returns 'Biscuit'
```

<br>

## 4️⃣ Object destructuring with a dynamic key 
>You know that you can destructure a variable and rename it right away with : notation

```js
const person = { id : 1, name : 'Andy Uz' };
const { name : personName } = person;

console.log(personName); //returns 'Andy Uz'
```

<br>

>But do you know that you can also destructure properties of an object
>when you don't know the key name or key name is dynamic?

```js
const templates = {
  'hello' : 'Hello there',
  'bye' : 'Good bye'
};

const templateName = 'bye';

const { [templateName] : template } = templates;

console.log(template) // returns 'Good bye'
```

<br>

## 5️⃣ Nullish coalescing, ?? Operator
>The ?? operator is useful when you wnat to check whether a variable is null or undefined.
>It returns the right-hand side operand when its left-hand side operand is null or undefined,
>and otherwise returns its left-hand side operand.

```js
const foo = null ?? 'Hello';
console.log(foo); //returns 'Hello'

const baz = 0 ?? 'Hello'
console.log(baz); //returns 0
```

<br>

>In the second example, 0 is returned because even though 0 is considered falsy in JS,
>it is not null or undefined. You may think that we can use the || operator here 
>but there's a difeerence between these two. 

```js
const cannotBeZero = 0 || 5;
console.log(cannotBeZero); //returns 5

const canBeZero = 0 ?? 5;
console.log(canBeZero); //returns 0
```

<br>

## 6️⃣ Optional chaining(?.)
```js
const book = { id:1, title: 'Title', author: null };
// normally, you would do this
console.log(book.author.age) // throws error
console.log(book.author && book.author.age); // returns null (no error)

// with optional chaining
console.log(book.author?.age); // returns undefined

// or deep optional chaining
console.log(book.author?.address?.city); // returns undefined
```

<br>

>You can also use optional chaining with functions like this 

```js
const person = {
  firstName: 'Haseeb',
  lastName: 'Anwar',
  printName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};

console.log(person.printName()); // returns 'Haseeb Anwar'
console.log(persone.doesNotExist?.()); // returns undefined
```

<br>

## 7️⃣ Boolean conversion using !! operator
>The !! operator can be used to quickly convert the result of an expression into a boolean true or false.

```js
const greeting = 'Hello there!';
console.log(!!greeting) // returns true

const noGreeting = '';
console.log(!!noGreeting); // returns false
```

<br>

## 8️⃣ String and Integer Conversions
>Quickly convert a string to a number using the + operator

```js
const stringNumer = '123';

console.log(+stringNumer); // returns integer 123
console.log(typeof +stringNumer); // returns 'number'
```

<br>

>To quickly convert a number to a string, use the + operator followed
>by an empty string "" 

```js
const myString = 25 + '';

console.log(myString); // returns '25'
console.log(typeof myString); // returns 'string'
```

<br>

## 9️⃣ Check falsy values in an array 
> You must be familiar with filter, some, and every array methods. 
> But you should also know that you can just the Boolean method to test for truthy values

```js
const myArray = [null, false, 'Hello', undefined, 0];

// filter falsy values
const filtered = myArray.filter(Boolean);
console.log(filtered); // returns ['Hello']

// check if at least one value is truthy
const anyTruthy = myArray.some(Boolean);
console.log(anyTruthy); // returns true

// check if all values are truthy
const allTruthy = myArray.every(Boolean);
console.log(allTruthy); // returns false
```

<br>

>As we know that these array methods take a callback function,
>so we pass Boolean as a callback function. Boolean itself takes an argument and returns true or false
>based on the truthiness of the argument. 

```js
myArray.filter(val => Boolean(val));

//Is the same as this 
myArray.filter(Boolean);
```

<br>

## 🔟 Flattening array of arrays 
>There is a method flat on the prototype Array that lets you make a single array from an array of arrays.

```js
const myArray = [{ id: 1 }, [{ id: 2 }], [{ id: 3 }]];

const flattedArray = myArray.flat(); 
// returns [ { id: 1 }, { id: 2 }, { id: 3 } ]
```

<br>

>You can also define a depth level that specifies how deep a nested array structure sholbe be flattened.

```js
const arr = [0, 1, 2, [[[3, 4]]]];

console.log(arr.flat(2)); // returns [0, 1, 2, [3,4]]
```

<br>

sources : <https://betterprogramming.pub/10-modern-javascript-tricks-every-developer-should-use-377857311d79>
