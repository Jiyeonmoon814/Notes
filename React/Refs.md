## ⚡ Forwarding Refs
>Ref forwarding is an opt-in feature that lets some components take a ref they receive,
>and pass it further down (in other words, "forward" it) to a child.


## Forwarding refs to DOM components 
>Consider a FancyButton component that renders the native button DOM element

```jsx
function FancyButton(props) {
  return (
    <button className="FancyButton">
      {props.children}
    </button>
  );
}
```

>React components hide their implementation details, including their rendered output. 
>Other components using FancyButton <strong>usually will not need to obtain a ref to the inner button DOM element.</strong>
>This is good because it prevents components from relying on each other's DOM structure too much.

>Although such encapsulation is desirable for application-level components like FeedStory or Comment,
>it can be inconvenient for highly resuable leaf components like FancyButton or myTextInput.
>These components tend to be use throughout the application in a similar manner as a regular DOM button and input,
>and accessing their DOM nodes may be unavoidable for managing focus, selection, or animations. 

<br>

>1️⃣ Creat a React ref by calling React.createRef and assign it to a ref variable.

```jsx
const ref = React.createRef();
```

>2️⃣ Pass our ref down to FancyButton ref={ref} by specifying it as a JSX attribute.

```jsx
<FancyButton ref={ref}>Click Me!</FancyButton>;
```


>3️⃣ React passes the ref to the (props, ref) => ... function inside forwardRef as a second argument.


> ❗ This ref argument only exists when you define a component with React.forwardRef call. Regular function or class components
> don't receive the ref argument, and ref is not available in props either.

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  
));
```


>4️⃣ Forward this ref argument down to button ref={ref} by specifying it as a JSX attribute.

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));
```


>5️⃣ When the ref is attached, ref.current will point to the button DOM node.

<br>

>This way, components using FancyButton can get a ref to the underlying button DOM node 
>and access it if necessary just like if they used a DOM button directly.


```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

//You can now get a ref directily to the DOM button
const ref = React.createRef();
<FancyButton ref={ref}>Click Me!</FancyButton>;
```

<br>

<br>

## 🤔 Is there something like instance variables?
>The <strong>useRef()</Strong> Hook isn't just for DOM refs. The ref object is a generic container
>whose current property is mutable and can hold any value, similar to an instance property on class.
>You can write to it from inside useEffect : 

```jsx
function Timer() {
  const intervalRef = useRef();
  
  useEffect(() => {
    const id = setInterval(() => {
      // ...
    });
    intervalRef.current = id;
    return () => {
      clearInterval(intervalRef.current);
    };
  })'
}
```

<br>

>If we just wanted to set an interval, we wouldn't need the ref(since id could be local to the effect),
>but it's useful if we want to clear the interval from an event handler like this 

```jsx
//...
function handleCancelClick() {
  clearInterval(intervalRef.current);
}
//...
```

>Conceptually, you can think of refs as similar to instance variables in a class. 
>Unless you're doing lazy initialization, avoid setting refs during rendering.
>Instead, typically you want to modify refs in event handlers and effects. 

<br>

## 🙄 How to get the previous props or state?

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  const prevCountRef = useRef();
  useEffect(() => {
    prevCountRef.current = count;
  });
  const prevCount = prevCountRef.current;
  
  return <h1> Now:{count}, Before: {prevCount} </h1>;
}
```

<br>

>This might be a bit convoluted but you can extract it into a custom Hook.

>useRef returns a mutable ref object whose .current property is initialized to the passed argument(initialValue).
>The returned object will persist for the full lifetime of the component.

```jsx 
function Counter() {
  const [count, setCount] = useState(0);
  //***
  const prevCount = usePrevious(count);
  //***
  return <h1>Now: {count}, Before:{prevCount}</h1>;
}

//useRef inside useEffect ***
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

<br>

>Note how this would work for props, state, or any other calculated value.

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  const calculation = count + 100;
  const prevCalculation = usePrevious(calculation);
  //...
}
```

<br>

## Displaying a custom name in DevTools 
>React.forwardRef accepts a render function. React DevTools uses this function to determine 
>what to display for the ref forwarding component.

>For example, the following component will appear as "ForwardRef" in the DevTools

```jsx
const WrappedComponent = React.forwardRef((props, ref) => {
  return <LogProps {...props} forwardedRef={ref} />;
});
```

>If you name the render function, DevTools will also include its name(e.g. "ForwardRef(myFunction)")

```jsx
const WrappedComponent = React.forwardRef(
  function myFunction(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }
);
```

>You can even set the function's displayName property to include the component you're wrapping

```jsx
function logProps(component) {
  class LogProps extends React.Component {
    //...
  }
  
function forwardRef(props, ref) {
  return <LogProps {...props} forwardRef={ref} />;
}

//Give this component a more helpful display name in DevTools.
// e.g. "ForwardRef(logProps(MyComponent))"
const name = Component.displayName || Component.name;
forwardRef.displayName = `logProps(${name})`;

return React.forwardRef(forwardRef);
}
```
