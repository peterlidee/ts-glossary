# React hooks

## State

```ts
const [value, setValue] = useState(false);
const [user, setUser] = useState<User | null>(null);
const [user, setUser] = useState<User>({ name: 'Peter' });
const [user, setUser] = useState<User>({} as User);
```

## Reducer

Example

```ts
import { useReducer } from 'react';

type StateType = {
  count: number;
};
type ResetAction = {
  type: 'reset';
};
type UpdateAction = {
  type: 'increment' | 'decrement';
  value: number;
};
// = discriminated union
type ActionType = ResetAction | UpdateAction;

const initialState = { count: 0 };

function reducer(state: StateType, action: ActionType) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + action.value };
    case 'decrement':
      return { count: state.count - action.value };
    case 'reset':
      return initialState;
    default:
      return state;
  }
}

function ReducerStrictType() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <div>
      <strong>ReducerStrictType</strong>
      <span> count: {state.count} </span>
      <button onClick={() => dispatch({ type: 'increment', value: 10 })}>
        + 10
      </button>
      <button onClick={() => dispatch({ type: 'decrement', value: 10 })}>
        - 10
      </button>
      <button onClick={() => dispatch({ type: 'reset' })}>reset</button>
    </div>
  );
}

export default ReducerStrictType;
```

## Context

```ts
import { createContext, useState } from 'react';

type Props = {
  children: React.ReactNode;
};
type ThemeType = 'dark' | 'light';
type ThemeUnknownContextType = {
  theme: ThemeType;
  toggleTheme: () => void;
};

export const ThemeUnknownContext =
  createContext<null | ThemeUnknownContextType>(null);
// we can assert ThemeUnknownContextType to avoid all null checks
// export const ThemeUnknownContext = createContext<ThemeUnknownContextType>({} as ThemeUnknownContextType);

export const ThemeUnknownContextProvider = ({ children }: Props) => {
  const [theme, setTheme] = useState<ThemeType>('dark');
  const toggleTheme = () => {
    setTheme(theme === 'dark' ? 'light' : 'dark');
  };
  return (
    <ThemeUnknownContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeUnknownContext.Provider>
  );
};
```

```ts
import { useContext } from 'react';
import { ThemeUnknownContext } from './ThemeUnknownContext';

export default function UnknownContext() {
  const themeContext = useContext(ThemeUnknownContext);
  const theme = themeContext?.theme;
  const toggleTheme = themeContext?.toggleTheme;

  return (
    <div
      style={
        theme === 'dark'
          ? { background: '#333', color: '#fff' }
          : { background: 'lightgreen', color: '#000' }
      }
    >
      <strong>Unknown context</strong>
      <span>theme: {theme} </span>
      <button onClick={toggleTheme}>toggle theme</button>
    </div>
  );
}
```

## useRef

```ts
import React, { useEffect, useRef } from 'react';

function DomRef() {
  const inputRef = useRef<HTMLInputElement>(null);
  useEffect(() => {
    inputRef.current?.focus();
  }, []);
  return (
    <div>
      <strong>DomRef </strong>
      <input ref={inputRef} />
    </div>
  );
}

export default DomRef;
```

```ts
import { useEffect, useRef, useState } from 'react';

function MutableRef() {
  const [timer, setTimer] = useState(0);
  const intervalRef = useRef<null | number>(null);
  const stopTimer = () => {
    if (intervalRef.current) window.clearInterval(intervalRef.current);
  };
  useEffect(() => {
    intervalRef.current = window.setInterval(() => {
      setTimer((timer) => timer + 1);
    }, 1000);
    return () => stopTimer();
  }, []);
  return (
    <div>
      <strong>MutableRef</strong>
      <span>timer: {timer} </span>
      <button onClick={stopTimer}>stop timer</button>
    </div>
  );
}

export default MutableRef;
```

## useEffect
