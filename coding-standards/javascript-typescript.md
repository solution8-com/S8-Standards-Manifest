# JavaScript/TypeScript Coding Standards

## Overview

JavaScript and TypeScript are core languages at Solution8. These standards ensure we write clean, maintainable, and type-safe code.

## TypeScript First

- **Use TypeScript** for all new projects when possible
- TypeScript provides type safety and better tooling support
- Maintain strict TypeScript configuration

## Naming Conventions

### Variables and Functions
```typescript
// Use camelCase for variables and functions
const userName = 'John Doe';
const calculateTotal = (items: Item[]) => { ... };

// Use PascalCase for classes and interfaces
class UserService { ... }
interface UserData { ... }

// Use UPPER_CASE for constants
const MAX_RETRY_COUNT = 3;
const API_ENDPOINT = 'https://api.example.com';
```

### Files and Directories
- Use kebab-case for file names: `user-service.ts`, `api-client.ts`
- Use PascalCase for component files in React: `UserProfile.tsx`

## Code Style

### Formatting
- Use 2 spaces for indentation
- Use semicolons
- Use single quotes for strings (except when avoiding escaping)
- Maximum line length: 100 characters
- Use trailing commas in multi-line objects and arrays

### ESLint & Prettier
- All projects must use ESLint and Prettier
- Follow the standard configuration provided in the template repository

## TypeScript Best Practices

### Type Annotations
```typescript
// Always specify return types for functions
function getUser(id: string): Promise<User> {
  return fetchUser(id);
}

// Use interfaces for object shapes
interface User {
  id: string;
  name: string;
  email: string;
}

// Avoid 'any' type - use 'unknown' when type is truly unknown
const parseData = (data: unknown): ParsedData => { ... };
```

### Type Guards
```typescript
function isUser(obj: unknown): obj is User {
  return (
    typeof obj === 'object' &&
    obj !== null &&
    'id' in obj &&
    'name' in obj
  );
}
```

## Modern JavaScript Features

### Use Modern Syntax
```typescript
// Use const/let instead of var
const config = { ... };
let counter = 0;

// Use arrow functions for callbacks
items.map(item => item.value);

// Use destructuring
const { name, email } = user;
const [first, ...rest] = items;

// Use template literals
const message = `Hello, ${userName}!`;

// Use optional chaining and nullish coalescing
const value = user?.profile?.address?.city ?? 'Unknown';
```

### Async/Await
```typescript
// Prefer async/await over raw promises
async function fetchUserData(id: string): Promise<UserData> {
  try {
    const response = await fetch(`/api/users/${id}`);
    return await response.json();
  } catch (error) {
    logger.error('Failed to fetch user', { id, error });
    throw error;
  }
}
```

## React/Frontend Standards

### Component Structure
```typescript
// Use functional components with hooks
import React, { useState, useEffect } from 'react';

interface UserProfileProps {
  userId: string;
}

export const UserProfile: React.FC<UserProfileProps> = ({ userId }) => {
  const [user, setUser] = useState<User | null>(null);
  
  useEffect(() => {
    loadUser(userId);
  }, [userId]);
  
  return (
    <div className="user-profile">
      {/* Component JSX */}
    </div>
  );
};
```

### Hooks Best Practices
- Follow the Rules of Hooks
- Extract custom hooks for reusable logic
- Keep hooks at the top level of components

## Node.js Standards

### Module Structure
```typescript
// Use ES modules
import { Router } from 'express';
import { userService } from './services';

// Export named exports when possible
export const userRouter = Router();

// Default export for single responsibility modules
export default class UserController { ... }
```

### Error Handling
```typescript
// Use custom error classes
class ValidationError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

// Handle errors in async routes
app.post('/users', async (req, res, next) => {
  try {
    const user = await userService.create(req.body);
    res.json(user);
  } catch (error) {
    next(error);
  }
});
```

## Testing

### Unit Tests
- Use Jest as the testing framework
- Aim for 80%+ code coverage
- Write clear test descriptions

```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create a user with valid data', async () => {
      const userData = { name: 'John', email: 'john@example.com' };
      const user = await userService.createUser(userData);
      
      expect(user).toHaveProperty('id');
      expect(user.name).toBe(userData.name);
    });
    
    it('should throw ValidationError for invalid email', async () => {
      const userData = { name: 'John', email: 'invalid' };
      
      await expect(userService.createUser(userData))
        .rejects
        .toThrow(ValidationError);
    });
  });
});
```

## Documentation

- Use JSDoc comments for public APIs
- Document complex logic with inline comments
- Keep comments up-to-date with code changes

```typescript
/**
 * Calculates the total price including tax
 * @param items - Array of items in the cart
 * @param taxRate - Tax rate as a decimal (e.g., 0.2 for 20%)
 * @returns Total price with tax applied
 */
function calculateTotal(items: CartItem[], taxRate: number): number {
  const subtotal = items.reduce((sum, item) => sum + item.price, 0);
  return subtotal * (1 + taxRate);
}
```

## Tools & Configuration

### Required Tools
- TypeScript (latest stable version, 5.0+ minimum)
- ESLint with TypeScript support
- Prettier for code formatting
- Jest for testing

### tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "outDir": "./dist"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```
