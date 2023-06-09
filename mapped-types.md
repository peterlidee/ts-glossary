# Mapped types

## `keyof` operator

The keyof type annotation can be used to extract the keys from an object.

```ts
let keys: keyof ExistingType;
```

## `in` operator

The in operator maps over each item in the union type to create a new type.

```ts
type MappedTypeName = {
  [K in keyof ExistingType1]: ExistingType2;
};
```

Example:

```ts
interface Form<T> {
  values: T;
  errors: { [K in keyof T]?: string };
}

const contactForm: Form<{ name: string; email: string }> = {
  values: {
    name: 'Bob',
    email: 'bob@someemail.com',
  },
  errors: {
    email: 'Invalid email address',
  },
};
```

## Mapped type modifier

Example:

```ts
type Contact = {
  name: string;
  email?: string;
  age: 30;
};
type RequiredProperties<T> = {
  [K in keyof T]-?: T[K];
};

const bob: RequiredProperties<Contact> = {
  name: 'Bob',
  email: 'email',
  age: 30,
};

console.log(bob);
```

## `typeof` operator

```ts
let newObject: typeof existingObject;
```
