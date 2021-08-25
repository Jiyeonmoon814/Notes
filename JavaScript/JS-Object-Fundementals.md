### 🌝 The object literal is the simplest way to create objects 
> The simplest way to create an object is to use the object literal. We define a set of properties inside curly braces separated by commans.

```js
const game = {
  name: 'Fornite',
  developer: 'Epic Games'
};
```

### 🙄 Objects are dynamic collections of properties 
> Once an object is created we can add, edit or delete properties from it. 

```js
game.year = 2017;
delete game.year;
```

### 🙂 Properties can be accessed using the dot and the bracket notations

```js
console.log(game.name)
```

> When the key is not a valid identifier we need to use the bracket notation.

```js
console.log(game["name"]);
```

### 😲 Keys are converted to strings 
> Keys are string only. When non-string values are used as keys they are converted to strings. 
> When the developerKey is used as a key it is frist converted to a string using the toString method,
> then the result 'developer' string key is used to retrieve the value. 

```js
const developerKey = {
  toString () {
    return 'developer'
    }
  }
  
 console.log(game[developerKey]);
 //Epic Games
 ```
 
 ### 🥥 Objects inherit from other objects 
 > In JavaScript, objects inherit from other objects. Objects have a hidden property called `_proto_` pointing to their prototype.
 > All objects inherit from the global `Object.prototype.`

```js
game._proto_ === Object.prototype; //true
```

<br>

> The game object has properties like toString or toLocalString even if we haven't defined such methods. 
> They are inherited from the Oject.prototype object.

```js
console.log(game.toString);
//ƒ toString() { [native code] }

console.log(game.toLocaleString);
//ƒ toLocaleString() { [native code] }
```

<br>

> The Object.create() takes a prototype object and creates a new object pointing to it.

```js
const prototypeObj = {}
const obj = Object.create(prototypeObj);

console.log(obj._proto_ === prototypeObj)
```
 
