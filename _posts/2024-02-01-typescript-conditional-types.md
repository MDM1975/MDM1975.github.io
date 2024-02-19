---
layout: post
img: ../assets/imgs/conditional-types.jpg
summary: Conditional types in TypeScript allow developers to create types that adapt and change based on certain conditions. This article explores the basics and advanced use cases of conditional types in TypeScript, showcasing their practical applications and considerations.
---

Conditional types in TypeScript are powerful features that allow developers to create types that adapt and change based on certain conditions. Think of them like the Swiss Army knife of the TypeScript type system, enabling you to build more flexible, dynamic, and robust applications. Using conditional types, developers can define type behavior similar to if-else statements or ternary operators in regular programming logic. This article will explore the basics and advanced use cases of conditional types in TypeScript, showcasing their practical applications and considerations.

#### Understanding Conditional Types

At their core, conditional types use the `extends` keyword to perform type checks and select types conditionally. This mechanism allows TypeScript to evaluate and infer types based on specific conditions, providing a way to define type transformations, filtering, and other type operations. Conditional types are expressed using the following syntax:

```ts
type ConditionalType<T> = T extends U ? X : Y;
```

This syntax can be read as follows:

> If type `T` extends type `U`, then the result is type `X`; otherwise, the result is type `Y`.

For example, the following conditional type checks if the input type is a string and returns a string type if the condition is met; otherwise, it returns `never` (a type that represents a value that never occurs).

```ts
type Text<T> = T extends string ? string : never;
```

#### Basic Conditional Types

Conditional types can be used to perform basic type checks and transformations, such as filtering out specific types or applying different types based on conditions. The following examples illustrate some of the fundamental use cases for conditional types.

##### Truthy Type Checker

Imagine you're building a function that behaves differently based on whether the input is considered `truthy` or `falsy`. Using the `Truthy` conditional type, TypeScript can help ensure your function's behavior is accurately typed. This type effectively filters out falsy values _(0, "", false, null, undefined)_ at compile time, ensuring that the logic within your function deals only with values that are considered truthy (i.e., any value that is not falsy).

```ts
type Truthy<T> = T extends 0 | "" | false | null | undefined ? false : true;

type Foo = Truthy<0>; // false
type Bar = Truthy<1>; // true

type Fizz = Truthy<"">; // false
type Buzz = Truthy<"string">; // true
```

<br>

##### Object Type Checker

Consider a scenario where your code must differentiate between object and primitive types to handle them appropriately. The ObjectType conditional type does that, allowing TypeScript to enforce that only objects are processed in certain parts of your application.

```ts
type ObjectType<T> = T extends Record<string, any> ? T : never;

type Foo = ObjectType<"">; // never
type Bar = ObjectType<{ key: string }>; // { key: string; }
```

This becomes incredibly useful when dealing with dynamic data structures or APIs where the data type isn't known upfront.

<br>

##### Array Element Extractor

Working with arrays is common in programming, but sometimes, you need to operate on the type of the array's elements rather than the array itself. The ArrayType conditional type enables TypeScript to infer and extract the element type from an array, making it easier to work with functions that should operate on individual elements.

```ts
type ArrayType<T> = T extends Array<infer R> ? R : never;

type Bar = ArrayType<string>; // never
type Foo = ArrayType<string[]>; // string
```

This type could be used in generic functions that need to handle elements of an array without knowing the array's type upfront.

<br>

#### Advanced Conditional Types

The true power of conditional types shines when dealing with complex data structures or enforcing strict type constraints. The following examples demonstrate how conditional types can create advanced type definitions that adapt to various scenarios.

##### Recursive Strict Defined Type

Ensuring that objects conform to a strict structure with no optional or undefined properties can be crucial for reliability in large-scale applications. The StrictDefined type recursively applies conditions to enforce this strictness at all levels of an object.

```ts
type StrictDefined<T> = T extends null | undefined
  ? never
  : T extends Record<string, unknown>
  ? { [K in keyof T]-?: StrictDefined<T[K]> }
  : T;

type Foo = {
  a?: string;
  b?: string;
  c?: { d?: string };
};

type Bar = StrictDefined<Foo>; // { a: string; b: string; c: { d: string; } }
```

This type is beneficial in scenarios where the integrity of data structures is paramount, such as in configuration objects or API responses where missing values could lead to runtime errors.

<br>

#### Practical Applications and Considerations

- **Type Safety**: By enabling developers to specify conditions for types, conditional types play a pivotal role in enhancing type safety. They allow for creating precise type conditions and transformations based on the relationships between types.
- **Code Reusability:** Conditional types promote code reusability by allowing the creation of generic and adaptable type definitions. This reduces code duplication and fosters a more maintainable codebase.
- **Advanced Type Manipulation:** The ability to perform advanced type manipulations, such as filtering, extracting nested types, or applying transformations recursively, opens up new possibilities for handling complex type scenarios in TypeScript projects.

#### Conclusion

Conditional types are indispensable in the TypeScript toolkit, providing the versatility to tackle various type-related challenges in software development. From simple conditional checks to complex recursive type transformations, conditional types offer the mechanisms for writing type-safe, flexible, and maintainable TypeScript code. As TypeScript continues to evolve, understanding and leveraging conditional types will be invaluable for developers seeking to utilize the language's type system fully.
