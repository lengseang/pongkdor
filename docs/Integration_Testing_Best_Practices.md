# Integration Testing Best Practices
**For PongKdor.com (Next.js + Clean Architecture)**

---

## What is Integration Testing?

**Integration Testing** verifies that different parts of your system work correctly together.

Unlike **Unit Tests** (which test one piece in isolation using Test Doubles), integration tests check the interaction between:

- Use Cases + Real Repositories
- Application Layer + Infrastructure Layer
- API Routes + External Services (Sanity, Shopify)
- Multiple components working together

---

## When to Use Integration Testing

| Test Type         | Scope                              | Speed     | When to Use                              |
|-------------------|------------------------------------|-----------|------------------------------------------|
| **Unit Test**     | Single Use Case / Function         | Very Fast | Business logic in isolation              |
| **Integration Test** | Use Case + Real Repository      | Medium    | Data flow between layers                 |
| **E2E Test**      | Full user flow (UI → Backend)      | Slow      | Critical user journeys                   |

**Best Practice**: 
- Write **many unit tests**
- Write **fewer but high-value integration tests**
- Write **selective E2E tests** for the most important flows

---

## Integration Testing Strategy for Your Project

### Recommended Focus Areas

1. **Use Cases + Real Repositories**
   - Test `GetArticleBySlugUseCase` with actual Sanity data (or test database)
   - Test cart-related use cases with Shopify

2. **API Routes**
   - Test your Next.js API routes that call Use Cases

3. **Data Transformation**
   - Ensure data from Sanity/Shopify is correctly transformed into your Domain entities

4. **Error Handling**
   - Test how the system behaves when external services fail or return unexpected data

---

## Best Practices

### 1. Use Real Implementations (When Possible)

For integration tests, prefer using **real repository implementations** instead of Fakes:

```ts
// Integration test example
import { SanityArticleRepository } from '@/infrastructure/sanity/SanityArticleRepository';
import { GetArticleBySlugUseCase } from '@/application/blog/GetArticleBySlugUseCase';

describe('GetArticleBySlugUseCase (Integration)', () => {
  it('should fetch real article from Sanity', async () => {
    const repository = new SanityArticleRepository();
    const useCase = new GetArticleBySlugUseCase(repository);

    const article = await useCase.execute('some-real-slug');

    expect(article).not.toBeNull();
    expect(article?.title).toBeDefined();
  });
});
```

### 2. Use Test Data / Dedicated Test Environment

- Create a separate Sanity dataset for testing (e.g., `test` dataset)
- Or use a small set of stable test content
- Never run integration tests against production data

### 3. Keep Tests Focused

Integration tests should still be relatively focused. Don’t try to test the entire system in one test.

Good example:
- Test one Use Case + its repository

Bad example:
- Test the entire homepage rendering + multiple Use Cases in one test

### 4. Handle External Dependencies Carefully

- Use environment variables to switch between real and test services
- Mock external services only when they are slow or unreliable
- Prefer real calls for true integration testing

### 5. Test Error Scenarios

Integration tests are great for testing real-world failures:

- What happens when Sanity returns an error?
- What happens when a product is out of stock in Shopify?
- How does the system handle network timeouts?

---

## Recommended Tools

| Purpose                    | Tool                    | Recommendation |
|---------------------------|-------------------------|----------------|
| Integration Tests         | Vitest                  | Primary        |
| API Route Testing         | Supertest (if using Express) or built-in `fetch` | Use Next.js test utilities |
| Database/CMS Seeding      | Custom scripts          | Recommended    |
| E2E                       | Playwright              | Best choice    |

---

## Example Project Structure for Tests

```
src/
└── __tests__/
    ├── unit/                    # Fast unit tests with Fakes
    ├── integration/             # Integration tests with real services
    │   ├── blog/
    │   │   └── GetArticleBySlugUseCase.integration.test.ts
    │   └── shop/
    │       └── AddToCartUseCase.integration.test.ts
    └── e2e/                     # Playwright tests
```

---

## Summary: Testing Pyramid Recommendation

```
        E2E Tests (Few)
      Integration Tests (Some)
    Unit Tests (Many) ← Most of your tests should be here
```

- **Unit Tests**: Fast feedback on business logic
- **Integration Tests**: Verify layers work together correctly
- **E2E Tests**: Validate critical user journeys

---

This balanced approach will give you good confidence in your system without making your test suite too slow.

Would you like me to create example integration test files for specific features (like Article fetching or Cart functionality)? Or move to another topic?