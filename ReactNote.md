# Install react

1. > npx create-react-app my-app

# Basic React Hooks

1. must use hook inside function component
2. always the top level no if / loop...

## useState

> `useState` returns 2 values: the first is the current state (`count`), and the second is a function that updates this state (`setCount`). Below is a simple example to show how `useState` works in React.

Example:

```js
import React, {useState} from `react`;
//!! useState must return 2 values -- count and setCount
//!! and useState(4) 4 is the default value at the beginning.
const [count, setCount] = useState(4);

function decrementCount(){
    setCount (prevCount => prevCount - 1)
}


function incrementCount(){
    setCount (prevCount => prevCount + 1)
}

return (
    <button onClick = {decrementCount}>-<button>
    <span>{count}<span>
    <button onClick = {incrementCount}>+<button>
)
```

## useEffect

## useReducer

## ...

> The **spread operator** (`...`) is a useful feature in JavaScript, particularly in frameworks like React.<br><br>
> It allows you to expand elements of an array or properties of an object into a new array or object. This is especially handy for creating copies of arrays or objects, concatenating or combining arrays, and passing properties to a component as props. It's a concise and convenient way to handle various operations in JavaScript and React.

---

<details>
    <summary>"Form" Library</summary>
    <a href="https://formik.org">FORMIK doc</a>
    <br>
    <a href="https://react-hook-form.com">React Hook Form</a>
</details>

<details>
    <summary>"Hook" Usecase</summary>
    <a href="https://usehooks.com">useHooks</a>

</details>

---
