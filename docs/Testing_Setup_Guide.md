# PongKdor.com - Full Testing Setup Guide
**Next.js + Clean Architecture + TypeScript**

---

## 1. Recommended Testing Tools (2026)

| Tool          | Recommendation | Reason |
|---------------|----------------|--------|
| **Vitest**    | **Primary choice** | Faster than Jest, excellent TypeScript support, works great with Next.js |
| **Jest**      | Alternative    | More mature ecosystem, still widely used |
| **Testing Library** | For React components | Best for testing user interactions |
| **Playwright** | For E2E tests     | Modern, fast, great for full user flows |

**Recommendation**: Use **Vitest** + **Testing Library** + **Playwright**.

---

## 2. Project Setup

### Step 1: Install Dependencies

```bash
npm install -D vitest @vitejs/plugin-react jsdom
npm install -D @testing-library/react @testing-library/jest-dom
npm install -D @playwright/test
```

### Step 2: Configure Vitest

Create `vitest.config.ts` in the root:

```ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./src/test/setup.ts'],
    include: ['src/**/*.{test,spec}.{ts,tsx}'],
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});
```

### Step 3: Create Setup File

**`src/test/setup.ts`**

```ts
import '@testing-library/jest-dom';
```

### Step 4: Add Test Scripts to `package.json`

```json
{
  "scripts": {
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest run --coverage",
    "test:e2e": "playwright test"
  }
}
```

---

## 3. Recommended Folder Structure

```
src/
├── app/
├── domain/
├── application/
├── infrastructure/
├── lib/
├── components/
└── __tests__/
    ├── doubles/                    # Test doubles (Fakes, Mocks)
    │   ├── FakeArticleRepository.ts
    │   └── FakeProductRepository.ts
    ├── application/                # Use Case tests
    │   └── blog/
    │       └── GetArticleBySlugUseCase.test.ts
    ├── infrastructure/             # Integration tests (optional)
    └── e2e/                        # Playwright tests
```

---

## 4. Example: Testing a Use Case

### Create Test Double

**`src/__tests__/doubles/FakeArticleRepository.ts`**

```ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';
import { Article } from '@/domain/entities/Article';

export class FakeArticleRepository implements ArticleRepository {
  private articles: Article[] = [];

  async findBySlug(slug: string) {
    return this.articles.find(a => a.slug === slug) || null;
  }

  async findAll() {
    return this.articles;
  }

  addArticle(article: Article) {
    this.articles.push(article);
  }
}
```

### Write the Test

**`src/__tests__/application/GetArticleBySlugUseCase.test.ts`**

```ts
import { describe, it, expect, beforeEach } from 'vitest';
import { GetArticleBySlugUseCase } from '@/application/blog/GetArticleBySlugUseCase';
import { FakeArticleRepository } from '../doubles/FakeArticleRepository';
import { Article } from '@/domain/entities/Article';

describe('GetArticleBySlugUseCase', () => {
  let useCase: GetArticleBySlugUseCase;
  let fakeRepo: FakeArticleRepository;

  beforeEach(() => {
    fakeRepo = new FakeArticleRepository();
    useCase = new GetArticleBySlugUseCase(fakeRepo);
  });

  it('should return the article when it exists', async () => {
    const article = new Article('1', 'Test', 'test-slug', 'content', new Date(), true);
    fakeRepo.addArticle(article);

    const result = await useCase.execute('test-slug');

    expect(result?.title).toBe('Test');
  });

  it('should return null when article does not exist', async () => {
    const result = await useCase.execute('non-existing');
    expect(result).toBeNull();
  });
});
```

---

## 5. Running Tests

```bash
# Run tests in watch mode
npm run test

# Run tests with UI
npm run test:ui

# Run once with coverage
npm run test:coverage

# Run E2E tests
npm run test:e2e
```

---

## 6. Best Practices

- Write tests for **Use Cases** first (highest value)
- Use **Fakes** for most cases (simple and readable)
- Keep tests fast and isolated
- Use meaningful test descriptions
- Aim for high coverage on business logic (Application layer)

---

## 7. Next Steps Recommendation

1. Set up Vitest as shown above
2. Create your first `FakeArticleRepository`
3. Write tests for your most important Use Cases
4. Add E2E tests with Playwright for critical user flows (e.g., adding item to cart)

---

This setup gives you a modern, fast, and maintainable testing foundation aligned with Clean Architecture.

Would you like me to create **Playwright E2E test examples** next (for homepage, blog, or cart flow)? Or move to another topic?