### Handling Event
>Handling events with React elements is very similar to handling events on DOM elements, but there are some syntax differences
> >React events are named using camelCase, rather than lowercase
> >With JSX you pass a function as the event handler, rather than a string 

<br>

```javascript
//HTML DOM elements
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

<br>

```jsx
//in React
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

>Another difference is that you cannot return false to prevent default behaviour in React. 
>You must call preventDefault explicitly.

<br>

```jsx
//in React
function Form(){
  function handleSubmit(e){
    e.preventDefault();
    console.log('You clicked submit');
  }
  
  return(
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

<br>

>When using React, you generally don't need to call addEventListener to add listeners to a DOM element after it is created.
>Instead, just provide a listener when the element is initially rendered.

