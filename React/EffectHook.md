## 💡 Effect Hook

<br>

#### What does useEffect do?
>By using this Hook, you tell React that your component needs to do something after render. 
>React will remember the function you passed(we'll refer to it as our effect), 
>and call it later after performing the DOM updates. In this effect, we set the document title, 
>but we could also perform data fetching or call some other imperative API. 

<br>

#### Why is useEffect called inside a component?
>Placing useEffect inside the component lets us access the count state variable or any props right from the effect.
>It's already in the function scope.

<br>

#### Does use Effect run after every render?
>Yes. By default, it runs both after the first render and after every update. you might find it easier to think that
>effects happen after render. React guarantees the DOM has been updated by the time it runs the effects. 

<br>

```jsx
import React, { useState, useEffect } from 'react'; 
```

>We import the useEffect Hook from React. It lets us perform side effects in function components.
> >Data fetching, setting up a subscription, and manually changing the DOM in React components are all examples of side effects. 

<br>

```jsx
const FrinendStatus = (props) => {
  const [isOnline, setIsOnline] = useState(null);
  
  useEffect(() => {
    const handleStatusChange = (status) => {
      setIsOnline(status.isOnline);
    }
    
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    //Specify how to clean up after this effect 
    return function cleanUp() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  
  if(isOnline === null) {
    return 'Loading...'
   }
   return isOnline ? 'Online' : 'Offline';
}
```

>Every effect may return a function that cleans up after it. This lets us keep the logic for adding and 
>removing subscriptions close to each other. They're part of the same effect. <br>
>React performs the cleanup when the component unmounts. However, as we learned earlier, effects run for every render
>and not just once. This is why React also cleans up effect from the previous render before running the effects next time. 

<br>

```jsx
const Example = () => {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
}
```

>Other effects might not have a cleanup phase, and don't return anything. 

<br>

## 💡 Optimizing performance by skipping effects 
> In some cases, cleaning up or applying the effect after every render might create a performance problem.
> You can tell React to skip applying an effect if certain values haven't changed between re-renders.
> To do so, pass an array as an optional second argument to useEffect

<br>

```jsx
useEffect (() => {
  document.title = `You clicked ${count} times`
}, [count]); // Only re-run the effect if count changes 
```

> In the example above, we pass `[count]` as the second argument. It means If the count is 5,
> and then our component re-renders with count still equal to 5, React will compare "5" from the previous 
> render and "5" from the next render. Because all items in the array are the same (5 === 5), 
> React would skip the effect. That's how React optimizes performance. 

> When we render with count updated to 6, React will compare the items in the "5" array from the previous 
> render to items in the "6" array from the next render. This time, React will re-apply the effect because 
> 5!==6. If there are multiple items in the array, React will re-run the effect even if just one of them is different. 

<br>

```jsx
useEffect(() => {
  setFilterItems(
    items.filter((item) => 
      item.toLowerCase().includes(search.toLowerCase())
    )
  )
}, [search, items])
```

> You can also set more than one as the second argument like the above example. 
> React would skip the effect, if nothing has changed from search, items state. 


<br>

## 🤔 How to make an AJAX Call 

```jsx 
function MyComponent() {
  const [error, setError] = useState(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [items, setItems] = useState([]);
  
  useEffect(() => {
    fetch("https://api.example.com/items")
      .then(res => res.json())
      .then(
        (result) => {
          setIsLoaded(true);
          setItems(result);
        },
        //it's important to handle errors here 
        //instead of a catch() block so that we don't swallow
        //exceptions from actual bugs in components. 
        (error) => {
          setIsLoaded(true);
          setError(error);
        }
      )
  }, [])
```

<br>

>If you want to run an effect and clean it up only once (on mount and unmount),
>you can pass an empty array [] as a second argument. This tells React that your effect
>doesn't depend on any values from props or state, so it never needs to re-run. 
>This isn't handled as a special case, it follows directly from how the dependencies array always works. 

```jsx
if(error) {
  return <div>Error : {error.message}</div>;
}else if(!isLoaded) {
  return <div>Loading...</div>;
}else (
  return (
  <ul>
    {items.map(item => (
      <li key={items.id}>
        {item.name} {item.price}
      </li>
     ))}
  </ul>
    );
  }
}
```
