### Using State Correctly 

#### Do not modify state directly
>The only place where you can assign this.state in the constructor. 

```jsx
//Correct 
this.setState({comment:'Hi'});

//Wrong 
this.setState.comment = 'Hello';
```

<br>

#### State updates may be asynchronous
>React may batch multiple setState() calls into a single update for performance.
>Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state. 

```jsx 
//Correct
this.setState((state, props) => ({
  counter : state.counter + props.increment
}));

//Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

<br>

#### State updates are merged 
>When you call setState(), React merges the object you provide into the current state. 
>For example, your state may contain several independent variables

```jsx
constructor(props) {
  super(props);
  this.state = {
    posts: [],
    comments: []
  };
}
```

<br>

>Then you can update them independently with seperate setState() calls.
>The merging is hallow, so this.setState({comments}) leaves this.state.posts intact,
>but completely replaces this.state.comments.

```jsx
componentDidmount(){
  fetchPosts().then(response => {
    this.setState({
      posts: response.posts
    });
  });
  fetchComments().then(response => {
    this.setState({
      comments: response.comments
    });
  });
}
```

<br>

### The Data flows down 
>Neither parent nor child components can know if a certain component is stateful or stateless,
>and they shouldn't care whether it is defined as a function or a class. 
>This is why state is ofte called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.
>A component may choose to pass its state down as props to its child components.

```jsx
<FormattedDate date={this.state.date} />
```

<br>

>The FormattedDate component would receive the date in its props and wouldn't know 
>whether it came from the Clock's state, from the Clock's props, or was typed by hand. 

```jsx
const FormattedDate = (props) => {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

<br>

>This is commonly called a "top-down" or "unidirectional" data flow. Any state is always owned 
>by some specific component, and any data or UI derived from that state can only affect components
>below them in the tree. If you imagine a componenet tree as a waterfall of props, each component's state
>is like an additional water source that joins it at an arbitrary point but also flows down. 
>We can see that all components are truly isolated and an App component that renders three <Clock>s 

```jsx
  const App = () => {
    return (
  <div>
    <Clock />
    <Clock />
    <Clock />
  </div>
  );
 }
  
 ReactDOM.render(
  <App />,
  document.getElementById('root')
 )
 ```
  
 >In React apps, whether a component is stateful or stateless is considered an implementation detail of the component
 >that may change over time. You can use stateless components inside stateful components, and vice versa. 
 
