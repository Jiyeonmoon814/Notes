## Composition
>React has a powerful composition model, and we recommend using composition 
>instead of inheritance to reuse code between components. 

## Containment
>Some components don't know their children ahead of time. This is especially common
>for components like Sidebar or Dialog that represent generic boxes.

>Recommend that such components use the special children prop to pass 
>children elements directly into their output. 

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

>This lets other components pass arbitrary children to them by nesting the JSX.

```jsx
function WelcomeDialog(){
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting my github!
      </p>
    </FancyBorder>
  );
}
```

>Anything inside the FancyBorder JSX tag gets passed into the FancyBorder component as a children prop.
><Strong>Since FancyBorder renders {props.children} inside a div, the passed elements appear in the final output.</Strong>

<br>

>While this is less common, sometimes you might need multiple holes in a component.
>In such cases you may come up with your own convention instead of using children

```jsx
function SplitPane(props){
  return(
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.left}
      </div>
    </div>
  );
}

function App(){
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

>React elements like Contacts and Chat are just objects,
>so you can pass them as props like any other data. This approach may remind you of slots
>in other libraries but there are no limitations on what you can pass as props in React.

<br>

## Specialization
>Sometimes we think about components as being special cases of other components.
>For example we might say that a WelcomeDialog is a specail case of Dialog.

<br>

>In React, this is also achieved by composition, where a more specific component renders
>more generic one and configures it with props. 

```jsx 
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting my github!" />
  );
}
```

