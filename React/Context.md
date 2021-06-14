## 📑 Context 
>Context provides a way to pass data through the component tree without having to pass props down manually at every level. 
>In a typical React application, data is passed top-dow (parent to child) via props.
>On the other hand, Context provides a way to share values like these between components 
>without having to explicitly pass a prop through every level of the tree. 

### 🤔 When to use Context 
>Context is designed to share data that can be considered "global" for a tree of React components,
>such as the current authenticated user, theme, or preferred alnguage. 
> >Context is primarily used when some data needs to be accessible by many components at different nesting levels. 

```jsx
import react, { createContext, useReducer } from 'react';

export const ThemeContext = createContext('light');
```

>Context lets us pass a value deep into the component tree 
>without explicitly threading it through every component. 

<br>

```jsx 
const App = () => {
  render(){
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext>
    );
  }
}
```

>By using a Provider, we can pass the current theme to the tree below.
>Any componenet can read it, no matter how deep it is.

<br>

```jsx
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  )
}
```

>A component in the middle doesn't have to pass the theme down explicitly anymore. 

<br>

```jsx
const ThemedButton = () => {
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

>Assign a contextType to read the current theme context.
>React will find the closest theme Provide above and use its value.
>In this case, the current theme will be dark.

<br>

### API 
#### React.createContext 

```jsx
export const MyContext = createContext(initialState); 
```

>Creates a Context object. When React renders a component that subscribes to this Context object
>it will read the current context value from the closest matching Provider above it in the tree. 

### Context.Provider 

```jsx
<MyContext.Provider value={
  /* some value such as, */
  transactions:state.transactions,
  deleteTransaction,
  addTransaction
  }
```

>Every Context object comes with a Provider React component that allows consuming components 
>to subscribe to context changes. 
>The Provider component accepts a value prop to be passed to consuming components 
>that are descendants of this Provider. One Provider can be connected to many consumers. 
>Providers can be nested to override values deeper within the tree. <br>
>All consumers that are descendants of a Provider will re-render whenever the Provider's value prop changes. 
>The propagation from Provider to its descendant consumers (including .contextType and useContext) 
>is not subject to the shouldComponentUpdate method, so the consumer is updated even when an ancestor 
>component skips an update. 
  
### Context.Consumer

```jsx
<MyContext.Consumer>
  {value => /* render something based on the context value */ }
</MyContext.Consumer>
```

>A React component that subscribes to context changes. 
>Using this component lets you subscribe to a context within a function component.
>The value argument passed to the function will be equal to the value prop of 
>the closest Provider for this context above in the tree. 

<br>

### Context.displayName

```jsx
export const MyContext = createContext(/* some value */);
MyContext.displayName = 'MyDisplayName';

<MyContext.Provider> // "MyDisplayName.Provider" in DevTools
<MyContext.Consumer> // "MyDisplayName.Consumer" in DevTools
```

>Context object accepts a displayName string property.
>React DevTools uses this string to determine what to display for the context. 

