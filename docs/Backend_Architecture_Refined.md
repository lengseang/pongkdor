# PongKdor.com - Refined Backend Architecture
**Version**: 2.0 (Production-Ready Clean Architecture)  
**Date**: June 22, 2026

---

## 1. Architecture Goals

- Keep business logic independent of external services (Shopify, Sanity, Database)
- Easy to test and maintain
- Scalable from MVP to larger system
- Practical for a solo founder (not over-engineered)

**Core Principle**:  
**Dependency Rule** — Dependencies point inward. The Domain layer has no knowledge of Next.js, Shopify, or Sanity.

---

## 2. Final Recommended Architecture

We use a **Clean + Modular** approach inside Next.js.

```
src/
├── app/                          # Presentation Layer (Next.js)
│   ├── (routes)/
│   ├── api/                      # API Routes (if needed)
│   └── layout.tsx
├── application/                  # Application Layer (Use Cases)
│   ├── blog/
│   │   ├── GetArticleListUseCase.ts
│   │   └── GetArticleBySlugUseCase.ts
│   └── shop/
│       ├── GetProductsUseCase.ts
│       └── AddToCartUseCase.ts
├── domain/                       # Domain Layer (Core Business)
│   ├── entities/
│   │   ├── Article.ts
│   │   └── Product.ts
│   ├── repositories/             # Interfaces only
│   │   ├── ArticleRepository.ts
│   │   └── ProductRepository.ts
│   └── value-objects/
├── infrastructure/               # Infrastructure Layer
│   ├── sanity/
│   │   └── SanityArticleRepository.ts
│   ├── shopify/
│   │   └── ShopifyProductRepository.ts
│   └── database/                 # Future custom DB
└── lib/
    └── container.ts                # Simple dependency injection
```

---

## 3. Layer Responsibilities

| Layer            | Responsibility                              | Can Depend On          | Should Not Depend On      |
|------------------|---------------------------------------------|------------------------|---------------------------|
| **Domain**       | Business rules & entities                   | Nothing (pure)         | Any framework or service  |
| **Application**  | Orchestrates use cases                      | Domain                 | Infrastructure            |
| **Infrastructure**| Concrete implementations (APIs, DB)        | Application + Domain   | Presentation              |
| **Presentation** | UI, routing, API endpoints                  | Application            | Domain directly (via use cases) |

---

## 4. Detailed Examples

### 4.1 Domain Layer Example

**`domain/entities/Article.ts`**
```ts
export class Article {
  constructor(
    public id: string,
    public title: string,
    public slug: string,
    public content: string,
    public publishedAt: Date,
    public isFeatured: boolean
  ) {}
}
```

**`domain/repositories/ArticleRepository.ts`** (Interface only)
```ts
export interface ArticleRepository {
  findAll(): Promise<Article[]>;
  findBySlug(slug: string): Promise<Article | null>;
  findFeatured(limit?: number): Promise<Article[]>;
}
```

### 4.2 Application Layer Example

**`application/blog/GetArticleBySlugUseCase.ts`**
```ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';
import { Article } from '@/domain/entities/Article';

export class GetArticleBySlugUseCase {
  constructor(private articleRepository: ArticleRepository) {}

  async execute(slug: string): Promise<Article | null> {
    const article = await this.articleRepository.findBySlug(slug);
    
    if (!article) {
      throw new Error('Article not found');
    }
    
    return article;
  }
}
```

### 4.3 Infrastructure Implementation

**`infrastructure/sanity/SanityArticleRepository.ts`**
```ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';
import { Article } from '@/domain/entities/Article';
import { sanityClient } from '@/lib/sanity';

export class SanityArticleRepository implements ArticleRepository {
  async findBySlug(slug: string): Promise<Article | null> {
    const data = await sanityClient.fetch(
      `*[_type == "article" && slug.current == $slug][0]`,
      { slug }
    );
    
    if (!data) return null;
    
    return new Article(
      data._id,
      data.title,
      data.slug.current,
      data.content,
      new Date(data.publishedAt),
      data.isFeatured
    );
  }

  // Implement other methods...
}
```

---

## 5. Dependency Injection (Simple Version)

Create a simple container:

**`lib/container.ts`**
```ts
import { SanityArticleRepository } from '@/infrastructure/sanity/SanityArticleRepository';
import { GetArticleBySlugUseCase } from '@/application/blog/GetArticleBySlugUseCase';

export const articleRepository = new SanityArticleRepository();
export const getArticleBySlugUseCase = new GetArticleBySlugUseCase(articleRepository);
```

Then use it in Server Components or Route Handlers.

---

## 6. How It Works With Shopify + Sanity

- **Blog** → Mostly Sanity (custom repository)
- **Shop** → Mostly Shopify (we create repository interfaces that talk to Shopify Storefront API)
- Future custom features → Can be added in Domain + Application without touching external services

---

## 7. Benefits of This Architecture

- Business logic is protected and testable
- Easy to change Sanity → Another CMS
- Easy to change Shopify → Another e-commerce platform
- Clear separation makes the codebase easier to understand as it grows
- Good foundation for adding tests later

---

## 8. When to Evolve Further

Only consider these when you actually need them:
- Move to full NestJS backend → When custom backend logic becomes complex
- Add message queue → For background jobs (order processing, emails)
- CQRS → When read and write operations have very different needs

---

**This architecture is clean, practical, and enterprise-grade while staying realistic for your project.**

**End of Refined Backend Architecture Document**