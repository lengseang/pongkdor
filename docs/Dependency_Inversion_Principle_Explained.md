# Dependency Inversion Principle (DIP) Explained
**For PongKdor.com Project**

---

## What is the Dependency Inversion Principle?

The **Dependency Inversion Principle** is one of the five SOLID principles of object-oriented design.

**Official Definition** (Robert C. Martin):

> 1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
> 2. Abstractions should not depend on details. Details should depend on abstractions.

### Simple Explanation:

Instead of high-level code depending directly on concrete implementations (like a specific database or CMS), both should depend on **interfaces** (abstractions).

This makes your code more flexible, testable, and decoupled.

---

## Why DIP Matters

Without DIP:
- Your business logic becomes tightly coupled to specific technologies (e.g., Sanity, Shopify, PostgreSQL).
- Changing the database or CMS requires rewriting a lot of code.
- Testing becomes difficult because you can't easily mock dependencies.

With DIP:
- Business logic depends only on interfaces.
- You can easily swap implementations (Sanity → Payload, or even a mock for testing).
- Code becomes much more maintainable and scalable.

---

## How DIP Works in Clean Architecture

In Clean Architecture, **Dependency Inversion** is one of the core rules:

- The **Domain** and **Application** layers define **interfaces**.
- The **Infrastructure** layer provides the **concrete implementations**.
- Dependencies flow **inward** toward the Domain.

### Visual Flow:

```
High-level (Application / Use Cases)
          ↓ depends on abstraction (interface)
Domain Layer (defines interfaces)
          ↑ implemented by
Infrastructure Layer (SanityRepository, ShopifyRepository)
```

---

## Real Example for PongKdor.com

### Problem Without DIP:

```ts
// Bad - Direct dependency on Sanity
class GetArticleBySlugUseCase {
  async execute(slug: string) {
    // Directly calls Sanity client
    const data = await sanityClient.fetch(`*[_type == "article" && slug.current == $slug]`);
    return data;
  }
}
```

If you want to switch to Payload CMS later, you have to rewrite this Use Case.

### Solution With DIP (Recommended):

**Step 1: Define Interface in Domain Layer**

```ts
// domain/repositories/ArticleRepository.ts
export interface ArticleRepository {
  findBySlug(slug: string): Promise<Article | null>;
}
```

**Step 2: Create Use Case (depends only on interface)**

```ts
// application/blog/GetArticleBySlugUseCase.ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';

export class GetArticleBySlugUseCase {
  constructor(private articleRepository: ArticleRepository) {}

  async execute(slug: string) {
    return await this.articleRepository.findBySlug(slug);
  }
}
```

**Step 3: Implement the interface in Infrastructure**

```ts
// infrastructure/sanity/SanityArticleRepository.ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';

export class SanityArticleRepository implements ArticleRepository {
  async findBySlug(slug: string) {
    // Actual Sanity GROQ query here
    const data = await sanityClient.fetch(...);
    return new Article(...);
  }
}
```

Now you can easily create:

- `PayloadArticleRepository`
- `MockArticleRepository` (for testing)
- `InMemoryArticleRepository`

Without changing the `GetArticleBySlugUseCase`.

---

## Benefits in Your Project

| Benefit                        | How DIP Helps                                      |
|--------------------------------|----------------------------------------------------|
| Easy CMS switch                | Change only the repository implementation          |
| Better testing                 | Use mock repositories in tests                     |
| Cleaner code                   | Business logic is not polluted with API details    |
| Team collaboration             | Different developers can work on different layers  |
| Long-term flexibility          | Future-proof against technology changes            |

---

## How to Apply DIP in Your Project

1. **Define interfaces** in the `domain/repositories` folder
2. **Implement interfaces** in the `infrastructure` folder
3. **Inject dependencies** (manually or using a simple container)
4. **Never import infrastructure** directly into Application or Domain layers

---

## Summary

**Dependency Inversion Principle** is the reason we use the **Repository Pattern** in Clean Architecture.

- High-level code (Use Cases) depends on **abstractions** (interfaces)
- Low-level code (Sanity, Shopify integrations) implements those abstractions
- This creates a **decoupled**, **testable**, and **flexible** system

This is one of the most important concepts for building maintainable backend architecture in 2026.

---

Would you like me to show you how to implement **Dependency Injection** in Next.js (simple version) next? Or explain another SOLID principle?