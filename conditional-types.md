# Conditional types

```ts
T1 extends T2 ? A : B
```

Example: remove null from union.

```ts
type NullableString = string | null;
type RemoveNull<T> = T extends null ? never : T;

let firstName: RemoveNull<NullableString>;
// type error
firstName = null;
firstName = 'Bob';
```

## `infer`

```ts
function addPerson(personName: string) {
  return {
    type: 'AddPerson',
    payload: personName,
  };
}

function removePerson(id: number) {
  return {
    type: 'RemovePerson',
    payload: id,
  };
}

type AddPersonType = typeof addPerson;
type RemovePersonType = typeof removePerson;
type FunctionReturnType<T extends (...args: any) => any> = T extends (
  ...args: any
) => infer R
  ? R
  : T;

type Union =
  | FunctionReturnType<AddPersonType>
  | FunctionReturnType<RemovePersonType>;

const person = { name: 'Fred' };
// fails because not a function
type PersonType = FunctionReturnType<typeof person>;
```

## `exclude` utility type equivalent

```ts
const person = {
  name: 'Fred',
  age: 30,
  email: 'fred@somewhere.com',
};
// T is union, T are keys
type RemoveFromUnion<T, K> = T extends K ? never : T;
type ContactKeys = RemoveFromUnion<keyof typeof person, 'age'>;
```

## `pick` utility type equivalent

```ts
const person = {
  name: 'Fred',
  age: 30,
  email: 'fred@somewhere.com',
};
// T is object type, K are keys
type ObjectWithKeys<T, K extends keyof T> = {
  [P in K]: T[P];
};
type Contact = ObjectWithKeys<typeof person, 'name' | 'email'>;
```

## `omit` utility type equivalent

This is a combo of pick and exclude.

```ts
const person = {
  name: 'Fred',
  age: 30,
  email: 'fred@somewhere.com',
};

// takes object type and keys to remove
type ObjectWithoutKeys<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
// { age: number }
type Profile = ObjectWithoutKeys<typeof person, 'name' | 'email'>;
```
