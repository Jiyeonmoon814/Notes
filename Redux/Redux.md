## ⚡Redux
>Redux is a pattern and library for managing and updating application state, using evetns called "actions".
>It serves as a centralized store for state that needs to be used across your entire application,
>with rules ensuring that the state can only be updated in a predictable fashion. 
>So Redux helps you manage "global" state - state that is needed across many parts of your application.
> >Redux is more useful when :
> ><li>large amounts of application state that are needed in many places in the app</li>
> ><li>The app state is updated frequently over time </li>
> ><li>The logic to update that state may be complex </li>
> ><li>The app has a medium or large-sized codebase, and might be worked on by many people</li>

<br>

## State Management 
>The state, the source of truth that drives our app 

```jsx
function Counter() {
// State : a counter value
const [counter, setCounter] = useState(0)
```

>The actions, the event that occur in the app based on user input, and trigger updates in the state.

```jsx
// Action : code that causes an update to the state when something happens
const increment = () => {
  setCounter(prevCounter => prevCounter + 1)
  }
```

>The view, a declarative description of the UI based on the current state 

```jsx
return (
  <div>
    Value : {counter} <button onClick={increment}>Increment</button>
  </div>
  )
}
```

<br>

>This is a small example of "one-way data flow". However, the simplicity can break down when we have multiple components
>that need to share and use the same state, especially if those components are located in different parts of the application. 
>One way to solve this is to extract the shared from the components, and put it into a centralized location outside the component tree.
>With this, our component tree becomes a big view, and any component can access the state or trigger actions, no matter where they are in the tree. 

<br>

## Actions
> An action is a plain JavaScript object that has a type field. 
> You can think of an action as an event that describes something that happened in the application. 
> An action object can have other fields with additional information about what happened. 
> By convention, we put that information in a field called payload. 

```jsx
const addTodo = (text) => {
  return {
    type : 'todos/todoAdded',
    payload : text
  }
}
```

