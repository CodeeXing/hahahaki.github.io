---
layout: default
title: "Getting Started with TypeScript: A Practical Guide"
date: 2024-01-15
categories: [TypeScript, JavaScript, Web Development]
author: Hahahaki
---

# Getting Started with TypeScript: A Practical Guide

TypeScript has become an essential tool in modern web development, offering type safety and improved developer experience to JavaScript projects. In this guide, we'll explore the fundamentals of TypeScript and how to integrate it into your workflow.

## What is TypeScript?

TypeScript is a strongly typed superset of JavaScript that compiles to plain JavaScript. It adds optional static typing, classes, and interfaces to JavaScript, making it easier to catch errors during development rather than at runtime.

## Why Use TypeScript?

### 1. Type Safety
```typescript
// JavaScript - potential runtime error
function add(a, b) {
  return a + b;
}
add(5, "10"); // "510" - not what we wanted!

// TypeScript - caught at compile time
function add(a: number, b: number): number {
  return a + b;
}
add(5, "10"); // Error: Argument of type 'string' is not assignable to parameter of type 'number'
```

### 2. Better IDE Support
TypeScript provides excellent autocomplete, inline documentation, and refactoring capabilities in modern IDEs.

### 3. Enhanced Code Quality
Type annotations serve as inline documentation and make code more readable and maintainable.

## Getting Started

### Installation
```bash
npm install -g typescript
```

### Your First TypeScript File
Create a file named `hello.ts`:
```typescript
function greet(name: string): string {
  return `Hello, ${name}!`;
}

console.log(greet("TypeScript"));
```

Compile it:
```bash
tsc hello.ts
```

This generates a `hello.js` file that you can run with Node.js.

## Basic Types

TypeScript provides several basic types:

```typescript
// Primitives
let isDone: boolean = false;
let age: number = 25;
let name: string = "John";

// Arrays
let numbers: number[] = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];

// Tuple
let tuple: [string, number] = ["hello", 42];

// Enum
enum Color {
  Red,
  Green,
  Blue
}
let c: Color = Color.Green;

// Any (use sparingly!)
let notSure: any = 4;
notSure = "maybe a string";
```

## Interfaces

Interfaces define the structure of objects:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // Optional property
}

function createUser(user: User): void {
  console.log(`Creating user: ${user.name}`);
}

createUser({
  id: 1,
  name: "Alice",
  email: "alice@example.com"
});
```

## Classes

TypeScript supports object-oriented programming with classes:

```typescript
class Animal {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  public move(distance: number): void {
    console.log(`${this.name} moved ${distance} meters.`);
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Woof! Woof!");
  }
}

const dog = new Dog("Buddy");
dog.bark();
dog.move(10);
```

## Generics

Generics allow you to write reusable, type-safe code:

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output1 = identity<string>("myString");
let output2 = identity<number>(100);

// Generic interface
interface GenericResponse<T> {
  data: T;
  status: number;
  message: string;
}

const userResponse: GenericResponse<User> = {
  data: { id: 1, name: "Alice", email: "alice@example.com" },
  status: 200,
  message: "Success"
};
```

## Best Practices

1. **Enable Strict Mode**: Always use strict mode in your `tsconfig.json`
```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

2. **Avoid `any`**: Use specific types or `unknown` instead of `any` when possible

3. **Use Interfaces for Objects**: Define clear contracts for your data structures

4. **Leverage Type Inference**: Let TypeScript infer types when obvious
```typescript
// Type inference works here
let message = "Hello"; // TypeScript knows this is a string
```

5. **Use Union Types**: When a value can be one of several types
```typescript
type Status = "pending" | "approved" | "rejected";
```

## Conclusion

TypeScript significantly improves the JavaScript development experience by catching errors early, providing better tooling support, and making code more maintainable. Start small by adding TypeScript to a new project or gradually migrating an existing JavaScript codebase.

## Resources

- [TypeScript Official Documentation](https://www.typescriptlang.org/docs/)
- [TypeScript Playground](https://www.typescriptlang.org/play)
- [Definitely Typed](https://github.com/DefinitelyTyped/DefinitelyTyped) - Type definitions for popular libraries

Happy coding with TypeScript!

---

*Have questions or feedback? Feel free to [contact me](../../contact.html)!*

[← Back to Blog](../../blog.html) | [Home](../../index.html)
