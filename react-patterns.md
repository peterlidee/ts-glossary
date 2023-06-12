# React patterns

<https://github.com/peterlidee/react-in-typescript>

## Children

```ts
type ParentProps = {
  children: React.ReactNode;
};
```

## Component as prop

(Component is not called, just passed as function.)

```ts
import Parent from './Parent';
import Child from './Child';

function ComponentProps() {
  return <Parent component={Child} />;
}
```

```ts
import React from 'react';
import { ChildProps } from './Child';

type ParentProps = {
  component: React.ComponentType<ChildProps>;
};

function Parent({ component: Component }: ParentProps) {
  return <div>Parent {<Component message='hello' />}</div>;
}

export default Parent;
```

## Composition

Called component is called an passed as prop. No types needed.

```ts
import User from './User';
import Child from './Child';

function Parent() {
  const user = <User name='Peter' />;
  return (
    <div>
      Parent Component
      <Child user={user} />
    </div>
  );
}

export default Parent;
```

## Render props

Component returns function.

```ts
type User = {
  id: number;
  name: string;
};
type Response = {
  loading: boolean;
  error: undefined | Error;
  data: undefined | { users: User[] };
};
type Props = {
  children: (response: Response) => JSX.Element;
};

function GetData({ children }: Props) {
  // fake API call
  const response: Response = {
    loading: false,
    error: undefined,
    data: {
      users: [
        { id: 1, name: 'Peter' },
        { id: 2, name: 'Wendy' },
      ],
    },
  };
  return children(response);
}

export default GetData;
```

```ts
import GetData from './GetData';

function DisplayData() {
  return (
    <GetData>
      {({ loading, error, data }) => {
        if (loading) return <>Loading...</>;
        if (error) return <>Error {error.message}</>;
        if (data?.users && data.users.length === 0)
          return <div>No users found.</div>;
        return (
          <div>
            {data?.users.map((user) => (
              <span key={user.id}>{user.name}</span>
            ))}
          </div>
        );
      }}
    </GetData>
  );
}

export default DisplayData;
```
