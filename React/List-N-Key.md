## 🗝️ Keys
>Keys help React identify which items have changed, are added, or are removed.
>Kyes should be given to the elements inside the array to give the elements a stable identity

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) => 
  <li key={number.toString()}>
    {number}
  </li>
  );
 ```
 
 <br>
 
 >The best way to pick a key is to use a string that uniquely identifies a list item
 >among its siblings. Most often you would use IDs from your data as keys

```jsx
const todoItems = todos.map((todo) => 
  <li key={todo.id}>
    {todo.text}
  </li>
  );
```

<br>

>When you don't have stable IDs for rendered items, you may use the item index as a key as a last resort.
>We don't recommend using indexes for keys if the order of items may change.
>This can negatively impact performance and may cause issues with component state.
>If you choose not to assign an explicit key to list items then Reach will default to using indexes as keys.

```jsx
const todoItems = todos.map((todo, index) => 
  //Only do this if itmes have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
  );
```

<br>

## 🏄‍♀️ Extracting Components with Keys
><Strong>Keys only make sense in the context of the surroundoing array.</Strong>
>For example, if you extract a ListItem component, you should keep the key on the 
>ListItem elements in the array rather than on the li element in the ListItem itself.

<br>

##### :x: Wrong!

```jsx
const ListItem = (props) => {
//Wrong! There is no need to specify the key here*
const value = props.value; 
  return (
    <li key={value.toString()}>
      {props.value}
    </li>
    );
}

const NumberList = (props) => {
  const Numbers = props.numbers;
  const listItems = numbers.map((number) =>
  //Wrong! The key should've been specified here*
  <ListItem value={number} />
  );
  
  return (...);
}
```

##### :o: Corrct!

```jsx
const ListItem = (props) => {
  //Correct! There is no need to specify the key here 
  return <li>{props.value}</li>
}

const NumberList = (props) => {
  //Declare a separate listItems variable and included it in JSX *
  const numbers = props.numbers;
  const listItems = numbers.map((number) => {
  //Correct! Key should be specified inside the array! 
  <ListItem key={number.toString()} value={number} />
  );
  
  return (
    <ul>
      {listItems}
    </ul>
    );
}
  
 const numbers = [1, 2, 3, 4, 5];
 ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
 );
```

<br>

## 🌞 Keys must only be unique among siblings
>Keys used within arrays should be unique among their siblings.
>However, <Strong>they don't need to be globally unique.</Strong>
>We can use the same keys when we produce two different arrays 

```jsx
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) => 
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  
  const content = props.posts.map((post) => 
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.contnet}</p>
    </div>
   );
 }
 
 const posts = [
  {id: 1, title, 'Hi', content: 'Welcome'},
  {id: 2, title: 'Bye', content: 'See u soon'}
 ];
 
 ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
 );
```

<br>

>Keys serve as a hint to React but they don't get passed to your components. 
>If you need the same value in your component, pass it explicitly as a prop with a different name.
>With the example below, the Post component can read props.id, but not props.key.

```jsx 
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
  );
```

<br>

## 📋 Embedding map() in JSX
>In the examples above we declared a separate listItems variable and included it in JSX.
>JSX allows embedding any expression in curly braces so we could inline the map() result.

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) => 
        <ListItem key={number.toString()}
            value={number} />
        )}
     </ul>
  );
}
```

>Sometimes this results in clearer code, but this style can also be abused.
>Like in JavaScript, it's up to you to decide whether it is worth extracting a variable for readability.
>Keep in mind that if the map() body is too nested, it might be a good time to extract a component.

