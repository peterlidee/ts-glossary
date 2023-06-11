# Generics

## Generic classes

```ts
class KeyValuePair<K, V> {
  constructor(public key: K, public value: V) {}
}
let pair = new KeyValuePair<number, string>(1, 'a');
// The TypeScript compiler can sometimes infer
// generic type arguments so we don't need to specify them.
let other = new KeyValuePair(1, 'a');
```

## Generic functions

```ts
function wrapInArray<T>(value: T) {
  return [value];
}
let numbers = wrapInArray(1);
```

## Generic interfaces

```ts
interface Result<T> {
  data: T | null;
}
```

## Implementing generic parameter contraints

Example:

```ts
function echo<T extends number | string>(value: T) {}
// Restrict using a shape object
function echo<T extends { name: string }>(value: T) {}
// Restrict using an interface or a class
function echo<T extends Person>(value: T) {}
```

Another example:

```ts
interface Form<T> {
  values: T;
}
function getFieldValue<T, K extends keyof T>(form: Form<T>, fieldName: K) {
  return form.values[fieldName];
}

const contactForm = {
  values: {
    name: 'Bob',
    email: 'bob@someemail.com',
  },
};
console.log(getFieldValue(contactForm, 'name'));
// error on phone (not in values)
console.log(getFieldValue(contactForm, 'phone'));
```

## Extending generic classes

```ts
// Passing on generic type parameters
class CompressibleStore<T> extends Store<T> {}
// Constraining generic type parameters
class SearchableStore<T extends { name: string }> extends Store<T> {}
// Fixing generic type parameters
// only accept <Product>
class ProductStore extends Store<Product> {}
```

## Type mapping

```ts
type ReadOnly<T> = {
  readonly [K in keyof T]: T[K];
};
type Optional<T> = {
  [K in keyof T]?: T[K];
};
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};
```
