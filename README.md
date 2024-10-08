# React Render Props: _An Easy Guide_

Learn how to implement Render Props in React.js to create reusable and flexible components. This repository contains a detailed guide along with examples to demonstrate key concepts.

## Table of Contents

- Introduction
- Detail
- Examples

## Introduction

The term “render prop” refers to a technique for sharing code between React components using a prop whose value is a function.

[As stated on the React.js official website.](https://legacy.reactjs.org/docs/render-props.html)

## Detail

This definition has two key parts:

1. technique for sharing code between React components
2. using a prop whose value is a function

I'll start by explaining the second part

### _Part 2: Using a prop whose value is a function_

#### Example: 1

> App.tsx

```js
import { FC, Fragment } from "react";
interface RenderProps {
  age: () => number;
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
>
> Above example demonstrates a simple use of Render Props in React,
> In App component a component "PropsRendering": <PropsRendering age={() => 20} /> renders, with prop "age" whose value is a function "()=>20" that is returning the value "20" and inside "PropsRendering" component we can call this function age() that will results 20 and the output on the screen is "Age: 20"

> ##### _Note: The function ()=>20 returns the value 20, which has the type number._

### Passing Arguments

You can also pass the argument(s) in the function

#### Example: 2

> App.tsx

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

> Here's another example of Render Props, this time involving login status and user roles. The App component renders the component PropsRendering with a prop(logged) and passes a function inside this prop. This function, (isLoggedIn) => (isLoggedIn ? "Admin" : "guest"), takes a boolean argument (isLoggedIn) and returns the string "Admin" if isLoggedIn is true; otherwise, it returns "guest".

#### Example: 3

The prop, thats value is a function can also return a component

> App.tsx

```js
import { FC, Fragment } from "react";
interface RenderProps {
  loged: (value: boolean) => React.ReactNode;
}
const PropsRendering: FC<RenderProps> = ({ loged }) => {
  return (
    <Fragment>
      <div> User: {loged(true)}</div>
    </Fragment>
  );
};
const DisplayUser: FC<{ userName: string }> = ({ userName }) => {
  return <Fragment>{userName}</Fragment>;
};
function App() {
  return (
    <div className="App">
      <PropsRendering
        loged={(isLogedin) =>
          isLogedin ? (
            <DisplayUser userName="Admin" />
          ) : (
            <DisplayUser userName="guest" />
          )
        }
      />
    </div>
  );
}
export default App;
```

> ##### _Note: In anbove example the prop "loged" is a function that returning a component <DisplayUser userName="Admin" />_

### _part 1: technique for sharing code between React components_

```js
import { FC, Fragment, useState } from "react";

type HoverTrackerProps = {
  render: (isHovered: boolean) => React.ReactNode,
};

// Create the HoverTracker Component:
// This component tracks whether the mouse is hovering over it and passes this information via the render prop.
const HoverTracker: React.FC<HoverTrackerProps> = ({ render }) => {
  const [isHovered, setIsHovered] = useState(false);

  const handleMouseEnter = () => setIsHovered(true);
  const handleMouseLeave = () => setIsHovered(false);

  return (
    <div onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
      {render(isHovered)}
    </div>
  );
};
// HoverText Component:
// This component changes its text color when hovered.

const HoverText: React.FC = () => {
  return (
    <HoverTracker
      render={(isHovered) => (
        <p style={{ color: isHovered ? "blue" : "black" }}>
          Hover over this text!
        </p>
      )}
    />
  );
};

// HoverButton Component:
// This component changes its background color when hovered.
const HoverButton: React.FC = () => {
  return (
    <HoverTracker
      render={(isHovered) => (
        <button style={{ backgroundColor: isHovered ? "lightgreen" : "gray" }}>
          Hover over this button!
        </button>
      )}
    />
  );
};

// Usage
// We can now use HoverText and HoverButton in the same app, and both will share the hover logic from HoverTracker.
function App() {
  return (
    <div className="App">
      <h1>Simple Render Props Example</h1>
      <HoverText />
      <HoverButton />
    </div>
  );
}
export default App;
```

##### Explanation

> HoverTracker Component: This component tracks the hover state and passes it to its children via the render prop.
> HoverText and HoverButton Components: Both use the hover state to change their style, but they customize their appearance differently. The text changes color, while the button changes background color.
> Usage: You can use HoverText and HoverButton in the app, and both benefit from the reusable hover detection logic.
