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
