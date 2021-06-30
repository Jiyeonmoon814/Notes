## 👀 Show/Hide Component with useState
>useState returns a pair : the current state value and 
>a function that lets you update it. 

```jsx
import React, { useState } from 'react';

function LessText({ text, maxLength }) {
  const [hidden, setHidden] = useState(false);
  
  if(text <= maxLength) {
    return <span>{text}</span>
  }
  
  return (
     <span>
      {hidden ? `${text.substr(0, maxLength).trim()}...` : text}
      {hidden 
              ? (<a onClick={()=>setHidden(true)}>hide</a>)
              : (<a onClick={()=>setHidden(false)}>show</a>)
              }
     </span>
  );
}
```

<br>

## 🆕 Updating state based on previous state
>Create a useState and initialize the value 0 

```jsx
function StepTracker() {
  const [steps, setSteps] = useState(0);
```

<br>

>Declare a function to increment a step counter.
>Now you can update the state by using setSteps in the function

```jsx
function increment() {
  setSteps(steps => steps + 1);
}

return (
  <div>
    Today you've taken {steps} steps! <br />
    <button onClick={increment}> I took another step</button>
  </div>
  );
}
```

<br>

## ⛓️ State as an Array

```jsx
function RandomList() {
  const [items, setItems] = useState([]);
  
  const addItem = () => {
    setItems ([
    //copy the existed values
    ...items,
      {
        id: items.length,
        value: Math.random() * 100
      }
    ]);
  };
  
  return (
    <>
      <button onClick={addItem}>Add a number</button>
      <ul>
        {items.map(item => ()
          <li key={item.id}>{item.value}</li>
        ))}
      </ul>
    </>
  );
}
```

<br>

## 🗝️ State with Multiple keys

```jsx
function LoginForm() {
  const [form, setValues] = useState({
    username:'',
    password:''
  });

const printValues = e => {
  e.preventDefault();
  console.log(form.username, form.password);
}

const updateInput = e => {
  setValues({
    ...form,
    [e.target.name] : e.target.value;
  });
}

return (
  <form onSubmit={printValues}>
    <label>
      Username : 
      <input value={form.username}
             name="username"
             onChange={updateInput} />
    </label>
    <lable>
      Password : 
      <input value={form.password}
             name="password"
             type="password"
             onChange={updateInput} />
     </label> 
     <button>Submit</button>
  </form>
  );
}
```

