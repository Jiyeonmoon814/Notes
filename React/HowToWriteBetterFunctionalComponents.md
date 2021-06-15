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
>to optimized child components that rely on reference equality to prevent unnecessary renders(e.g. shouldComponentUpdate)

```jsx
const { useMemo, useState } = React;
```


