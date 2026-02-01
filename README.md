# react-optimization-steps

## 1 Use useMemo hook?
- This return momoize value that's why we use it
- useMemo is used to memoize expensive calculations so they don’t run on every render.
```js
// With useMemo hook - So here total will be called once only if setCount change then total will not be called but in below without useMemo code function called all time.
import React, { useMemo, useState } from "react";
import "./style.css";

export default function App() {
  const [count, setCount] = useState(0);

  const total = useMemo(() => {
    console.log("Calculating...");
    let total = 0;
    for (let i = 0; i < 100000000; i++) {
      total += i;
    }
    return total;
  }, []);

  return (
    <>
      <h1>{total}</h1>
      <button onClick={() => setCount(count + 1)}>
        Click {count}
      </button>
    </>
  );
}

// Without useMemo
function App() {
  const [count, setCount] = useState(0);

  const expensiveCalculation = () => {
    console.log("Calculating...");
    let total = 0;
    for (let i = 0; i < 100000000; i++) {
      total += i;
    }
    return total;
  };

  return (
    <>
      <h1>{expensiveCalculation()}</h1>
      <button onClick={() => setCount(count + 1)}>
        Click {count}
      </button>
    </>
  );
}

```

---

## 2 Use useCallback hook.
- returns memoize function.
- You pass a function as a prop to a child component.
- Without useCallback, the function is recreated on every render, causing unnecessary re-renders.
- If will use useCallback then will not be re-created or not re-render that's why will not re-render the children again.

```js
// with useCallback
import React, { useState, useCallback } from "react";

const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");

  return (
    <button onClick={onClick}>
      Child Button
    </button>
  );
});

export default function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Child button clicked");
  }, []);

  return (
    <div>
      <h2>Parent Count: {count}</h2>

      <Child onClick={handleClick} />

      <button onClick={() => setCount(count + 1)}>
        Parent Button
      </button>
    </div>
  );
}


// without useCallback
import React, { useState } from "react";

const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");

  return (
    <button onClick={onClick}>
      Child Button
    </button>
  );
});

export default function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    console.log("Child button clicked");
  };

  return (
    <div>
      <h2>Parent Count: {count}</h2>

      <Child onClick={handleClick} />

      <button onClick={() => setCount(count + 1)}>
        Parent Button
      </button>
    </div>
  );
}

```

---

## 3 use React.memo().
- React.memo is used to prevent unnecessary re-renders of a component when its props have not changed.
- If any state changes on parent then will not re-render will we have not passed as props in children component.

```js
// With React.memo()
const Child = React.memo(function Child({ name }) {
  console.log("Child rendered");
  return <h3>Hello {name}</h3>;
});

// Without React.memo()
function Child({ name }) {
  console.log("Child rendered");
  return <h3>Hello {name}</h3>;
}
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Child name="Prem" />
      <button onClick={() => setCount(count + 1)}>
        Click
      </button>
    </>
  );
}
```

---

## 4 use lazy loading, suspense.
- if we use lazy loading then only download that component only on bundler file.
- If we will not use it then download all components.
- Lazy loading means loading components only when they are needed, instead of loading everything at once.

```js
import React, { Suspense, useState } from "react";

const Dashboard = React.lazy(() => import("./Dashboard"));

export default function App() {
  const [show, setShow] = useState(false);

  return (
    <div>
      <h1>Home Page</h1>

      <button onClick={() => setShow(true)}>
        Load Dashboard
      </button>

      {show && (
        <Suspense fallback={<h2>Loading Dashboard...</h2>}>
          <Dashboard />
        </Suspense>
      )}
    </div>
  );
}
```
