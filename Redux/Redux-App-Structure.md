## 🏪 Create the Redux store
> The Redux store is created using the configureStore function from Redux Toolkit. 
> configureStore requires that we pass in a reducer argument. When we call configureStore,
> we can pass in all of the different reducers in an object. The key names in the object will define
> the keys in our final state value. 

<br> 

```jsx
import { configureStore } from '@reduxjs/toolkit'
import usersReducer from '../feature/users/userSlice'
import postsReducer from '../feature/posts/postsSlice'
import commentsReducer from '../feature/comments/commentsSlice'

export default configureStore({
  reducer : {
    users: usersReducer,
    posts: postsReducer,
    comments: commentsReducer
  }
})
```

> When we pass in an object like { counter: counterReducer }, that says that we want to have a state.counter section of our Redux state object, 
> and that <Strong>we want the counterReducer function to be in charge of deciding if and how to update the state.counter section </Strong>
> whenever an action is dispatched.

###### 📝 Redux allows store setup to be customized with different kinds of plugins("middleware" and "enhancers"). configureStroe automatically adds several middleware to the store setup by default to provide a good developer experience, and also sets up the store so that the Redux DevTools Extension can inspect its contents.

<br>

## 🔪 Creating Slice Reducers and Actions
> <Strong>A slice is a collection of Redux reducer logic and actions for a single feature in your app, </Strong>
> typically defined together in a single file. Since we know that the counterReducer function is coming from 
> features/counter/counterSlice.js, let's see what's in that file, piece by piece. <br>
> What's really important in Redux is the reducer functions, and the logic they have for calculating new state.

<br>

>Redux Toolkit has a function called createSlice, which takes care of the work of generating action type strings,
>action creator functions, and action objects. All you have to do is define a name for this slice, write an object 
>that has some reducer functions in it, and it generates the corresponding action code automatically. 

```jsx
// features/counter/counterSlice.js 

import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    vale: 0
  },
```

>createSlice needs us to pass in the initial state value for the reducers, so that there's a state the first time it gets called.

<br>

```jsx
  reducers: {
    increment: state => {
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions
export default counterSlice.reducer 
```

>The String from the name option is used as the first part of each action type, and the key name of each reducer function 
>is used as the second part. So, the "counter" name + the "increment" reducer function generated an action type of 
>{type: "counter/increment"}

<br>

###### createSlice automatically generates action creators with the same names as the reducer functions we wrote. We can check that by calling one of them and seeing what it returns

```jsx
console.log(counterSlice.actions.increment())
// {type: "counter/increment"}
```

###### It also generates the slice reducer function that knows how to respond to all these action types.

```jsx 
const newState = counterSlice.reducer(
  { value: 10},
  counterSlice.actions.increment()
)
console.log(newState)
// {value: 11}
```

## 💡 Rules of Reducers 
##### 1️⃣ They should only calculate the new state value based on the state and action arguments.
##### 2️⃣ They are not allowed to modify the existing state. Instead, they must make "immutable updates", by copying the existing state and making changes to the copied values.
##### 3️⃣ They must not do any asynchronous logic or other "side effects".

<br>

## 📍 Reducers and Immutable updates
> Earlier, we talked about "mutation"(modifying existing object/array values) and 
> "immutability"(treating values as something that cannot be changed). In Redux,
> <Strong>our reducers are never allowed to mutate the original / current state values! </Strong>

##### :x: Illegal - by default, this will mutate the state

```jsx
state.value = 123
```

##### :o: This is safe, because we made a copy

```jsx
return {
  ...state,
  value : 123
}
```

<br>

## 📝 Writing Async logic with Thunks 
>So far, all the logic in our application has been synchronous. Actions are dispatched,
>the store runs the reducers and calculates the new state, and the dispatch function finishes.
>But, the javascript language has many ways to write code that is asynchronous, and our apps normally have async logic
>for things like fetching data from an API. We need a place to put that async logic in our Redux apps. 

#### A thunk is a specific kind of Redux function that can contain asynchronous logic and it's written using two functions.
<li> An inside thunk function, which gets dispatch and getState as arguments. </li>
<li> The outside creator function, which creates and returns the thunk function. </li>

<br>

```jsx
// features/counter/counterSlice.js
export const incrementAsync = amount => dispatch => {
  setTimeout(() => {
    dispatch(incrementByAmount(amount))
  }, 1000)
}
```

> The function above is called a thunk and allows us to perform async logic.
> It can be dispatched like a regular action: `dispatch(incrementAsync(10))`.
> This will call the thunk with the `dispatch` function as the first argument. 
> Async code can then be executed an other actions can be dispatched. 

<br>

> We can use them the same way we use a typical Redux action creator. 

```jsx
store.dispatch(incrementAsync(5))
```

<br>

>However, using thunks requires that the `redux-thunk middleware` be added to the Redux store when it's created.
>Fortunately, Redux Toolkit's configureStore function already sets that up for us automatically, 
>so we can go ahead and use thunks here. <br>
>When you need to make AJAX calls to fetch data from the server, <Strong>you can put that call in a thunk.</Strong>

```jsx
// features/counter/counterSlice.js

// the outside "thunk creator" function 
const fetchUserById = userId => {
  // the inside "thunk function"
  return async (dispatch, getState) => {
    try{
      //make an async call in the thunk
      const user = await userAPI.fetchById(userId)
      //dispatch an action when we get the response back
      dispatch(userLoaded(user))
    }catch(err){
      //If something went wrong, handle it here
      console.log(err)
    }
  }
}
```

<br>

## 📖 Reading data with useSelector
> a useSelector hook lets our component extract whatever pieces of data it needs from the Redux store state.

```jsx
import React, { useState } from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment, incrementByAmount, incrementAsync, selectCount } from './counterSlice'

export function Counter() {
  const count = useSelector(selectCount)
  const dispatch = useDispatch()
  const [incrementAmout, setIncrementAmount] = useState('2')
  
  retunr (
    <div>
    ...
      <button className={styles.button} aria-label="Increment value"
          onClick={()=>dispatch(increment())} > + 
      </button>
    ...
    </div>
  )
}
```

<br>

> Earlier, we saw that we can write `selector functions`, which take state as an argument and return some part of the state value.
> A counterSlice.js has this selector function. The function below is called  a selector 
> and `allow us to select a value from the state.` Selectors can also be defined inline where they're used instead of in the slice file. <br>
> For example : `useSelector((state) => state.counter.value)` 

```jsx
// features/counter/counterSlice.js 
export const selectCount = state => state.counter.value 
```

<br>

> If we had access to a Redux sotre, we could retrieve the current counter value as 

```jsx
const count = selectCount(store.getState()) //0
```

<br>

> Our component can't talk to the Redux store directly, because we're not allowed to import it into component files. 
> But `useSelector` takes care of talking to the Redux store behind the scenes for us. 
> If we pass in a selector function, it calls `someSelector(store.getState())` for us, and returns the result. <br>
> So, we can get the current store counter value by doing :

```jsx
const count = useSelector(selectCount)
```

<br>

> We don't have to only use selectors that have already been exported, either. 
> For example, we could write a selector function as an inline argument to `useSelector`

```jsx
const countPlusTwo = useSelector(state => state.counter.value + 2)
```

##### Any time an action has been dispatched and the Redux store has been updated, useSelector will re-run our selector function. If the selector returns a different value than last time, useSelector will make sure our component re-renders with the new value. 

## 📬 Dispatching Actions with useDispatch
> Similarly, we know that if we had access to a Redux store, we could dispatch actions using action crators,
> like `store.dispatch(increment())`. Since we don't have access to the store itself, we need some way to have access to just the dispatch method. <br>
> The useDispatch hook does that for us, and gives us the actual method from the Redux store : 

```jsx 
const dispatch = useDispatch()
```

<br>

> From there, we can dispatch actions when the user does something like clicking on a button 

```jsx
// features/counter/Counter.js 
<button className={styles.button} aria-label="Increment value" 
      onClick={() => dispatch(increment())} > + 
</button>
```

<br>

## 🙄 What kind of data should I put into Redux? 
###### By now you might be wondering, "Do I always have to put all my apps state into the Redux store?"
##### The answer is "NO". Global state that is needed across the app should go in the Redux store. State that's only needed in one place should be kept in component state.

> If you're not sure where to put something, here are some common rules of thumb for determining what kind of data should be put into Redux : 
> <li>Do other parts of the application care about this data?</li>
> <li>Do you need to be able to create further derived data based on this original data?</li>
> <li>Is the same data being used to drive multiple components?</li>
> <li>Is there value to you in being able to restore this state to a given point in time(ie, time trave debugging)?</li>
> <li>Do you want to cache the data(ie, use what's in state if it's already there instead of re-requesting it)?</li>
> <li>Do you want to keep this data consistent while hot-reloading UI components (which may lose their internal state when swapped)?</li>

<br>

## 🏬 Providing the Store
> We've seen that our components can use the useSelector and useDispatch hooks to talk to the Redux store. 
> But, since we didn't import the store, how do those hooks know what Redux store to talk to?

<br>

```jsx
// index.js
...
import store from './app/store'
import { Provider } from 'react-redux'

ReactDOM.renter(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

> We always have to call `ReactDOM.render(<App />)` to tell React to start rendering our root `<App>` component. 
> In order for our hooks like `useSelector` to work right, we need to use a component called `<Provider>` to pass down the Redux store
> behind the scenes so they can access it. 

> We already created our store in `app/store.js`, so we can import it here. Then, we put our `<Provider>` component around the whole `<App>`, and pass in the store:`<Provider store={store}>`. 
> Now, any React components that call useSelector or useDispatch will be talking to the Redux store we gave to the Provider.

<br>

## 💡 Conclusion 
#### 1️⃣ We can create a Redux store using the Redux Toolkit `configureStore` API
> configureStore accepts a reducer function as a named argument and automatically sets up the store with good default settings.

#### 2️⃣ Redux logic is typically organized into files called `slices`
> A slice contains the reducer logic and actions related to a specific feature / section of the Redux state. 
> `createSlice` API generates acton creators and action types for each individual reducer function you provide. 

#### 3️⃣ Redux reducers must follow specific rules 
> <li>Should only calculate a new state value based on the state and action arguments</li>
> <li>Must make immutable updates by copying the existing state</li>
> <li>Cannot contain any asynchronous logic or other side effects</li>
> <li>createSlice API uses Immer to allow mutating immutable updates</li>

#### 4️⃣ Async logic is typically written in special functions called `thunks`
> Thunks receive `dispatch` and `getState` as arguments and Redux toolkit enables the redux-thunk middleware by default

#### 5️⃣ React-Redux allows React components to interact with a Redux store
> Wrapping the app with `<Provider store={store}>` enables all components to sue the store. 

#### 6️⃣ Global state should go in the Redux store, local state should stay in React components 
