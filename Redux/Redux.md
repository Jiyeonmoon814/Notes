## ⚡Redux
>Redux is a pattern and library for managing and updating application state, using evetns called "actions".
>It serves as a centralized store for state that needs to be used across your entire application,
>with rules ensuring that the state can only be updated in a predictable fashion. 
>So Redux helps you manage "global" state - state that is needed across many parts of your application.

##### Redux is more useful when :
<li>large amounts of application state that are needed in many places in the app</li>
<li>The app state is updated frequently over time </li>
<li>The logic to update that state may be complex </li>
<li>The app has a medium or large-sized codebase, and might be worked on by many people</li>

<br>

## State Management 
###### The state, the source of truth that drives our app 

```jsx
function Counter() {
// State : a counter value
const [counter, setCounter] = useState(0)
```


###### The actions, the event that occur in the app based on user input, and trigger updates in the state.

```jsx
// Action : code that causes an update to the state when something happens
const increment = () => {
  setCounter(prevCounter => prevCounter + 1)
  }
```

###### The view, a declarative description of the UI based on the current state 

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

## 🥊 Actions
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

<br>

## ⚡ Reducers
> A reducer is a function that receives the current state and an action object, decides how to update the state
> if necessary, and returns the new state <Strong>(state, action) => newState</Strong>.
> You can think of a reducer as an event listener which handles events based on the received action (event)type.

##### The logic inside reducer functions typically follows the same series of steps
###### 1️⃣ Check to see if the reducer cares about this action. 
###### 2️⃣ If so, make a copy of the state, update the copy with new values and return it.
###### 3️⃣ Otherwise, return the existing state unchanged.

```jsx
const initialState = { value : 0 }

function counterReducer(state = initialState, action) {
  // check to see if the reducer cares about this action
  if(action.type === 'counter/increment') {
    return {
      // if so, make a copy of state
      ...state,
      
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  //otherwise return the existing state unchanged
  return state
}
```

<br>

## 🏪 Store
> The current Redux application state lives in an object called the store.
> The store is created by passing in a reducer, and has a method called getState that returns the current state value.

```jsx
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer : counterReducer })

console.log(store.getState()) // {value:0}
```

<br>

## 📬 Dispatch
> The Redux store has a method called dispatch. 
> <Strong>The only way to update the state is to call store.dispatch() and pass in an action object.</Strong>
> The store will run its reducer function and save the new state value inside, 
> and we can call getState() to retrieve the update value.

```jsx
store.dispatch({ type: 'counter/increment' })

console.log(store.getState()) // {value:1}
```

<br>

> <Strong>You can think of dispatching action as "triggering an event" in the application.</Storng>
> Something happened, and we want the store to know about it.
> Reducers act like event listeners, and when they hear an action they are interested in, they updated the state in response.

```jsx
const increment = () => {
  return {
    type: 'counter/increment'
  }
}

store.dispatch(increment())

console.log(store.getState()) // {value:2}
```

<br>

## ☑️ Selectors
> Selectors are functions that know how to extract specific pieces of information from a store state value.
> As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data.

```jsx
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())

console.log(currentValue) // 2 
```

<br>

## 💡 Redux Application Data Flow 
##### Initail setup
<li> A Redux store is created using a root reducer function.</li>
<li> The store calls the root reducer once, and saved the return value as its initial state</li>
<li> When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render.
    They also subscribe to any future store updates so they can know if the state has changed.</li>

##### Updates 
<li> Something happens in the app, such as a user clicking a button, The app code dispatches an action to the Redux store 
    ,like dispatch({type : 'counter/increment'})</li>
<li> The store runs the reducer function again with the previous state and the current action, and saves the return value as the new state.</li>
<li> The store notifies all parts of the UI that are subscribed that the store has been updated.</li>
<li> Each UI component that needs data from the store checks to see if the parts of the state they need have changed.</li>
<li> Each component that sees its data has changed forces a re-render with the new data, so it can update what's shown on the screen.</li>


<br>

## 💯 Conclusion 
##### Redux is a library for managing global application state. 
##### Redux uses a "one-way data flow" app structure
>When something happens in the app : <br>
> 1️⃣The UI dipatches an action <br>
> 2️⃣ The store runs the reducers, and the state is updated based on what occurred <br>
> 3️⃣ The store notifies the UI that the state has changed <br>
> 4️⃣ The UI re-renders based on the new state <br>
##### Redux uses several types of code 
<li> Actions are plain objects with a type field, and describe what happened in the app.</li>
<li> Reducers are functions that calculate a new state value based on previous state + an action.</li>
<li> A Redux store runs the root reducer whenever an action is dspatched. </li>

