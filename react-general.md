# React general

<https://github.com/peterlidee/react-in-typescript>

- [React general](#react-general)
  - [Events](#events)
  - [Style props](#style-props)
  - [React.ComponentProps](#reactcomponentprops)
    - [Use, extend or alter props of html elements](#use-extend-or-alter-props-of-html-elements)
    - [Extract props](#extract-props)
  - [Generics](#generics)
    - [Generic Component with primitives](#generic-component-with-primitives)
    - [Generic components with object](#generic-components-with-object)
  - [Never type](#never-type)

## Events

```ts
type Props = {
  changeHandler: (event: React.ChangeEvent<HTMLInputElement>) => void;
  handleNoParamNoEventClick: () => void;
  handleEventClick: (event: React.MouseEvent<HTMLButtonElement>) => void;
  handleEventAndParamClick: (
    event: React.MouseEvent<HTMLButtonElement>,
    value: string
  ) => void;
};
```

## Style props

```ts
type StylePropsProps = {
  styles: React.CSSProperties;
};
```

## React.ComponentProps

### Use, extend or alter props of html elements

```tsx
type InputProps = React.ComponentProps<'input'>;

function CustomInput(props: InputProps) {
  return (
    <div>
      <strong>copy all props, no custom ones</strong>
      <input {...props} />
    </div>
  );
}

export default CustomInput;
```

```jsx
type CustomButton1Props = {
  variant: 'primary' | 'secondary',
} & React.ComponentProps<'button'>;

function CustomButton1({ variant, children, ...rest }: CustomButton1Props) {
  return (
    <div>
      <strong>extend native props with your own</strong>
      <button className={`btn--${variant}`} {...rest}>
        custom button
      </button>
    </div>
  );
}

export default CustomButton1;
```

```jsx
import React from 'react';

type CustomButton2Props = {
  children: string,
} & Omit<React.ComponentProps<'button'>, 'children'>;

function CustomButton2({ children, ...rest }: CustomButton2Props) {
  return (
    <div>
      <strong>Omit (type utility)</strong>
      <em>(restrict children to string only)</em>
      <button {...rest}>{children}</button>
    </div>
  );
}

export default CustomButton2;
```

### Extract props

```jsx
import React from 'react';
import DisplayUser from './DisplayUser';

function ExtractProps({ user }: React.ComponentProps<typeof DisplayUser>) {
  return (
    <div>
      <h3>Extract Props with React.ComponentProps</h3>
      <em>
        Extract type directly from component, without using the type itself.
      </em>
      <div>
        id: {user.id} name: {user.name.firstName} {user.name.lastName}
      </div>
    </div>
  );
}

export default ExtractProps;
```

## Generics

### Generic Component with primitives

```jsx
type GenericComponentProps<T> = {
  list: T[]
  handler: (item: T) => void
}

function GenericComponent<T extends number | string>({ list, handler }: GenericComponentProps<T>) {
  return (
    <div>
      <strong>Generic Component with primitives</strong>
      {list.map((listItem, index) =>
        <span key={index} onClick={() => handler(listItem)} style={{ margin: '0 3px' }}>{listItem}</span>)}
    </div>
  )
}

export default GenericComponent
```

### Generic components with object

```jsx
import { PersonType } from './people';

type GenericComponentAdvancedProps<T> = {
  list: T[];
  handler: (item: T) => void;
}

function GenericComponentAdvanced<T extends PersonType>({ list, handler }: GenericComponentAdvancedProps<T>) {
  return (
    <div>
      <strong>Generic Component with objects</strong>
      {list.map(listItem =>
        <span key={listItem.id} onClick={() => handler(listItem)} style={{ margin: '0 3px' }}>{listItem.name}</span>)}
    </div>
  )
}

export default GenericComponentAdvanced
```

## Never type

```jsx
type NumberType = {
  number: number,
};

type PositiveNumberType = NumberType & {
  isPositive: boolean,
  isNegative?: never,
  isZero?: never,
};

type NegativeNumberType = NumberType & {
  isPositive?: never,
  isNegative: boolean,
  isZero?: never,
};

type ZeroNumberType = NumberType & {
  isPositive?: never,
  isNegative?: never,
  isZero: Boolean,
};

type NeverTypeProps = PositiveNumberType | NegativeNumberType | ZeroNumberType;

function NeverType({ number, isPositive, isNegative, isZero }: NeverTypeProps) {
  return (
    <div>
      value: {number} is {isPositive && 'positive'} {isNegative && 'negative'}{' '}
      {isZero && 'zero'}
    </div>
  );
}

export default NeverType;
```
