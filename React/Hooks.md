## Hooks
> Only call Hooks at the top level.
> Only call Hooks from React function components. 
> >If you write a function component and realize you need to add some state to it,
> >previously you had to convert it to a class. Now you can use a Hook inside the existing function component.

<br>

### :round_pushpin: State Hook
>useState is a Hook. We call it inside a function component to add some local state to it and useState returns a pair
> >the current state value <br>
> >and A function that lets you update it. 

<br>

```jsx
import {useState} from 'react';

const App = () => {
  // You can use Hooks here! 
  // Declare a new state variable, which we'll call tasks 
  const [tasks,setTasks] = useState([
    {id:1, text:"Meeting", reminder:true },
    {id:2, text:"Shopping", reminder:false }
  ])
  // And of course you can declare multiple state variables 
  const [showAddTask,setShowAddTask] = useState(false);
  
  return (
    <div></div>
  )
}
```

<br>

#### What does calling useState do? 
>It declares a "state variable". This is a way to preserve some values between the function calls. 
>useState is a new way to use the exact same capabilities that this.state provides in a class. 
>Normally, variables disappear when the function exits 
> >but state variables are preserved by React. 

<br>

#### What do we pass to useState as an argument?
>The only argument to the useState() Hook is the initial state. Unlike with classes,
>the state doesn't have to be an object. We can keep a number or a string if that's all we need.
>If we wanted to store two different values in state, we would call useState() twice 

<br>

#### What does useState return?
>It returns a pair of values.
> >The current state (tasks) <br>
> >and A function that updates it. (setTasks)

<br>

#### Recap what we learned line by line 

<br>

```jsx
import { useState } from 'react';
```

>We import the useState Hook from React. It lets us keep local state in a function component.

<br>

```jsx
const example = () => {
  const [count, setCount] = useState(0);
```

>Inside the example component, we declare a new state variable by calling the useState Hook. <br>
>We're calling our variable count because it holds the number of buttom clicks. We initialize it to zero by passing 0 as the only useState argument. <br>
>The second returned item is itself a function. It lets us update the count so we'll name it setCount. 

<br>

```jsx
  return (
    <div>
      <p>You click {count} times</p>
```

>Reading state 

<br>

```jsx 
    <button onClick={()=>setCount(count+1)}>
      Click Me
    </button>
   </div>
  );
 }
```

>When the user clicks, we call setCount with a new value to update state. React will the re-render the example component, passing the new count value to it. 



