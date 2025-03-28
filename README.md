# TypeScript AST Static Resolver

A TypeScript utility that converts AST nodes to their literal values. This utility is designed to resolve static values from TypeScript AST nodes and does not reflect runtime changes.

## Features

- Resolves various TypeScript AST nodes to their literal values:
  - Numeric literals (including decimals, scientific notation, hex, octal, binary)
  - BigInt literals
  - String literals (including template literals)
  - Regular expression literals
  - Boolean literals (true/false)
  - Undefined values
  - Null values
  - Array literals
  - Object literals
  - Template expressions
  - Variable references
  - Imported constants

## Installation

```bash
npm install ts-ast-static-resolver
```

## Usage

```typescript
import { resolveToLiteral } from 'ts-ast-static-resolver';
import ts from 'typescript';

// Create a TypeScript program
const program = ts.createProgram(['your-file.ts'], {
  target: ts.ScriptTarget.Latest,
});

// Get the AST node you want to resolve
const sourceFile = program.getSourceFile('your-file.ts');
const node = // ... get your AST node

// Resolve the node to its literal value
const result = resolveToLiteral(node, program);

// Check the result type and value
if (result.valueType === 'StringLiteral') {
  console.log(result.value); // string value
} else if (result.valueType === 'NumericLiteral') {
  console.log(result.value); // number value
}
// ... handle other types
```

## API

### Types

```typescript
type ResolverResult =
  | { valueType: 'NumericLiteral'; value: number }
  | { valueType: 'BigIntLiteral'; value: bigint }
  | { valueType: 'StringLiteral'; value: string }
  | { valueType: 'RegularExpressionLiteral'; value: RegExp }
  | { valueType: 'TrueKeyword'; value: true }
  | { valueType: 'FalseKeyword'; value: false }
  | { valueType: 'UndefinedKeyword'; value: undefined }
  | { valueType: 'VoidExpression'; value: undefined }
  | { valueType: 'NullKeyword'; value: null }
  | { valueType: 'ArrayLiteralExpression'; value: unknown[] }
  | { valueType: 'ObjectLiteralExpression'; value: Record<string, unknown> }
  | { valueType: undefined; value: undefined };
```

### Functions

#### `resolveToLiteral(expr: ts.Node, program: ts.Program): ResolverResult`

Converts a TypeScript AST node to its literal value. If the conversion is not possible, returns `{ valueType: undefined, value: undefined }`.

## Examples

### Resolving String Literals

```typescript
const result = resolveToLiteral(stringNode, program);
if (result.valueType === 'StringLiteral') {
  console.log(result.value); // "hello world"
}
```

### Resolving Template Literals

```typescript
const result = resolveToLiteral(templateNode, program);
if (result.valueType === 'StringLiteral') {
  console.log(result.value); // "Value: 123"
}
```

### Resolving Arrays

```typescript
const result = resolveToLiteral(arrayNode, program);
if (result.valueType === 'ArrayLiteralExpression') {
  console.log(result.value); // ["hello", 123]
}
```

### Resolving Objects

```typescript
const result = resolveToLiteral(objectNode, program);
if (result.valueType === 'ObjectLiteralExpression') {
  console.log(result.value); // { hello: 123 }
}
```

## Limitations

- This utility only resolves static values and does not reflect runtime changes
- Complex expressions or dynamic values cannot be resolved
- Some TypeScript-specific features may not be fully supported
- Certain edge cases and special values may not be resolved as expected

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT
