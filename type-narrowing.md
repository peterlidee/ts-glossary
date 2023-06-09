# Type narrowing

8 ways to do type narrowing in Typescript:

- [Type narrowing](#type-narrowing)
  - [1. Type assertion](#1-type-assertion)
  - [2. Non-null assertion operator](#2-non-null-assertion-operator)
  - [3. `typeof` type guard](#3-typeof-type-guard)
  - [4. `instanceof` type guard](#4-instanceof-type-guard)
  - [5. `in` type guard](#5-in-type-guard)
  - [6. User defined type guard with type predicate](#6-user-defined-type-guard-with-type-predicate)
  - [7. User defined type guard with assertion signature](#7-user-defined-type-guard-with-assertion-signature)
  - [8. Discriminated union pattern](#8-discriminated-union-pattern)

## 1. Type assertion

```ts
const button1 = <HTMLButtonElement>document.querySelector('.go');
const button2 = document.querySelector('.go') as HTMLButtonElement;
```

## 2. Non-null assertion operator

```ts
return text!.concat(text!);
```

## 3. `typeof` type guard

```ts
function double(item: string | number) {
  if (typeof item === 'string') {
    return item.concat(item);
  } else {
    return item + item;
  }
}
```

## 4. `instanceof` type guard

```ts
objectVariable instanceof ClassName;
```

## 5. `in` type guard

```ts
propertyName in objectVariable;
```

```ts
function sayHello(contact: Contact) {
  if ('firstName' in contact) {
    console.log('Hello ' + contact.firstName);
  }
}
```

## 6. User defined type guard with type predicate

```ts
function isTypeName(paramName: WideTypeName): paramName is NarrowTypeName {
  // some check
  return boolean_result_of_check;
}
```

```ts
interface Person {
  firstName: string;
  surname: string;
}
interface Organisation {
  name: string;
}
type Contact = Person | Organisation;

function isPerson(item: Contact): item is Person {
  return (item as Person).firstName !== undefined;
}

function sayHello(contact: Contact) {
  if (isPerson(contact)) {
    console.log('Hello' + contact.firstName);
  }
}
```

## 7. User defined type guard with assertion signature

```ts
function assertTypeName(
  paramName: WideTypeName
): asserts paramName is NarrowTypeName {
  if (some_check) {
    throw new Error('Assert failed');
  }
}
```

```ts
interface Person {
  firstName: string;
  surname: string;
}
interface Organisation {
  name: string;
}
type Contact = Person | Organisation;

function assertPerson(contact: Contact): asserts contact is Person {
  if ((contact as Person).firstName === undefined) {
    throw new Error('Not a person');
  }
}

function sayHello(contact: Contact) {
  assertPerson(contact);
  console.log('Hello' + contact.firstName);
}
```

## 8. Discriminated union pattern

```ts
// common singleton type property
type Type1 = {
  ...
  commonName: "value1"
}
type Type2 = {
  ...
  commonName: "value1"
}
...
type TypeN = {
  ...
  commonName: "valueN"
}

// union
type UnionType = Type1 | Type2 | ... | TypeN

// type guards
function (param: UnionType) {
  switch (param.commonName) {
    case "value1":
      // type narrowed to Type1
      break;
    case "value2":
      // type narrowed to Type2
      break;
    ...
    case "valueN":
      // type narrowed to TypeN
      break;
  }
}
```

Example:

```ts
// common singleton type property (contactType)
interface Person {
  firstName: string;
  surname: string;
  contactType: 'person';
}
interface Organisation {
  name: string;
  contactType: 'organisation';
}
// union
type Contact = Person | Organisation;

// type guards
function sayHello(contact: Contact) {
  switch (contact.contactType) {
    case 'person':
      console.log('Hello' + contact.firstName);
      break;
    case 'organisation':
      console.log('Hello' + contact.name);
      break;
    default:
    // do something
  }
}
```
