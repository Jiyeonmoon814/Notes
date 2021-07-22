## 📝 useHistory 
> This hook gives you access to history objects and you have access to several functions
> to navigate your page. It's all about navigation. 

```jsx
import { useHistory } from 'react-router-dom';

export const Portfolio = (props) => {
    const history = useHistory();
    
    return (
        <div>
            Portfolio
        </div>
    );
};
```

<br> 

##### the most common uses of attributes
<li> length(number) : the length of the page that you visited. </li>
<li> action(stirng): (POP, PUSH, REPLACE) </li>

> `POP` : Visiting the route via url, using history go function
> (`history.goBack(), history.goForward(), history.go()`) <br>
> `PUSH` : Using `history.push()` <br>
> `REPLACE` : Using `history.replace()`

<li>.push(pathname: string, state : any) / (location : object) : push a path or location to the history stack. </li>

```jsx
// using pathname
history.push("/blog");
// https://localhost:3000/blogs 

// using pathname and state 
history.push("/blog", { fromPopup : true })
// https://localhost:3000/blogs 

//using location
history.push({
  pathname: "/blogs",
  search: "?id=5",
  hash: "#react",
  state : { fromPopup : true }
})
// https://localhost:3000/blogs?id=5#react 
```
