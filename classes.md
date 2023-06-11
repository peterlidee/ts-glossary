# Classes

### Classes and constructors

```ts
class Account {
  id: number;
  constructor(id: number) {
    this.id = id;
  }
}
let account = new Account(1);
```

### Accessing properties and methods

```ts
account.id = 1;
account.deposit(10);
```

### Read-only and optional properties

```ts
class Account {
  readonly id: number;
  nickname?: string;
}
```

### Access modifiers

```ts
class Account {
  private _balance: number;

  // Protected members are inherited.
  // Private members are not.
  protected _taxRate: number;
}
```

### Parameter properties

```ts
class Account {
  // With parameter properties we can
  // create and initialize properties in one place.
  constructor(public id: number, private _balance: number) {}
}
```

### Getters and setters

```ts
class Account {
  private _balance = 0;

  get balance(): number {
    return this._balance;
  }

  set balance(value: number) {
    if (value < 0) throw new Error();
    this._balance = value;
  }
}
```

### Index signatures

```ts
class SeatAssignment {
  // With index signature properties we can add
  // properties to an object dynamically
  // without losing type safety.
  [seatNumber: string]: string;
}
let seats = new SeatAssignment();
seats.A1 = 'Mosh';
seats.A2 = 'John';
```

### Static members

```ts
class Ride {
  static activeRides = 0;
}
Ride.activeRides++;
```

### Inheritance

```ts
class Student extends Person {}
class Student extends Person {
  override speak() {
    console.log('Student speaking');
  }
}
```

### Method overriding

```ts
abstract class Shape {
  // Abstract methods don't have a body
  abstract render();
}
class Circle extends Shape {
  override render() {
    console.log('Rendering a circle');
  }
}
```

### Interfaces

```ts
interface Calendar {
  name: string;
  addEvent(): void;
}
class GoogleCalendar implements Calendar {}
```
