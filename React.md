- [What is React](#what-is-react)
- [Hooks](#hooks)
  - [useState](#usestate)
  - [useRef](#useref)
  - [useEffect](#useeffect)
  - [useReducer](#usereducer)
  - [useQuery](#usequery)
- [Custom Hook]()

## What is React?

-A library for building interactive UIs.

## Install react

1. > npx create-react-app my-app

## Basic React Hooks

1. must use hook inside function component
2. always the top level no if / loop...

## Hooks

> **Caution**: Only call it at the `top level` of your component or your `own Hooks`

### `useState`

- Add state variable to component and "remember" value
- It will trigger `re-render of component` if setter changed

[React UseState](https://react.dev/reference/react/useState#usestate)

> `useState` returns 2 values: the first is the current state (`count`), and the second is a function that updates this state (`setCount`).

Example:

```ts
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

---

### `useRef`

- Use for `DOM element` like `<button/>`, `<input/>`,`<div/>`, `<video/>`
- Track value that should not trigger UI updates. E.g. previous state values
- Store `mutable value` that persists across renders without causing re-renders when the value changes.

> **Mutable value**: value can be changed or modified

---

### `useEffect`

- To perform side effect such as fetching data

Example:

```ts
useEffect(() => {}, []);
```

---

### `useReducer`

- To handle a state that have multiple action
- Common practice is using `case` to check the action

---

### `useQuery`

- To `fetch`, handle API request and `state management` such as loading, error, success states
- Auto caching

---

## `Custom Hook`

### ``

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

1. Prop drilling

## map
