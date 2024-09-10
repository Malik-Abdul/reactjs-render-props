# React Render Props
## _A Comprehensive Guide_
Learn how to implement Render Props in React.js to create reusable and flexible components. This repository contains a detailed guide along with examples to demonstrate key concepts.

## Table of Contents

- Introduction
- What are Render Props?
- Basic Example
- Advanced Example
- Best Practices
- Conclusion

## Introduction
The term “render prop” refers to a technique for sharing code between React components using a prop whose value is a function.
[from React.js official website.](https://legacy.reactjs.org/docs/render-props.html)

Let me break this defination
 - technique for sharing code between React components
 - using a prop whose value is a function

Firs I will explain the second part

### _using a prop whose value is a function_
>App.tsx
```js
import { FC, Fragment } from "react";
interface RenderProps {
  age: (value: boolean) => number;
}
const PropsRendering: FC<RenderProps> = ({ age }) => {
  return (
    <Fragment>
      <div> Age: {age()}</div>
    </Fragment>
  );
};
function App() {
  return (
    <div className="App">
     <PropsRendering age={() => 20} />
    </div>
  );
}
export default App;
```
> ##### _Output: Age: 20_
Above example demonstrates a simple use of Render Props in React,
In App component a component "PropsRendering": <PropsRendering age={() => 20} /> renders, with prop "age" whose value is a function "()=>20" that is returning the value "20" and inside "PropsRendering" component we can call this function age() that will results 20 and the output on the screen is "Age: 20"

## Passing Arguments
You can also pass the argument(s) in the function
>App.tsx
```js
import { FC, Fragment } from "react";

interface RenderProps {
  loged: (value: boolean) => string;
}
const PropsRendering: FC<RenderProps> = ({ loged }) => {
  return (
    <Fragment>
      <div> User: {loged(true)}</div>
    </Fragment>
  );
};
function App() {
  return (
    <div className="App">
      <PropsRendering loged={(isLogedin) => (isLogedin ? "Admin" : "guest")} />
    </div>
  );
}
export default App;
```
> ##### _Output: User: Admin_



