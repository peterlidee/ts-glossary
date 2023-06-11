# Standard types

- `Date`
- `any`
- `void`
- [Arrays](#arrays)
- [Tuples](#tuples)
- `never`
- `unknown`

## Arrays

```ts
const items: Array<Number> = [];
const items: number[] = [];
```

### Strongly typed rest parameters:

```ts
function logScores(firstName: string, ...scores: number[]) {
  console.log(firstName, scores);
}
logScores('Ben', 50, 75, 85);
```

## Tuples

```ts
const tomScore: [string, number] = ['Tom', 70];
```

### Tuples with labels

```ts
const tomScore: [name: string, score: number] = ['Tom', 70];
```

### Open-ended tuples

```ts
const bobScore: [string, ...number[]] = ['Tom', 70, 70, 70];
const tomScore: [name: string, ...scores: number[]] = ['Tom', 70, 70, 70];
```

## `never` type

### `never` type example

If we add a new status value but forget the catch it in the switch statement, below code will throw a typescript error. So, we made doSomeAction typesafe.

```ts
function reject(): never {
  // This function never returns because
  // it always throws an error
  throw new Error('Error!');
}
```

```ts
// 'xxx' will trigger never
type Status = 'Loading' | 'Loaded' | 'xxx';

function neverReached(never: never) {}

function doSomeAction(status: Status) {
  switch (status) {
    case 'Loading':
      // some code
      break;
    case 'Loaded':
      // some code
      break;
    default:
      neverReached(status);
  }
}
```
