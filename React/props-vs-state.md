## ⚡ props vs state
>The main responsibility of a Component is to translate raw data into rich HTML.
>With that in mind, the props and the state together constitute the raw data
>that the HTML output derives from. <br>
>You could say props + state is the input data for the render() function of a Component, 
>so we need to zoom in and see what each data type represents and where does it come from. 

<br>

>Before separating props and state, let's also identify where they overlap. 
><li>Both props and state are plain JS objects </li>
><li>Both props and state changes trigger a render update </li>
><li>Both props and state are deterministic. If your Component generates different outputs for the same combination of props and state then you're doing something wrong.</li>

> ‼️ If a component needs to alter one of its attributes at some point in time,
>that attribute should be part of its state, otherwise it should just be a prop for that Component. 

<br>

## ☀️ Props
>props (short for properties) are a Components's configuration, its options if you may.
>They are received from above and immutable as far as the Component receiving them is concerned.
><strong>A Component cannot change its props, </Strong> but it is responsible for putting together
>the props of its child Components.

## 🌝 State
>The state starts with a default value when a Component mounts and then 
><Strong>suffers from mutations in time (mostly generated from user events).</Strong>
>It's a serializable representation of one point in time ㅡ a snapshot.
>A component manages its own state internally, but ㅡ besides setting an initail state ㅡ 
>has no business fiddling with the state of its children. You could say <Strong> the state is private </Strong>

<br>

## ⭕ What props and state both can do 
> <li>props and state can get initial value from parent Component. </li>
> <li>props and state can set initial value for child Components. </li>
> <li>props and state can set default values inside Component. </li> 
> 📝 both props and state initial values received from parents override default values 
> defined insid a Component.

## 🙄 What only props can do 
> <li>props can be changed by parent Component. </li>
> <li>props can change in child Components. </li>

## 🧐 What only state can do 
> <li>state can change inside Component. </li>

<br>

## 🤔 Should this Component have state? 
> state is optional. Since state increases complexity and reduces predictability, 
> a Component without state is preferable. Even though you clearly can't do without state 
> in an interactive app, you should avoid having too many Stateful Components. 

### Stateless Component : props :o: state :x:
> <Strong>Only props, no state.</Strong> There's not much going on besides the render() function
> and all their logic revolves around the props they receive. This makes them very easy to follow
> (and test for that matter).

### Stateful Component : props & state :o:
> <Strong>Both props and state.</Strong> We also call these state managers. 
> They are in charge of client-server communication (XHR, web sockets, etc.), 
> processing data and responding to user events. These sort of logistics should be ensapsulated in
> a moderate number of Stateful components, while all visualization and formatting logic should move
> downstream into as many Stateless components as possible.
