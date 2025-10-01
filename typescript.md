# TypeScript Key Concepts

## Basic Types

### Primitive Types
- **string, number, boolean**: Basic JavaScript types
- **bigint**: For large integers beyond Number.MAX_SAFE_INTEGER
- **symbol**: Unique identifiers
- **null, undefined**: Absence of value
- **void**: No return value (functions)
- **never**: Values that never occur (infinite loops, thrown errors)

### Literal Types
- **String literals**: `type Status = "loading" | "success" | "error"`
- **Numeric literals**: `type Port = 80 | 443 | 8080`
- **Boolean literals**: `type IsTrue = true`
- **Template literal types**: `type Route = \`/api/\${string}\``

## Advanced Types

### Union Types
```typescript
type StringOrNumber = string | number;
type Status = "pending" | "fulfilled" | "rejected";
```

### Intersection Types
```typescript
type User = { name: string } & { age: number };
// Results in: { name: string; age: number }
```

### Conditional Types
```typescript
type IsArray<T> = T extends any[] ? true : false;
type NonNullable<T> = T extends null | undefined ? never : T;
```

### Mapped Types
```typescript
type Readonly<T> = { readonly [P in keyof T]: T[P] };
type Partial<T> = { [P in keyof T]?: T[P] };
type Pick<T, K extends keyof T> = { [P in K]: T[P] };
```

### Index Types
```typescript
type Person = { name: string; age: number };
type PersonKeys = keyof Person; // "name" | "age"
type PersonName = Person["name"]; // string
```

## Generics

### Basic Generics
```typescript
function identity<T>(arg: T): T { return arg; }
interface Array<T> { push(item: T): number; }
```

### Generic Constraints
```typescript
interface Lengthwise { length: number; }
function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
```

### Conditional Type Inference
```typescript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
type Parameters<T> = T extends (...args: infer P) => any ? P : never;
```

## Utility Types

### Built-in Utility Types
- **Partial\<T>**: Makes all properties optional
- **Required\<T>**: Makes all properties required
- **Readonly\<T>**: Makes all properties readonly
- **Pick\<T, K>**: Selects subset of properties
- **Omit\<T, K>**: Excludes subset of properties
- **Record\<K, T>**: Creates type with keys K and values T
- **Exclude\<T, U>**: Excludes types from union
- **Extract\<T, U>**: Extracts types from union
- **NonNullable\<T>**: Excludes null and undefined

### Advanced Utility Types
- **ReturnType\<T>**: Gets function return type
- **Parameters\<T>**: Gets function parameter types
- **ConstructorParameters\<T>**: Gets constructor parameter types
- **InstanceType\<T>**: Gets instance type of constructor

## Classes and Interfaces

### Interface vs Type Alias
```typescript
// Interface - can be extended and merged
interface User {
  name: string;
}
interface User {
  age: number; // Declaration merging
}

// Type alias - more flexible but no merging
type UserType = {
  name: string;
} & {
  age: number;
};
```

### Class Features
```typescript
class User {
  public name: string;           // Public access
  private _id: number;           // Private access
  protected role: string;        // Protected access
  readonly createdAt: Date;      // Readonly property
  static instances = 0;          // Static property

  constructor(name: string) {
    this.name = name;
    this.createdAt = new Date();
    User.instances++;
  }

  abstract getValue(): string;   // Abstract method
}
```

### Access Modifiers
- **public**: Accessible everywhere (default)
- **private**: Only within the class
- **protected**: Within class and subclasses
- **readonly**: Cannot be modified after initialization

## Advanced Patterns

### Discriminated Unions
```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; size: number }
  | { kind: "rectangle"; width: number; height: number };

function area(shape: Shape) {
  switch (shape.kind) {
    case "circle": return Math.PI * shape.radius ** 2;
    case "square": return shape.size ** 2;
    case "rectangle": return shape.width * shape.height;
  }
}
```

### Type Guards
```typescript
// User-defined type guards
function isString(value: unknown): value is string {
  return typeof value === "string";
}

// Built-in type guards
if (typeof x === "string") { /* x is string */ }
if (x instanceof Date) { /* x is Date */ }
if ("length" in x) { /* x has length property */ }
```

### Module Augmentation
```typescript
declare module "express" {
  interface Request {
    user?: User;
  }
}
```

## Declaration Files

### Ambient Declarations
```typescript
declare const API_URL: string;
declare function gtag(command: string, ...args: any[]): void;
declare module "*.svg" {
  const content: string;
  export default content;
}
```

### Triple-Slash Directives
```typescript
/// <reference path="./types.d.ts" />
/// <reference types="node" />
```

## Compiler Configuration

### tsconfig.json Key Options
```json
{
  "compilerOptions": {
    "strict": true,                    // Enable all strict checks
    "noImplicitAny": true,            // Error on implicit any
    "strictNullChecks": true,         // Null safety
    "strictFunctionTypes": true,      // Function type checking
    "noImplicitReturns": true,        // All paths must return
    "noFallthroughCasesInSwitch": true, // Switch exhaustiveness
    "exactOptionalPropertyTypes": true, // Exact optional types
    "target": "ES2020",               // Output target
    "module": "ESNext",               // Module system
    "moduleResolution": "node",       // Module resolution
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "skipLibCheck": true,            // Skip type checking of declaration files
    "forceConsistentCasingInFileNames": true
  }
}
```

### Strict Mode Features
- **noImplicitAny**: No implicit any types
- **strictNullChecks**: Null and undefined handling
- **strictFunctionTypes**: Contravariant function parameters
- **strictBindCallApply**: Type checking for bind/call/apply
- **strictPropertyInitialization**: Class property initialization
- **noImplicitThis**: No implicit this types

## Performance & Best Practices

### Type Performance
- **Avoid deep nesting**: Can slow compilation
- **Use type aliases**: For complex types
- **Prefer interfaces**: For object shapes
- **Use const assertions**: `as const` for immutable data

### Debugging Types
```typescript
// Type debugging utilities
type Debug<T> = { [K in keyof T]: T[K] };
type Expand<T> = T extends infer O ? { [K in keyof O]: O[K] } : never;

// Check if types are equal
type Equal<X, Y> = (<T>() => T extends X ? 1 : 2) extends
  (<T>() => T extends Y ? 1 : 2) ? true : false;
```

### Error Handling Patterns
```typescript
// Result type pattern
type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };

// Option type pattern
type Option<T> = T | null | undefined;
```

## Ecosystem & Tools

### Popular Type Libraries
- **@types/node**: Node.js types
- **@types/react**: React types
- **io-ts**: Runtime type validation
- **zod**: Schema validation with TypeScript
- **class-validator**: Decorator-based validation

### Development Tools
- **TypeScript ESLint**: Linting for TypeScript
- **Prettier**: Code formatting
- **ts-node**: Run TypeScript directly
- **tsc-watch**: Watch mode compilation
- **typescript-json-schema**: Generate JSON schemas

### Migration Strategies
- **Gradual adoption**: Add TypeScript incrementally
- **allowJs**: Allow JavaScript files during migration
- **checkJs**: Type check JavaScript files
- **Any escape hatch**: Use sparingly during migration