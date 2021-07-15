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



