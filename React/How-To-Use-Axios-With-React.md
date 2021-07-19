## 🙄 How to Make a GET Request

```jsx
import axios from "axios";
import React, { useState, useEffect } from "react";

const baseURL = "https://jsonplaceholder.typicode.com/posts/1";

export default function App() {
 const [post, setPost] = useState('')
 
 useEffect(() => {
  axios.get(baseURL)
       .then((res) => {
          setPost(res.data)
        })
   },[])
 
 if(!post) return null

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </div>
  );
}
```

> To perform this request when the component mounts, you use the useEffect hook.
> This involves Axios, using the `.get()` method to make a GET request to your endpoint, and using a .then() callback
> to get back all of the response data. <br>
> The response is returned as an object. The data(which is in this case a post with id, title, and body properties)
> is put in a piece of state called post which is displayed in the component. 

<br>

## 🤔 How to Make a POST Request

```jsx
import axios from "axios";
import React, { useState, useEffect } from "react";

const baseURL = "https://jsonplaceholder.typicode.com/posts";

export default function App() {
 const [post, setPost] = useState('')
 const [error, setError] = useState('')
 
 useEffect(() => {
  axios.get(`${baseURL}/1`).then((res) => {
    setPost(res.data)
  })
 },[])
 
 function createPost() {
  axios.post(baseURL, {
    title : 'Hi', body: 'This is a new post.'
      }).then((res) => {
        setPost(res.data);
      }).catch(error => {
          setError(error);
        });
      }
 
 if(error) return `Error: ${error.message}`
 if(!post) return 'No post!'

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={createPost}>Create Post</button>
    </div>
  );
}
```

> When you click on the button, it calls the createPost function.
> To make that POST request with Axios, you use the `.post()` method. As the second argument,
> you include an object property that specifies what you want the new post to be. 
> Then use a .then() callback to get back the response data and replace the first post you got with the new post you requested.
> This is very similar to the `.get()` method, `but the new resource you want to create is provied as the second argument after the API endpoint.`

<br>

## 🧐 How to Make a PUT Request
> To update a given resource, make a PUT Request. This time, the button will call a function to update a post.

<br>

```jsx
import axios from "axios";
import React, { useState, useEffect } from "react";

const baseURL = "https://jsonplaceholder.typicode.com/posts";

export default function App() {
 const [post, setPost] = useState('')
 const [error, setError] = useState('')
 
 useEffect(() => {
  axios.get(`${baseURL}/1`).then((res) => {
    setPost(res.data)
  })
 },[])
 
 function updatePost() {
  axios.put(`${baseURL}/1`, {
    title : 'Hi', body: 'This is an updated post.'
      }).then((res) => {
          setPost(res.data);
      }).catch(error => {
          setError(error);
        });
      }
 
 if(error) return `Error: ${error.message}`
 if(!post) return 'No post!'

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={updatePost}>Update Post</button>
    </div>
  );
}
```

> In the code above, you use the PUT method from Axios. And like with the POST method,
> you include the properties that you want to be in the updated resource. Again, using the .then() callback,
> you update the JSX with the data that is returned. 

<br>

## 🤨 How to Make a DELETE Request (+ Create an Axios Instance)
###### 💡 Note that you do not need a second argument whatsoever to perform this request 
###### ❗ This time, We will create an instance with the `.create()` method, Axios will remember that baseURL, plus other values you might want to specify for every request, including headers.

```jsx
import axios from "axios";
import React, { useState, useEffect } from "react";

// * create an Axios Instance
const client = axios.create({
  baseURL : "https://jsonplaceholder.typicode.com/posts"
})

export default function App() {
 const [post, setPost] = useState('')
 const [error, setError] = useState('')
 
 useEffect(() => {
  // since we declare client above, we can write client.get("/1") instead of axios.get(`${baseURL}/1`)
  client.get("1").then((res) => {
    setPost(res.data)
  })
 },[])
 
 function deletePost() {
  client.delete("/1")
       .then(() => {
          alert('Post is successfully deleted')
          setPost('')
     }).catch(error => {
      setError(error);
    });
  }
 
 if(error) return `Error: ${error.message}`
 if(!post) return 'No post!'

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={deletePost}>Delete Post</button>
    </div>
  );
}
```

> In most cases, you do not need the data that's returned from the `.delete()` method.
> But in the code above, the .then() callback is still used to ensure that your request is successfully resolved.
> In the code above, after a post is deleted, the user is alerted that it was deleted successfully.
> Then, the post data is cleared out of the state by setting it to its initial value of null.
> Also, once a post is deleted, the text "No post" is shown immediately after the alert message. 

<br>

## 🤓 How to Use the Async-Await Syntax with Axios 

```jsx
import axios from "axios";
import React, { useState, useEffect } from "react";

const client = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com/posts" 
});

export default function App() {
  const [post, setPost] = useState('');

  useEffect(() => {
    async function getPost() {
      const res = await client.get("/1")
      setPost(res.data)
    }
    getPost()
  }, []);
  
  async function deletePost() {
    awiat client.delete("/1")
    alert("Post is successfully deleted")
    setPost('')
  }
  
  if (!post) return "No post!"

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
      <button onClick={deletePost}>Delete Post</button>
    </div>
  );
}
```

> In useEffect, there's an async function called getPost. Making it async allows you to use the await keyword
> to resolve the GET request and set that data in state on the next line `without the .then() callback.`
> Note that the getPost function is called immediately after being created. <br>
> Additionally, the deletePost function is now async, which is a requirement to use the await keyword which resolves the promise it 
> returns (every Axios method returns a promise to resolve.) After using the await keyword with the DELETE request, 
> the user is alerted that the post was deleted, and the post is set to null. 

<br>

## 😎 How to Create a Custom useAxios Hook
> Instead of using useEffect to fetch data when the component mounts, you could create your own custom hook with Axios
> to perform the same operation as a reusable function. <br>
> First, install the package `npm install use-axios-client`

<br>

```jsx 
import { useAxios } from "use-axios-client"

export default function App() {
  const { data, error, loading } = useAxios({
    url: "https://jsonplaceholder.typicode.com/posts/1"
  })
  
  if (loading || !data) return "Loading..."
  if (error) return "Error"
  
  return (
    <div>
      <h1>{data.title}</h1>
      <p>{data.body}</p>
    </div>
  )
}
```

> Now you can call useAxios at the top of the app component, pass in the URL you want to make a request to,
> and the hook returns an object with all the values you need to handle the different states : `loading, error` and the resolved `data`.
> In the process of performing this request, the value loading will be true. If there's an error, you'll want to display that error state.
> Otherwise, if you have the returned data, you can display it in the UI. 
