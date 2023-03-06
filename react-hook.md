# useState

Allows us to track state in a function component.

The purpose of this hook to handle reactive data, any data that changes in the application is called state, when any of the data changes, React re-renders the UI.

```
const [count, setCount] = React.useState(0);
```

# useEffect

Allows us to perform side effects in your components.

Checkout the below lifecycle hooks

```
React.useEffect(() => {
  alert('Run when the component mounts and anytime the stateful data changes');
});

React.useEffect(() => {
  alert('Run when the component is first initialized');
}, []);

React.useEffect(() => {
  alert('Run only when count state changes');
}, [count]);

React.useEffect(() => {
  alert('Run when the component is destroyed or before the component is removed from UI');
  return () => alert('Goodbye Component');
});
```

# useContext

Allow us to manage state globally.

State should be held by the highest parent component in the stack that requires access to the state.

We have many nested components. The component at the top and bottom of the stack need access to the state. 

This is the case to use useContext, if not, we need to keep passing down our state.

```
const UserContext = createContext();

function Component1() {
  const [user, setUser] = useState("Jesse Hall");

  return (
    <UserContext.Provider value={user}>
      <h1>{`Hello ${user}!`}</h1>
      <Component2 />
    </UserContext.Provider>
  );
}

function Component2() {
  return (
    <>
      <h1>Component 2</h1>
      <Component3 />
    </>
  );
}

function Component3() {
  const user = useContext(UserContext);

  return (
    <>
      <h1>Component 5</h1>
      <h2>{`Hello ${user} again!`}</h2>
    </>
  );
}
```

# useRef

Allows use to persist values between renders.

It can be used to store a mutable value that does ___not cause a re-render when updated___.

```
function App() {
  const [inputValue, setInputValue] = useState("");
  const count = useRef(0);

  useEffect(() => {
    count.current = count.current + 1;
  });

  return (
    <>
      <input
        type="text"
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
      <h1>Render Count: {count.current}</h1>
    </>
  );
}

```

# useReducer

Allow us to update state using `dispatch`.

It's a different way to manage state using `Redux Pattern`. Instead of updating the state directly, we `dispatch` actions, that go to a reducer function, and this function figure out, how to compute the next state.

```
function reducer(state, dispatch) {
  switch (action.type) {
    case "increment":
      return state + 1;
    case "decrement":
      return state - 1;
    default:
      throw new Error();
  }
}

function useReducer() {
  const [state, dispatch] = React.useReducer(reducer, 0);
  return (
    <>
      Count : {state}
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
    </>
  );
}
```

# useMemo

Allow us to memorize a value.

Think of memoization as caching a value so that it does not need to be recalculated.

```
function useCalculateWithMemo(someNumber: number): number {
  return useMemo(() => {
    return expensiveCalculation(someNumber);
  }, [someNumber]);
}
```

# useCallback

Allow us to memorize callback function.

Think of this hook is like `useMemo`. useMemo memorize a value, useCallback memorize a function.

```
const App = () => {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);

  const increment = () => {
    setCount((c) => c + 1);
  };
  const addTodo = useCallback(() => {
    setTodos((t) => [...t, "New Todo"]);
  }, [todos]);

  return (
    <>
      <Todos todos={todos} addTodo={addTodo} />
      <hr />
      <div>
        Count: {count}
        <button onClick={increment}>+</button>
      </div>
    </>
  );
};

const Todos = ({ todos, addTodo }) => {
  return (
    <>
      <h2>My Todos</h2>
      {todos.map((todo, index) => {
        return <p key={index}>{todo}</p>;
      })}
      <button onClick={addTodo}>Add Todo</button>
    </>
  );
};
```

# useEffect vs useMemo vs useCallback

___useEffect___
It's the alternative for the class component lifecycle methods componentDidMount, componentWillUnmount, componentDidUpdate, etc. You can also use it to create a side effect when dependencies change, i.e. "If some variable changes, do this".

___useMemo___
Remember, this is all about `VALUE`. It will run on every render, but with cached values. It will only use new values when certain dependencies change. It's used for optimization when you have expensive computations.

___useCallback___
On every render, everything that's inside a functional component will run again. If a child component has a dependency on a function from the parent component, the child will re-render every time the parent re-renders even if that function "doesn't change" (the reference changes, but what the function does won't).
It's used for optimization by avoiding unnecessary renders from the child, making the function change the reference only when dependencies change. You should use it when a function is a dependency of a side effect e.g. useEffect.

# useImperativeHandle

Allow us to customize the handle exposed as a ref. (Read `forwardRef` to know more)

We want our parent component to get data from the child component. We can achieve this type of data flow with the `useImperativeHandle`, which allows us to expose a value, state, or function inside a child component to the parent component through ref.

```
const App = () => {
  const ref = useRef(null);

  function handleClick() {
    ref.current.childFunction();
    // ref.current.style.opacity = 0.5;
    // This won't work because the DOM node isn't exposed:
    // To do this forwardRef must return return <input {...props} ref={ref} />;
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);
  const [count, setCount] = useState(0);

  useImperativeHandle(ref, () => {
    return {
      inputFunction() {
        inputRef.current.focus();
        setCount(count + 1);
      },
    };
  });

  return <input {...props} ref={inputRef} value={count} />;
});
```

# useLayoutEffect

Allows us to perform side effects in your components.

`useLayoutEffect` is called before the browser paints those updates for users to see, synchronously

`useEffect` is called after the browser paints those updates, asynchronously

Think of use case is using it with `ref` to calculate `ref` size, position,... before the browser paints

```
const App = () => {
  const [randomNumber, setRandomNumber] = useState(0);

  // Change to useLayoutEffect, It will be less laggy
  useEffect(() => {
    if (randomNumber === 0) {
      setRandomNumber(Math.random() * 1000);
    }
  }, [randomNumber]);

  return (
    <div className="App">
      <p>{randomNumber}</p>
      <button onClick={() => setRandomNumber(0)}>Change value</button>
    </div>
  );
};

useEffect
Click event => Browser paints first (show 0) => useEffect is run => setRandomNumber => Broswer paints second (show the new random number)
useLayoutEffect
Click event => useLayoutEffect is run => setRandomNumber => Broswer paints (show the new random number)
```

# useDeferredValue

Allow us to throttle value

```
export default function App() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  return (
    <>
      <label>
        Search albums:
        <input value={query} onChange={e => setQuery(e.target.value)} />
      </label>
      <Suspense fallback={<h2>Loading...</h2>}>
        <SearchResults query={deferredQuery} />
      </Suspense>
    </>
  );
}
```

# useDebugValue

Allow us to add a label to a custom Hook in React DevTools.

# useInsertionEffect

It is a version of useEffect that fires before any DOM mutations

Read this https://beta.reactjs.org/reference/react/useInsertionEffect
