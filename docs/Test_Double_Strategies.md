# Implementing Test Double Strategies
**For PongKdor.com (Clean Architecture + Next.js)**

---

## What Are Test Doubles?

**Test Doubles** are objects that replace real dependencies during testing. They allow you to test your code in isolation without relying on external services like databases, APIs (Sanity, Shopify), or other complex systems.

They are essential when following **Clean Architecture** and the **Dependency Inversion Principle**.

---

## Types of Test Doubles

| Type     | Purpose                              | When to Use                              | Example |
|----------|--------------------------------------|------------------------------------------|--------|
| **Dummy**    | Placeholder object                   | When the value is not used               | Passing an empty object |
| **Stub**     | Provides predefined responses        | Control indirect inputs                  | Return fixed data from repository |
| **Spy**      | Records information about calls      | Verify interactions                      | Check if a method was called |
| **Mock**     | Pre-programmed with expectations     | Verify behavior + replace dependency     | Mock repository to throw error |
| **Fake**     | Working implementation with shortcuts| When real implementation is too slow/heavy | In-memory repository |

---

## Why Test Doubles Are Important for Your Project

Without Test Doubles:
- Your tests would depend on real Sanity or Shopify calls
- Tests become slow and flaky
- You can't test error scenarios easily

With Test Doubles:
- Fast, reliable unit tests
- Test business logic in isolation
- Easy to simulate success, failure, and edge cases

---

## Practical Example: Testing Use Cases

We'll use the `GetArticleBySlugUseCase` from previous documents.

### 1. Create a Test Double (Stub/Mock)

Create a fake repository for testing:

**`src/__tests__/doubles/FakeArticleRepository.ts`**

```ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';
import { Article } from '@/domain/entities/Article';

export class FakeArticleRepository implements ArticleRepository {
  private articles: Article[] = [];

  async findBySlug(slug: string): Promise<Article | null> {
    return this.articles.find(a => a.slug === slug) || null;
  }

  async findAll(): Promise<Article[]> {
    return this.articles;
  }

  // Helper method for tests
  addArticle(article: Article) {
    this.articles.push(article);
  }
}
```

### 2. Write a Unit Test for the Use Case

**`src/__tests__/application/GetArticleBySlugUseCase.test.ts`**

```ts
import { GetArticleBySlugUseCase } from '@/application/blog/GetArticleBySlugUseCase';
import { FakeArticleRepository } from '../doubles/FakeArticleRepository';
import { Article } from '@/domain/entities/Article';

describe('GetArticleBySlugUseCase', () => {
  let useCase: GetArticleBySlugUseCase;
  let fakeRepository: FakeArticleRepository;

  beforeEach(() => {
    fakeRepository = new FakeArticleRepository();
    useCase = new GetArticleBySlugUseCase(fakeRepository);
  });

  it('should return an article when slug exists', async () => {
    const testArticle = new Article('1', 'Test Title', 'test-slug', 'content', new Date(), true);
    fakeRepository.addArticle(testArticle);

    const result = await useCase.execute('test-slug');

    expect(result).toEqual(testArticle);
  });

  it('should return null when article does not exist', async () => {
    const result = await useCase.execute('non-existing-slug');

    expect(result).toBeNull();
  });
});
```

---

## Recommended Strategy for PongKdor.com

| Layer                  | Testing Approach                     | Test Double Type     |
|------------------------|--------------------------------------|----------------------|
| **Domain**             | Unit test entities                   | Rarely needed        |
| **Application (Use Cases)** | Unit test with fake repositories | **Fake** or **Stub** |
| **Infrastructure**     | Integration tests                    | Real implementation  |
| **Presentation**       | Component tests + E2E                | Sometimes mocks      |

**Best Practice**:
- Use **Fakes** for most unit tests (simple in-memory implementations)
- Use **Mocks** when you need to verify specific method calls
- Use real implementations only in integration tests

---

## Tools Recommendation

For testing in Next.js + TypeScript:

- **Jest** (most popular)
- **Vitest** (faster, modern alternative)
- **Testing Library** (for React component testing)
- **Playwright** or **Cypress** (for E2E tests)

---

## Summary

Test Doubles are a key part of writing maintainable and reliable code when using Clean Architecture.

By depending on **interfaces** (thanks to Dependency Inversion), you can easily replace real repositories with test doubles during testing.

This makes your business logic (Use Cases) fast and easy to test in isolation.

---

Would you like me to create a full testing setup guide (Jest/Vitest configuration + example test structure) for your project next?