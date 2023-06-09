# Generics

## Implementing generic parameter contraints

Example:

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
