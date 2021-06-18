## 📍 Specifying the React element type 
>The first part of a JSX tag determines the type of the React element. <br>
>Capitalized types indicate that the JSX tag is referring to a React component. These tags get compiled into
>a direct reference to the named variable.

```jsx
function FooExample(){
  return <Foo />
  //<Foo /> is a JSX expression and it must be in scope
}
```

<br>

### React Must be in scope 
>Since JSX compiles into calls to React.createElement, the React library must also
>always be in scope from your JSX code. 

```jsx
import React from 'react'; 
import CustomButton from './CustomButton';

function WarningButton() {
  //return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```

<br>

### Using Dot notation for JSX Type 
>For example, if MyComponents.DatePicker is a component, you can use it directly from JSX with 

```jsx
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props}{
    return <div>Imagine a {props.color} daterpicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

<br>

### User-Defined Components must be Capitalized
>When an element type starts with a lowercase letter, it refers to a built-in component
>like div or span tag and results in a string 'div' or 'span' passed to React.createElement.
>Types that start with a capital letter with a capital letter compile to React.createElement(smth)
>and correspond to a component defined or imported in your JavaScript file. 

<br>

>Wrong Example 

```jsx
import React from 'react';

//This is a component and should've been capitalized 
function hellop(props) {
  return <div>Hello {props.toWhat} </div>;
}

function HelloWorld(){
  //React thinks <hello /> is an HTML tag bc it's not capitalized 
  return <hello toWhat="World">;
}
```

>Correct Example 

```jsx
import React from 'react';

function Hello(props) {
  //This is use of <div> is legitimate bc div is a valid HTML tag
  return <div>Hello {props.toWhat} </div>;
}

function HelloWorld() {
  return <hello toWhat="World" />;
}
```

<br>

### Choosing the type at Runtime 
>You cannot use a general expression as the React element type. 
>If you do want to use a general expression to indicate the type of the element, 
>just assign it to a capitalized variable first. This often comes up when you want to render
>a different component based on a prop. 

```jsx 
import React from 'react'; 
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo : PhotoStory,
  video : VideoStory
};

//Wrong example
//JSX type can't be an expression 
function Stroy(props) {
  return <components[props.storyType] story={props.story} />;
}

//Correct example
//JSX type can be a capitalized variable 
function Story(props) {
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

### Props in JSX 
>There are several different ways to specify props in JSX.

<br>

#### 1️⃣ JavaScript expressions as props 
>You can pass any JavaScript expression as a prop, by surrounding it with {}.

```jsx
//in this JSX
<MyComponent foo={1 + 2 + 3 + 4} />
```

>For MyComponent, the value of props.foo will be 10 because the expression 1 + 2 + 3 + 4 gets evaluated. 
>if statements and for loops are not expression in JavaScript, so they can't be used in JSX directly.
>Instead, you can put these in the surrounding code.

```jsx
function NumberDescriber(props) {
  let description; 
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
```

<br>

#### 2️⃣ String Literals
>You can pass a string literal as a prop. These two JSX expressions are equivalent.

```jsx
<MyComponent message="hello world" />
<MyComponent message={'hello world'} />
```

>When you pass a string literal, its value is HTML -unescaped.So these two JSX expressions are equivalent.

```jsx
<MyComponent message="&lt;3" />
<MyComponent message={'<3'} />
```

<br>

#### 3️⃣ Props default to "True"
>If you pass no value for a prop, it defaults to true. These two JSX expressions are equivalent.

```jsx
<MyTextBox autocomplete />
<MyTextBox autocomplete={true} />
```

>In general, we don't recommend not passing a value for a prop, 
>because it can be confused with the ES6 object shorthand {foo} which is shorter for {foo: foo}
>rather than {foo: true}. 

<br>

#### 4️⃣ Spread attributes 
>If you already have props as an object, and you want to pass it in JSX, you can use ... as a "spread" operator
>to pass the whold props object. These two components are equivalent. 

```jsx 
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

<br>

>You can also pick specific props that your component will consume while passing
>all other props using the spread operator. 

```jsx 
const Button = props => {
  const { kind, ...other } = props; 
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
}

const App = () => {
  return (
    <div>
      <Button kind="primary" onClick={() => console.log("clicked!")}>
        Hello World!
      </Button>
    </div>
  );
};
```

>In the example above, the kind prop is safely consumed and is not passed on to the button element in the DOM.
>All other props are passed via the ...other object making this component really flexible.
>You can see that it passes an onClick and children props. <br>
>Spread attributes can be useful but they also make it easy to pass unnecessary props to components
>that don't care about them or to pass invalid HTML attributes to the DOM. We recommend using this syntax sparingly. 

<br>

### Children in JSX 
>In JSX expressions that contain both an opening tag and a closing tag, the content between those tags is passed
>as a special prop: props.children. There are several different ways to pass children. 

<br>

#### 1️⃣ String Literals 
>You can put a string between the opening and closing tags and props.children will just be that string.

```jsx
<MyComponent>Hello World</MyComponent>
```

>This is valid JSX, and props.children in MyComponent will simply be the string "Hello world".
>HTML is unescaped, so you can generally write JSX just like you would write HTML in this way. 

```jsx
<div>This is valid HTML &amp; JSX at the same time.</div>
```

<br>

#### 2️⃣ JSX Children 
>You can provide some JSX elements as the children. This is useful for displaying nexted components.

```jsx
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>
```

<br>

>A React component can also return an array of elements.

```jsx 
render() {
  //No need to wrap list items in an extra element!
  <li key="A">First items</li>
  <li key="B">Second items</li>
  <li key="C">Third items</li>
}
```

<br>

#### 3️⃣ JavaScript Expressions as Children
>You can pass any JavaScript expression as children, by enclosing it within {}.
>For example, these expressions are equivalent

```jsx
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```

<br>

>This is often useful for rendering a list of JSX expression of arbitrary length.
>For example, this renders an HTML list

```jsx
function Item(props) {
  return <li>{props.message}</li>
}

function TodoList() {
  const todos = ['finish doc', 'mornging meeting', 'dentist'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}
```

<br>

#### 4️⃣ Functions as Children 
>Normally, JavaScript expressions inserted in JSX will evaluate to a string, a React element, 
>or a list of those things. However, props.children works just like any other prop in that
>it can pass any sort of data, not just the sorts that React knows how to render. 
>For example, if you have a custom component, you could have it take a callback as props.children

```jsx 
//Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i=0;i<props.numTimes;i++){
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  )
}
```

>Children passed to a custom component can be anything, as long as that component transforms them
>into something React can understand before rendering. 
>This usage is not common, but it works if you want to stretch what JSX is capable of. 

<br>

#### 5️⃣ Booleans, Null, and Undefined are ignored
>false, null, undefined, and true are valid children. They simply don't render.
>This can be useful to conditionally render React elements. This JSX renders the Header component only
>if showHeader is true 

```jsx 
<div>
  //Render only if showHeader is true
  {showHeader && <Header />}
  <Content />
</div>
```

<br>

>One caveat is that some "falsy" values, such as the 0 number, are still rendered by React.
>For example, this code will not behave as you might expect because 0 will be printed 
>when props.messages is an empty array

```jsx
<div>
  {props.messages.length &&
    <MessageList messages={props.message} />
   }
</div>
```

>To fix this, make sure that the expression before && is always boolean.

```jsx
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.message} />
   }
</div>
```

>Conversely, if you want a value like false, true, null, or undefined to appear in the output,
>you have to conver it to a string first

```jsx
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```






