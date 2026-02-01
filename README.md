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
