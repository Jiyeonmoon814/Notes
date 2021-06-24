## ⏫ Lifting State Up 
>Lifting state up is a common patter that is essential for React developers to know. 
>It helps you avoid more complex (and often unnecessary) patterns for managing your state. 

<br>

## 💔 Breaking down an app 
>Let's say there's a simple Todo app which consists of three components: TodoCount, TodoList and AddTodo. 
>All of these components, as their name suggests, are going to need to share some common state. 

```jsx
// App.js
import React from 'react';

export default function App() {
  return (
    <>
      <TodoCount />
      <TodoList />
      <AddTodo />
    </>
  );
}
```

<br>

>How many total to dues you have in your application.

```jsx
function TodoCount() {
  return <div>Total Todos: </div>;
}
```

<br>

>Where you show all of your todos. It has some initial state which you'll display within an unordered list.

```jsx
function TodoList() {
  const [todos, setTodos ] = React.useState(["Meeting","Yoga","Dentist"]);
  
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo}>{todo}</li>
      ))}
    </ul>
  );
}
```

<br>


>This consists of a form, where you want to be able to add a new item to this list.

```jsx
function AddTodo() {
  const handleSubmit = e => {
    e.preventDefault();
    const todo = e.target.elements.todo.value;
  }

retrun (
  <form onSubmit={handleSubmit}>
    <input type="text" id="todo" />
    <button type="submit">Add Todo</button>
  </form>
 );
}
```

>Here's the thing. These components need to share some state, some todo states. 

<br>

### 🤔 Why should you care about lifting state up?
>We lift up state to a common ancestor of components that need it, so they can all share in the state.
>This allows us to more easily share state among all of these components that need rely upon it.

>So what common ancestor should you lift up your state to so all of the components can read
>from and update that state? <br>
><Strong>The App component. </strong>Here's what your app should now look like 

```jsx
import { useState } from 'react';
export default function App() {
  const [todos, setTodos ] = useState(["Meeting","Yoga","Dentis"]);
  
   return (
    <>
      <TodoCount />
      <TodoList />
      <AddTodo />
    </>
  );
}
```

<br>

### 🧐 How to pass state down 
>There's a small problem. TodoList doesn't have access to the todos state variable,
>so you need to pass down the data from App. <br>You can do that with components in React using props ❗

```jsx
import { useState } from 'react';
export default function App() {
  const [todos, setTodos ] = useState(["Meeting","Yoga","Dentist"]);
  
  return (
      <>
        <TodoCount todos={todos}/>
        <TodoList todos={todos}/>
        <AddTodo />
      </>
   );
 }
```

<br>

>You can destructure todos from the props object. This allows you to see your todo items once again.

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo}>{todo}</li>
      ))}
    </ul>
  );
}
```

<br>

>Since TodoCount relies upon that state, we'll provide the same prop, todos, 
>destructure it from the props object and display the total number of todos using todos.length

```jsx
function TodoCount( {todos} ) {
  return <div>Total Todos : {todos.length} </div>;
}
```

<br>

### 🙄 How to pass callbacks down 
>To update your todo state, you don't need to pass down both values, the variable and the setter function.<br>
><Strong>All you need to do is pass down setTodos.</Strong> You'll pass it down to addTodo as a prop of the same name
>(setTodos) and destructure it from props. 

```jsx
function AddTodo( {setTodos} ) {
  const handleSubmit = e => {
    e.preventDefault();
    const todo = e.target.element.todo.value;
    //***
    setTodos(prevTodos => [...prevTodos, todo]);
    //***
  }
  
  return(
    <form onSubmit={handleSubmit}>
    ... </form>
  );
}
```

<br>

>As you can see, you're using your form on submit to get access to the input's value 
>whatever was typed into it, which you're putting within a local variable named todo. 

>Instead of needing to pass down the current todos array, you can just use an inner function 
>to get the previous todo's value. This allows you to get previous todos and just return 
>what you want the new state to be. This new state will be an array, in which you'll spread 
>all of the previous todos and add your new todo as the last element in that array. 


