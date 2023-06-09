# Creating types

- [Creating types](#creating-types)
  - [Enum](#enum)
  - [Union types](#union-types)
  - [Intersection types](#intersection-types)
    - [Intersection of common members](#intersection-of-common-members)
    - [Intersection vs Union](#intersection-vs-union)

## Enum

```ts
enum TypeName {
    value1,
    value2,
    ...
}
enum Day {
  Monday = 'MON',
  Tuesday = 'TUE',
  ...
}
```

## Union types

```ts
type A_or_B_or_C = A | B | C;
```

## Intersection types

```ts
type A_and_B_and_C = A & B & C;
```

### Intersection of common members

The type of a common member of an intersection type is mathematically intersected.

```ts
type BaseElement = {
  name: string;
  kind: 'text' | 'number' | 'email';
};
type TextInput = {
  kind: 'text';
};
type Field = BaseElement & TextInput;
const field: Field = {
  name: 'Test',
  // kind can only be 'text' as that is intersection
  kind: 'text',
};
```

### Intersection vs Union

```ts
type Group1 = 'A' | 'B' | 'C';
type Group2 = 'B' | 'C' | 'D';
// union = A B C D
type Union = Group1 | Group2;
// intersection = B C
type Intersection = Group1 & Group2;
```
