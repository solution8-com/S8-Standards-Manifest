# Testing Strategy

## Overview

A comprehensive testing strategy is essential for delivering reliable, maintainable software. This guide covers our testing philosophy, practices, and standards from unit tests to end-to-end testing.

## Table of Contents

- [Testing Philosophy](#testing-philosophy)
- [Test Pyramid](#test-pyramid)
- [Test-Driven Development (TDD)](#test-driven-development-tdd)
- [Unit Testing](#unit-testing)
- [Integration Testing](#integration-testing)
- [End-to-End Testing](#end-to-end-testing)
- [Testing Tools and Frameworks](#testing-tools-and-frameworks)
- [Code Coverage](#code-coverage)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)

## Testing Philosophy

### Core Principles

1. **Tests as Documentation**: Tests should clearly describe expected behavior
2. **Fast Feedback**: Tests should run quickly to enable rapid iteration
3. **Reliability**: Tests should be deterministic and repeatable
4. **Maintainability**: Tests should be easy to understand and modify
5. **Isolation**: Tests should not depend on external systems or other tests

### Testing Goals

```markdown
✓ Catch bugs early in development
✓ Enable confident refactoring
✓ Document system behavior
✓ Provide regression protection
✓ Support continuous deployment
✓ Reduce debugging time
```

### When to Write Tests

**Write tests BEFORE code (TDD)**:
- Complex business logic
- Critical system components
- Bug fixes (regression tests)

**Write tests WITH code**:
- Standard features
- API endpoints
- UI components

**Write tests AFTER code** (acceptable, but not ideal):
- Exploratory code
- Prototypes being productionized

## Test Pyramid

The test pyramid guides our test distribution strategy:

```
        /\
       /E2E\      ← 10% (Slow, Brittle, High Value)
      /------\
     /  INT   \   ← 20% (Medium Speed, Medium Value)
    /----------\
   /    UNIT    \ ← 70% (Fast, Stable, Foundation)
  /--------------\
```

### Distribution Guidelines

| Test Type | Percentage | Speed | Cost | Maintenance |
|-----------|-----------|-------|------|-------------|
| Unit | 70% | Very Fast | Low | Low |
| Integration | 20% | Medium | Medium | Medium |
| E2E | 10% | Slow | High | High |

### Why This Distribution?

**Unit Tests (70%)**:
- Fast execution (milliseconds)
- Pinpoint failures
- Easy to write and maintain
- Enable rapid development

**Integration Tests (20%)**:
- Validate component interactions
- Test real dependencies
- Catch integration issues
- Higher confidence than unit tests

**E2E Tests (10%)**:
- Validate critical user journeys
- Test entire system
- Highest confidence
- Slowest and most fragile

## Test-Driven Development (TDD)

### TDD Cycle (Red-Green-Refactor)

```
┌─────────────┐
│  RED        │ Write a failing test
│  Write Test │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  GREEN      │ Make the test pass
│  Write Code │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  REFACTOR   │ Improve the code
│  Clean Up   │
└──────┬──────┘
       │
       └──────► Repeat
```

### TDD Example

**Step 1: Write the test (RED)**

```javascript
// calculator.test.js
describe('Calculator', () => {
  it('should add two numbers correctly', () => {
    const calculator = new Calculator();
    const result = calculator.add(2, 3);
    expect(result).toBe(5);
  });
});

// Test fails - Calculator doesn't exist yet
```

**Step 2: Make it pass (GREEN)**

```javascript
// calculator.js
class Calculator {
  add(a, b) {
    return a + b;
  }
}

// Test passes
```

**Step 3: Refactor**

```javascript
// calculator.js
class Calculator {
  add(...numbers) {
    return numbers.reduce((sum, num) => sum + num, 0);
  }
}

// Tests still pass, but code is more flexible
```

### TDD Benefits

✅ **Better Design**: Forces you to think about API design first  
✅ **Complete Coverage**: All code is tested by definition  
✅ **Less Debugging**: Catch issues immediately  
✅ **Regression Safety**: Tests protect against future changes  
✅ **Documentation**: Tests serve as usage examples  

### TDD Anti-Patterns

❌ **Writing all tests first**: Write one test at a time  
❌ **Testing implementation details**: Test behavior, not internals  
❌ **Large test scenarios**: Keep tests focused and small  
❌ **Skipping refactor step**: Always clean up after green  

## Unit Testing

### What to Unit Test

```javascript
✓ Business logic
✓ Calculations and transformations
✓ Validation logic
✓ Utility functions
✓ State management
✓ Error handling
```

### Unit Test Structure (AAA Pattern)

```javascript
describe('UserService', () => {
  it('should create a new user with hashed password', () => {
    // Arrange - Set up test data and dependencies
    const userService = new UserService();
    const userData = {
      email: 'test@example.com',
      password: 'password123'
    };
    
    // Act - Execute the function being tested
    const user = userService.createUser(userData);
    
    // Assert - Verify the results
    expect(user.email).toBe('test@example.com');
    expect(user.password).not.toBe('password123');
    expect(user.password).toMatch(/^\$2[ayb]\$.{56}$/); // bcrypt hash
  });
});
```

### Mocking and Stubbing

**Mocking External Dependencies**:

```javascript
// userService.test.js
import { UserService } from './userService';
import { DatabaseService } from './databaseService';
import { EmailService } from './emailService';

jest.mock('./databaseService');
jest.mock('./emailService');

describe('UserService.registerUser', () => {
  let userService;
  let mockDb;
  let mockEmail;
  
  beforeEach(() => {
    mockDb = new DatabaseService();
    mockEmail = new EmailService();
    userService = new UserService(mockDb, mockEmail);
  });
  
  it('should save user and send welcome email', async () => {
    // Arrange
    const userData = { email: 'test@example.com', name: 'Test User' };
    mockDb.save.mockResolvedValue({ id: 1, ...userData });
    mockEmail.sendWelcome.mockResolvedValue(true);
    
    // Act
    const result = await userService.registerUser(userData);
    
    // Assert
    expect(mockDb.save).toHaveBeenCalledWith(
      expect.objectContaining({ email: 'test@example.com' })
    );
    expect(mockEmail.sendWelcome).toHaveBeenCalledWith('test@example.com');
    expect(result.id).toBe(1);
  });
});
```

### Testing Edge Cases

```javascript
describe('divide', () => {
  it('should divide two numbers correctly', () => {
    expect(divide(10, 2)).toBe(5);
  });
  
  it('should handle division by zero', () => {
    expect(() => divide(10, 0)).toThrow('Division by zero');
  });
  
  it('should handle negative numbers', () => {
    expect(divide(-10, 2)).toBe(-5);
    expect(divide(10, -2)).toBe(-5);
  });
  
  it('should handle decimal results', () => {
    expect(divide(10, 3)).toBeCloseTo(3.333, 3);
  });
  
  it('should handle zero dividend', () => {
    expect(divide(0, 5)).toBe(0);
  });
});
```

### Parameterized Tests

```javascript
describe('isPalindrome', () => {
  test.each([
    ['racecar', true],
    ['hello', false],
    ['A man a plan a canal Panama', true],
    ['', true],
    ['a', true],
    ['ab', false]
  ])('isPalindrome("%s") should return %s', (input, expected) => {
    expect(isPalindrome(input)).toBe(expected);
  });
});
```

### Async Testing

```javascript
describe('UserAPI', () => {
  it('should fetch user data', async () => {
    const user = await fetchUser(1);
    expect(user.id).toBe(1);
    expect(user.name).toBeDefined();
  });
  
  it('should handle API errors', async () => {
    await expect(fetchUser(9999)).rejects.toThrow('User not found');
  });
  
  it('should timeout after 5 seconds', async () => {
    jest.setTimeout(6000);
    await expect(slowAPICall()).rejects.toThrow('Timeout');
  });
});
```

## Integration Testing

### What to Integration Test

```markdown
✓ Database queries and transactions
✓ API endpoint responses
✓ Service-to-service communication
✓ External API integrations
✓ File system operations
✓ Message queue interactions
```

### Database Integration Tests

```javascript
// userRepository.integration.test.js
import { UserRepository } from './userRepository';
import { setupTestDatabase, teardownTestDatabase } from './test-helpers';

describe('UserRepository Integration Tests', () => {
  let db;
  let userRepo;
  
  beforeAll(async () => {
    db = await setupTestDatabase();
    userRepo = new UserRepository(db);
  });
  
  afterAll(async () => {
    await teardownTestDatabase(db);
  });
  
  beforeEach(async () => {
    await db.query('DELETE FROM users');
  });
  
  it('should create and retrieve a user', async () => {
    // Create
    const userData = {
      email: 'test@example.com',
      name: 'Test User'
    };
    const createdUser = await userRepo.create(userData);
    
    // Retrieve
    const foundUser = await userRepo.findById(createdUser.id);
    
    // Assert
    expect(foundUser.email).toBe(userData.email);
    expect(foundUser.name).toBe(userData.name);
  });
  
  it('should handle unique email constraint', async () => {
    const userData = { email: 'test@example.com', name: 'Test' };
    
    await userRepo.create(userData);
    
    await expect(userRepo.create(userData))
      .rejects
      .toThrow('Email already exists');
  });
});
```

### API Integration Tests

```javascript
// api.integration.test.js
import request from 'supertest';
import { app } from './app';
import { setupTestDatabase, teardownTestDatabase } from './test-helpers';

describe('User API Integration Tests', () => {
  let db;
  
  beforeAll(async () => {
    db = await setupTestDatabase();
  });
  
  afterAll(async () => {
    await teardownTestDatabase(db);
  });
  
  describe('POST /api/users', () => {
    it('should create a new user', async () => {
      const response = await request(app)
        .post('/api/users')
        .send({
          email: 'test@example.com',
          name: 'Test User',
          password: 'password123'
        })
        .expect(201);
      
      expect(response.body.user).toMatchObject({
        email: 'test@example.com',
        name: 'Test User'
      });
      expect(response.body.user.password).toBeUndefined();
    });
    
    it('should return 400 for invalid email', async () => {
      const response = await request(app)
        .post('/api/users')
        .send({
          email: 'invalid-email',
          name: 'Test User'
        })
        .expect(400);
      
      expect(response.body.error).toContain('Invalid email');
    });
  });
  
  describe('GET /api/users/:id', () => {
    it('should retrieve user by ID', async () => {
      // Setup: Create a user
      const createResponse = await request(app)
        .post('/api/users')
        .send({ email: 'test@example.com', name: 'Test User' });
      
      const userId = createResponse.body.user.id;
      
      // Test: Retrieve the user
      const response = await request(app)
        .get(`/api/users/${userId}`)
        .expect(200);
      
      expect(response.body.user.id).toBe(userId);
    });
    
    it('should return 404 for non-existent user', async () => {
      await request(app)
        .get('/api/users/99999')
        .expect(404);
    });
  });
});
```

### Testing with Test Containers

```javascript
// docker-integration.test.js
import { GenericContainer } from 'testcontainers';
import { Client } from 'pg';

describe('Database Integration with TestContainers', () => {
  let container;
  let client;
  
  beforeAll(async () => {
    // Start PostgreSQL container
    container = await new GenericContainer('postgres:14')
      .withEnvironment({
        POSTGRES_USER: 'test',
        POSTGRES_PASSWORD: 'test',
        POSTGRES_DB: 'testdb'
      })
      .withExposedPorts(5432)
      .start();
    
    const port = container.getMappedPort(5432);
    
    client = new Client({
      host: 'localhost',
      port: port,
      user: 'test',
      password: 'test',
      database: 'testdb'
    });
    
    await client.connect();
  }, 60000); // Increase timeout for container startup
  
  afterAll(async () => {
    await client.end();
    await container.stop();
  });
  
  it('should perform database operations', async () => {
    await client.query('CREATE TABLE users (id SERIAL, name TEXT)');
    await client.query("INSERT INTO users (name) VALUES ('Test User')");
    
    const result = await client.query('SELECT * FROM users');
    
    expect(result.rows).toHaveLength(1);
    expect(result.rows[0].name).toBe('Test User');
  });
});
```

## End-to-End Testing

### E2E Testing Scope

Test critical user journeys:

```markdown
✓ User registration and login flow
✓ Purchase/checkout process
✓ Account management workflows
✓ Core feature functionality
✓ Critical business processes
```

### E2E Test Example (Playwright)

```javascript
// e2e/checkout.spec.js
import { test, expect } from '@playwright/test';

test.describe('Checkout Flow', () => {
  test.beforeEach(async ({ page }) => {
    // Setup: Login as test user
    await page.goto('/login');
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    await expect(page).toHaveURL('/dashboard');
  });
  
  test('should complete purchase successfully', async ({ page }) => {
    // Add item to cart
    await page.goto('/products');
    await page.click('[data-testid="product-1"]');
    await page.click('[data-testid="add-to-cart"]');
    
    // Verify cart
    await page.click('[data-testid="cart-icon"]');
    await expect(page.locator('[data-testid="cart-item"]')).toHaveCount(1);
    
    // Proceed to checkout
    await page.click('[data-testid="checkout-button"]');
    
    // Fill shipping information
    await page.fill('[name="address"]', '123 Test St');
    await page.fill('[name="city"]', 'Test City');
    await page.fill('[name="zipCode"]', '12345');
    await page.click('[data-testid="continue-to-payment"]');
    
    // Fill payment information (test mode)
    await page.fill('[name="cardNumber"]', '4242424242424242');
    await page.fill('[name="expiry"]', '12/25');
    await page.fill('[name="cvc"]', '123');
    
    // Complete purchase
    await page.click('[data-testid="complete-purchase"]');
    
    // Verify success
    await expect(page).toHaveURL(/\/order-confirmation/);
    await expect(page.locator('[data-testid="order-number"]')).toBeVisible();
  });
  
  test('should handle payment failure gracefully', async ({ page }) => {
    // Navigate to checkout with item in cart
    await page.goto('/checkout');
    
    // Use card number that triggers failure
    await page.fill('[name="cardNumber"]', '4000000000000002');
    await page.fill('[name="expiry"]', '12/25');
    await page.fill('[name="cvc"]', '123');
    
    await page.click('[data-testid="complete-purchase"]');
    
    // Verify error message
    await expect(page.locator('[data-testid="error-message"]'))
      .toContainText('Payment failed');
    
    // Verify still on checkout page
    await expect(page).toHaveURL(/\/checkout/);
  });
});
```

### Visual Regression Testing

```javascript
// e2e/visual.spec.js
import { test, expect } from '@playwright/test';

test.describe('Visual Regression Tests', () => {
  test('homepage should match snapshot', async ({ page }) => {
    await page.goto('/');
    await expect(page).toHaveScreenshot('homepage.png');
  });
  
  test('product page should match snapshot', async ({ page }) => {
    await page.goto('/products/1');
    await page.waitForLoadState('networkidle');
    await expect(page).toHaveScreenshot('product-page.png', {
      maxDiffPixels: 100
    });
  });
});
```

## Testing Tools and Frameworks

### JavaScript/TypeScript

**Unit & Integration Testing**:
```json
{
  "devDependencies": {
    "jest": "^29.0.0",
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^6.0.0",
    "supertest": "^6.3.0"
  }
}
```

**E2E Testing**:
```json
{
  "devDependencies": {
    "@playwright/test": "^1.40.0",
    "cypress": "^13.0.0"
  }
}
```

### Python

```python
# requirements-dev.txt
pytest==7.4.0
pytest-cov==4.1.0
pytest-asyncio==0.21.0
pytest-mock==3.11.1
faker==19.0.0
```

### Java

```xml
<!-- pom.xml -->
<dependencies>
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.3</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.3.1</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

## Code Coverage

### Coverage Targets

| Test Type | Minimum Coverage | Target Coverage |
|-----------|-----------------|-----------------|
| Statements | 80% | 90% |
| Branches | 75% | 85% |
| Functions | 80% | 90% |
| Lines | 80% | 90% |

### Jest Coverage Configuration

```javascript
// jest.config.js
module.exports = {
  collectCoverage: true,
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'html'],
  coverageThreshold: {
    global: {
      statements: 80,
      branches: 75,
      functions: 80,
      lines: 80
    }
  },
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.test.{js,jsx,ts,tsx}',
    '!src/**/*.spec.{js,jsx,ts,tsx}',
    '!src/index.{js,ts}',
    '!src/setupTests.{js,ts}'
  ]
};
```

### Running Coverage

```bash
# Generate coverage report
npm test -- --coverage

# View HTML report
open coverage/lcov-report/index.html

# CI/CD integration
npm test -- --coverage --coverageReporters=lcov
```

### Coverage Anti-Patterns

❌ **Don't chase 100% coverage**: Focus on meaningful tests  
❌ **Don't test trivial code**: Getters, setters, simple assignments  
❌ **Don't write tests just for coverage**: Tests should add value  
❌ **Don't ignore uncovered critical paths**: Focus coverage on important code  

## Best Practices

### General Testing Principles

✅ **F.I.R.S.T. Principles**:
- **Fast**: Tests should run quickly
- **Independent**: Tests shouldn't depend on each other
- **Repeatable**: Same results every time
- **Self-Validating**: Pass or fail, no manual verification
- **Timely**: Written at the right time (TDD)

### Test Naming

```javascript
// ❌ Bad
it('test1', () => { /* ... */ });
it('works', () => { /* ... */ });

// ✅ Good
it('should return user when valid ID is provided', () => { /* ... */ });
it('should throw error when user ID does not exist', () => { /* ... */ });
it('should hash password before saving to database', () => { /* ... */ });
```

### Test Organization

```javascript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', () => { /* ... */ });
    it('should reject invalid email', () => { /* ... */ });
    it('should hash password', () => { /* ... */ });
  });
  
  describe('deleteUser', () => {
    it('should delete existing user', () => { /* ... */ });
    it('should throw error for non-existent user', () => { /* ... */ });
  });
});
```

### Test Data Management

```javascript
// Use factories for consistent test data
const createTestUser = (overrides = {}) => ({
  id: 1,
  email: 'test@example.com',
  name: 'Test User',
  role: 'user',
  createdAt: new Date('2024-01-01'),
  ...overrides
});

// Use in tests
it('should update user email', () => {
  const user = createTestUser({ email: 'old@example.com' });
  const updated = updateUserEmail(user, 'new@example.com');
  expect(updated.email).toBe('new@example.com');
});
```

## Common Patterns

### Testing Exceptions

```javascript
// Synchronous
expect(() => {
  dangerousFunction();
}).toThrow('Expected error message');

// Asynchronous
await expect(asyncDangerousFunction()).rejects.toThrow('Expected error');
```

### Testing Callbacks

```javascript
it('should call callback with result', (done) => {
  fetchData((error, result) => {
    expect(error).toBeNull();
    expect(result).toBeDefined();
    done();
  });
});
```

### Testing Events

```javascript
it('should emit event when user is created', () => {
  const eventEmitter = new EventEmitter();
  const userService = new UserService(eventEmitter);
  
  const eventHandler = jest.fn();
  eventEmitter.on('user:created', eventHandler);
  
  userService.createUser({ email: 'test@example.com' });
  
  expect(eventHandler).toHaveBeenCalledWith(
    expect.objectContaining({ email: 'test@example.com' })
  );
});
```

## Conclusion

Effective testing is crucial for:
- Reducing bugs in production
- Enabling confident refactoring
- Documenting expected behavior
- Facilitating continuous deployment
- Improving code quality

Remember: **Tests are an investment**. They take time to write but save significant time in debugging, maintenance, and preventing production issues.
