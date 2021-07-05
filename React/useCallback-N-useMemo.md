>Before we start, we should remember that <br>
##### 💡 A functional component is just a function which returns JSX. 
##### 💡 Rendering component means something calls the component. Everytime a function is excuted, the internally declared expression(const, another func, etc.) is also re-declared and then used. 
##### 💡 Components are re-rendered whenever their state is changed or the props received from their parents are changed. 

<br>

## 🤙 useCallback 
>Returns a memoized callback. 

```jsx
const memolizedCallback = useCallback (
  () => {
    doSomething(a,b);
  },
  [a, b],
);
```

<br>

>Pass an inline callback and an array of dependencies.
><Strong>useCallback will return a memoized version of the callback that only changes if one of the dependencies has changed.</Strong>
>This is useful when passing callbacks to optimized child components that rely on reference equality
>to prevent unnecessary renders(e.g. shouldComponentUpdate).

>useCallback(fn, deps) is equivalent to useMemo(() => fn, deps). 

<br>

## 📝 useMemo
>Returns a memoized value. 

```jsx
const memoizedValue = useMemo(() => computedExpensiveValue(a, b), [a, b]);
```

<br>

>Pass a create function and an array of dependencies. useMemo will only recompute the memoized value
>when one of the dependencies has changed. This optimization helps to avoid expensive calculations on every render.
>Remember that <Strong>the function passed to useMemo runs during rendering.</Strong>
>Don't do anything there that you wouldn't normally do while rendering. For example,
>side effects belong in useEffect, not useMemo. 

<br>

>If no array is provided, a new value will be computed on every render. 
><Strong>You may rely on useMemo as a performance optimization, not as a semantic guarantee. </Strong>

<br>

#### 💡 Note
><Strong>The array of dependencies is not passed as arguments to the callback or function.</Strong>
>Conceptually, though, that's what they represent : every value referenced inside the callback
>should also appear in the dependencies array. In the future, a sufficiently advanced compiler could create
>this array automatically. 
 
