### Handling Event
>Handling events with React elements is very similar to handling events on DOM elements, but there are some syntax differences
> >React events are named using camelCase, rather than lowercase
> >With JSX you pass a function as the event handler, rather than a string 

<br>

```javascript
//HTML DOM elements
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

<br>
```React
//in React
<button onClick={activateLasers}>
  Activate Lasers
</button>
```
