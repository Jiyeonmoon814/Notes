## 📁 Fragments 
> A common pattern in React is for a component to return multiple elements.
> Fragments let you group a list of children without adding extra nodes to the DOM. 

### 🤔 Motivation 
> A common pattern is for a component to return a list of children. 

```jsx 
export const Table = () => {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
       </table>
    );
}
```

> `<Columns />` would need to return multiple td elements in order for the rendered HTML to be valid.
> If a parent div was used inside the render() of Columns, then the resulting HTML will be invalid. 

```jsx 
export const Columns = () => {
  return (
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  )
}

// results in a <Table /> output of 
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```

<br>

### Usage 
#### 👏 Fragments solve this problem 

```jsx 
export const Columns = () => {
  return (
    <Fragment>
      <td>Hello</td>
      <td>World</td>
    </Fragment>
  )
}

// which results in a correct <Table /> output of 
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

<br>

#### Short Syntax
> There is a new, shorter syntax you can use for declaring fragments. It looks like empty tags. 
> You can use this the same way you'd use any other element except that it doesn't support keys or attributes. 

```jsx
export const Columns = () => {
  return (
    <>
      <td>Hello</td>
      <td>World</td>
    </>
  )
}
```

<br>

### 🗝️ Keyed Fragments 
> Fragments declared with the explicit `<Fragment>` syntax may have keys.
> A use case for this is mapping a collection to an array of fragments ㅡ for example, to create a description list.
> `Key` is the only attribute that can be passed to Frament.

```jsx
const Glossary = (props) => {
  return (
    <dl>
      {props.items.map(item => (
        // Without the key, React will fire a key warning 
        <Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </Fragment>
      ))}
    </dl>
  )
}
```
