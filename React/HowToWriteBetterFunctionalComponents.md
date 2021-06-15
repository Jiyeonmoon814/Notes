## 🤔How to write better Functional components in React 

### 📍 Memoize Complex data by using useMemo
>useMemo returns a memoized value. It will only recompute the memoized value
>when one of the dependencies has changed. This optimization helps to avoid expensive calculations on every render. 

```jsx
import React from 'react';

function SortedListView ({ title, items, comparisonFunc }) {
  const sortedItems = [...items];
  sortedItems.sort(comparisonFunc);
  return (
    <div>
      <h1> {title} </h1>
      <ul>
        {sortedItems.map(item => <li> {item} </li>)}
      </ul>
    </div>
  );
}
```

>This component takes an array of items, sorts them according to the comparison function provided, and display them.
>However, this sorting operation could take a large amount of time if the number of items is larger or
>the comparison function is complicated. This could end up becoming a bottleneck because the component will re-sort
>the items on each re-render even if the items array or the comparisonFunc does not change, 
>but some other prop or the state changes.

<br>

>Memoization can easily be accomplished with the useMemo Hook

```jsx 
import React, { useMemo } from 'react';

function SortedListView ({ title, items, comparisonFunc }) {
  const sortedItems = useMemo(() => {
    const sortedItems = [...items];
    sortedItems.sort(comparisonFunc);
    return sortedItems;
  }, [items, comparisonFunc]);
  return (
    ...
  );
}
```

>Memoizing can help prevent unnecessary re-renders when objects generated 
>in a parent component have to be passed to a child component. 
>For components under our own control, we can always prevent unnecessary re-renders. 
> >You do not need to use useMemo in instances where the operations are not expensive
> >or the object is not passed into any other component. 

<br> 

### 📍Memoize Callback function by using useCallback
>useCallback returns a memoized callback. It will return a memoized version of the callback
>that only changes if one of the dependencies has changed. This is useful when passing callbacks 
>to optimized child components that rely on reference equality to prevent unnecessary renders(e.g. shouldComponentUpdate).

```jsx
const { useMemo, useState } = React;

function SortController ({items}) {
  const [isAscending, setIsAscending] = useState(true);
  const [title, setTitle] = useState('');
  const ascendingFn = (a, b) => a < b ? -1 : (b > a ? 1 : 0);
  const descendingFn = (a, b) => b < a ? -1 : (a > b ? 1 : 0);
  const comparisonFunc = isAscending ? ascendingFn : descendingFn;
  return (
    <div>
      <input placeholder='Enter Title' value={title}
             onChange = { e => setTitle(e.target.value)} />
      <button onClick = {() => setIsAscending(true)}>
        Sort Ascending 
      </button>
      <button onClick = {() => setIsAscending(false)}>
        Sort Descending
      </button>
      <SortedListView title={title} items={items} comparisonFunc={comparisonFunc} />
    </div>
  );
}

  function SortedListView({ title, items, comparisonFunc }) {
    const sortedItems = useMemo(() => {
      const sortedItems = [...items];
      sortedItems.sort(comparisonFunc);
      return sortedItems;
    }, [items, comparisonFunc]);
    return (
      <div>
        <h1> {title} </h1>
        <ul> 
          {sortedItems.map(item => <li> {item} </li>)}
        </ul>
      </div>
    );
  }
  
  const items = [5,6,2,100,4,23,12,34]
  
  ReactDOM.render(
    <SortController items={items} />,
    document.querySelector('#root')
  )
```
>In the example above, if you go to the Result tab and type a new title in the text field, 
>it will cause SortController to re-render. As a result, ascendingFn and descendingFn will be recreated.
>This causes comparisonFunc to change. Since the useMemo in SortedListView depends on comparisonFunc,
>it will re-sort items even if the comparisonFunc does not change logically. 

<br>

>We can solve this problem by wrapping ascendingFn and descendingFn in useCallback. 
>This Hook is used to memoize functions. Note that we do not pass anything in the dependency array 
>for useCallback here because they do not rely on anything inside the component.
> > Just like useMemo, you do not need to use useCallback if the function is not passed into 
> > any component and is not a dependency for any other hook. 

```jsx
const { useMemo, useState, useCallback } = React 

function SortController ({ items }) {
  ...
  const ascendingFn = useCallback(
    (a, b) => a < b ? -1 : (b > a ? 1 : 0),
    []
  );
  const descendingFn = useCallback(
    (a, b) => b < a ? -1 : (a > b ? 1 : 0),
    []
  );
  const comparisonFunc = isAscending ? ascendingFn : descendingFn; 
  return(
    ...
  )
}
```

<br>

### 📍 Decouple functions that don't rely on the component 
>Another improvement that we can make in the function above is to actually move
>ascendingFn and descendingFn outside of SortController. This is because these functions 
>wukk bit be recreated ib evert re-render. 

```jsx
function ascendingFn (a, b) {
  return a < b ? -1 : (b > a ? 1 : 0);
}

function descendingFn (a, b) {
  return b < a ? -1 : (a > b ? 1 : 0);
}
```

<br> 

### 📍 Create Subcomponents 
>Creating subcomponents is a useful way to write optimized and readable React code. 
>Subcomponents divide the code base into smaller, digestible, and reusable chunks. 

<br>

### 📍 Create and Reuse Custom hooks 
>Just like components, we can create reusable Hooks. 

```jsx 
const { useMemo, useState, useCallback } = React 

function SortedListView ({ title, items, comparisonFunc }) {
  const sortedItems = useSorted(items, comparisonFunc)
  return (
    <div>
      <h1> {title} </h1>
      <ul>
        {sortedItems.map(item => <li> {item} </li>)}
      </ul>
    </div>
  );
}

function useSorted (items, comparisonFunc) {
  return useMemo(() => {
    const sortedItems = [...items];
    sortedItems.sort(comparisonFunc);
    return sortedItems;
  }, [items, comparisonFunc]);
}

function SortController ({ itmes }) {
  const [isAscending, setIsAscending] = useState(true); 
  const [title, setTitle] = useState('');
  const comparisonFunc = isAscending ? ascendingFn : descendingFn;
  return (
    <div>
      <input placeholder = 'Enter Title' value={title} 
          onChange={e => setTitle(e.target.value)} />
      <button onClick={() => setIsAscending(true)}>
        Sort Ascending 
      </button>
      <button onClick={() => setIsAscending(false)}>
        Sort Descending
      </button>
      <SortedListView title={title} items={items} comparisonFunc={comparisonFunc} />
    </div>
  );
}

function ascendingFn (a, b) {
  return a < b ? -1 : (b > a ? 1 : 0);
}

function descendingFn (a, b) {
  return b < a ? -1 : (a > b ? 1 : 0);
}

const items = [5,6,2,100,4,23,12,34]

ReactDOM.render(
  <SortController items={items} />,
  document.querySelector('#root')
)
```
