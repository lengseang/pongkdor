# PongKdor.com - Expanded Backend Design Document
**Version**: 1.1 (Detailed Clean Architecture)  
**Date**: June 22, 2026

---

## 1. Backend Architecture Overview

We follow **Clean Architecture** with **Domain-Driven Design (DDD)** influences. The goal is to keep business logic independent of frameworks and external services (Shopify, Sanity, Database).

**Core Principle**:  
Business logic should be testable and reusable even if we change Shopify to another e-commerce platform in the future.

---

## 2. Layered Architecture (Detailed)

### 2.1 Domain Layer (Core)
This is the heart of the system. It contains:
- Entities
- Value Objects
- Domain Events
- Repository Interfaces (not implementations)

**Example Entities**:
- `Article` (blog post)
- `Product` (merch)
- `Cart`
- `User` (future)

### 2.2 Application Layer (Use Cases)
Contains the business logic / use cases.

Examples of Use Cases:
- `CreateArticleUseCase`
- `GetArticleBySlugUseCase`
- `AddToCartUseCase`
- `CheckoutUseCase`
- `GetFeaturedProductsUseCase`

### 2.3 Infrastructure Layer
Contains concrete implementations:
- `SanityArticleRepository`
- `ShopifyProductRepository`
- `SupabaseUserRepository` (future)

### 2.4 Presentation Layer
- Next.js Server Components
- API Routes (if needed)
- Server Actions

---

## 3. Detailed Use Cases (MVP Scope)

### Blog Domain

**Use Case: Get Article List**
- Input: filters (category, search, page)
- Output: list of articles + pagination
- Involved: `ArticleRepository`

**Use Case: Get Article By Slug**
- Input: slug
- Output: full article with related content
- Business Rule: Only published articles are visible

### Shop Domain

**Use Case: Get Product List**
- Input: filters
- Output: products from Shopify + local enhancements (if any)

**Use Case: Add Item To Cart**
- Input: productId, variantId, quantity
- Output: updated cart
- Business Rule: Validate stock via Shopify

**Use Case: Create Checkout**
- Handled mostly by Shopify
- We only trigger and return checkout URL

---

## 4. Repository Pattern (Detailed)

We define interfaces in the Domain layer and implement them in Infrastructure.

### Example: Article Repository

**Domain Interface** (`domain/repositories/ArticleRepository.ts`):
```ts
export interface ArticleRepository {
  findAll(filters?: ArticleFilters): Promise<Article[]>;
  findBySlug(slug: string): Promise<Article | null>;
  findFeatured(limit: number): Promise<Article[]>;
}
```

**Implementation** (`infrastructure/sanity/SanityArticleRepository.ts`):
```ts
import { sanityClient } from '@/lib/sanity';

export class SanityArticleRepository implements ArticleRepository {
  async findAll(filters?: ArticleFilters) {
    // GROQ query to Sanity
  }

  async findBySlug(slug: string) {
    // GROQ query
  }
}
```

This pattern allows us to easily swap Sanity for another CMS later.

---

## 5. Dependency Injection

We will use a simple dependency injection approach (or a lightweight library like `tsyringe` later if needed).

Example in a Server Component or Route Handler:
```ts
const articleRepo = new SanityArticleRepository();
const useCase = new GetArticleBySlugUseCase(articleRepo);
const article = await useCase.execute(slug);
```

---

## 6. Error Handling Strategy

- Use custom error classes (`DomainError`, `ValidationError`, `NotFoundError`)
- Centralized error handling in middleware or error boundaries
- Proper logging with Sentry

---

## 7. Future Scalability Considerations

When the project grows:
- Move complex business logic to a separate NestJS backend
- Introduce message queue (BullMQ or Inngest)
- Add event-driven architecture for things like "Order Placed" events
- Consider CQRS for read-heavy blog vs write-heavy shop operations

---

## 8. Validation

This expanded design follows **Clean Architecture** properly while remaining practical. It gives us:
- Testability
- Maintainability
- Flexibility to change external services
- Clear separation between business rules and implementation details

**End of Expanded Backend Design Document**