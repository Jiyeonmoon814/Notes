## ⚡useReducer 

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

>An alternative to useState. Accepts a reducer of type (state, action) => newState, <br>
>and returns the current state paired with a dispatch method.<br>
>useReducer is usually preferable to useState when you have complex state logic 
>that involves multiple sub-values or when the next state depends on the previous one.
>useReducer also lets you optimize performance for components that trigger deep updates 
>because you can pass dispatch down instead of callbacks.

<br>

##### ✔️ Initial State

```jsx 
const initialState = {count: 0};
```

<br>

>You can also create the initial state lazily. 

```jsx
function init(initialCount) {
  return { count : initialCount };
}
```

>To do this, you can pass an init function 
>as the third argument. The initial state will be set to init(initialArg).<br>
>It lets you extract the logic for calculating the initial state outside the reducer. <br>
>This is also handy for resetting the state later in response to an action 

<br>

##### ✔️ Define how we specify the application state changes in response to certain actions to our stored context 

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment' :
      return {count: state.count + 1};
    case 'decrement' :
      return {count: state.count - 1};
    case 'reset' :
      return init(action.payload);
    default :
      throw new Error();
  }
}
```

<br>

##### ✔️ Create provider component and action 

```jsx 
function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  return (
    <>
      Count : {state.count}
      <button onClick={() => dispatch({type:'reset', payload:initialCount})}>
        Reset
      </button>
      <button onClick={() => dispatch({type:'decrement'})}>-</button>
      <button onClick={() => dispatch({type:'increment'})}>+</button>
     )
 }
```

<br>

### ⚡ useReducer could be the way to avoid callbacks down 
> In large component trees, an alternative we recommend is to pass down a dispatch function
> from useReducer via context 

<br>

```jsx 
const TodoDispatch = React.createContext(null);

function TodosApp(){
  const [todos, dispatch] = useReducer(todosReducer);
  
  return(
    <TodosDispatch.Provider value={dispatch}>
      <DeepTree todos={todos} />
    </TodosDispatch.Provider>
  );
}
```

<br>

>'dispatch' won't change between re-renders. <br>
>Any child in the tree inside TodosApp can use the dispatch function to pass actions up to TodosApp. 

```jsx
function DeepChild(props){
  //If we want to perform an action, we can get dispatch from context. 
  const dispatch = useContext(TodosDispatch);
  
  function handleClick() {
    dispatch({type:'add', text:'hello'});
  }
  
  return (
    <button onClick={handleClick}>Add todo</button>
  );
}
```

>This is both more convenient from the maintenance perspective (no need to keep forwarding callbacks),
>and avoids the callback problem altogether. Passing dispatch down like this is the recommended pattern for deep updates.
> >You can still choose whether to pass the application state down as props (more explicit)<br>
> >or as context (more convenient for very deep updates).

>If you use context to pass down the state too, use two different context types ㅡ 
>the dispatch context never changes, so components that read it don't need to re-render 
>unless they also need the application state.
