# Clean Architecture Layers Explained
**For PongKdor.com Project**

---

## What is Clean Architecture?

Clean Architecture is a software design philosophy created by Robert C. Martin ("Uncle Bob"). 

**Main Goal**: 
- Make your code **independent** of frameworks, databases, and external services.
- Make business logic **testable** and **maintainable**.
- Allow you to change technologies (e.g. switch from Sanity to another CMS) without rewriting your core business rules.

---

## The 4 Main Layers (Simplified)

Clean Architecture is usually divided into **4 layers**, with dependencies flowing **inward**.

```
          Presentation Layer (UI / Next.js)
                     ↑
          Application Layer (Use Cases)
                     ↑
               Domain Layer (Core)
                     ↑
          Infrastructure Layer (External Services)
```

**Rule**: Inner layers should **never** depend on outer layers.  
Outer layers can depend on inner layers.

---

## 1. Domain Layer (The Core)

**Purpose**: Contains your **business rules** and **core entities**. This is the most important and stable part of your application.

**What belongs here**:
- Entities (e.g., `Article`, `Product`, `Cart`)
- Value Objects
- Domain Events
- Repository **Interfaces** (not implementations)

**Example for PongKdor**:
- `Article` entity with rules like "Only published articles can be shown to users"
- `Product` entity
- `ArticleRepository` interface (defines what methods are needed)

**Key Rule**: This layer should not know anything about Next.js, Sanity, Shopify, or databases.

---

## 2. Application Layer (Use Cases)

**Purpose**: Contains the **business logic** / what the application actually does.

**What belongs here**:
- Use Cases (e.g., `GetArticleBySlugUseCase`, `AddToCartUseCase`)
- Orchestrates flow between entities and repositories

**Example**:
```ts
// GetArticleBySlugUseCase.ts
export class GetArticleBySlugUseCase {
  constructor(private articleRepository: ArticleRepository) {}

  async execute(slug: string) {
    const article = await this.articleRepository.findBySlug(slug);
    if (!article || !article.isPublished) {
      throw new Error("Article not found");
    }
    return article;
  }
}
```

This layer knows about Domain entities but **not** about how data is stored (Sanity or database).

---

## 3. Infrastructure Layer

**Purpose**: Contains the **concrete implementations** of how data is fetched/saved.

**What belongs here**:
- Repository implementations (e.g., `SanityArticleRepository`)
- API clients
- Database connections
- External service integrations (Shopify, Sanity, etc.)

**Example**:
```ts
export class SanityArticleRepository implements ArticleRepository {
  async findBySlug(slug: string) {
    // Actual GROQ query to Sanity here
    return sanityClient.fetch(...);
  }
}
```

This is where you connect to **Sanity**, **Shopify**, or a database.

---

## 4. Presentation Layer (UI Layer)

**Purpose**: Handles everything the user sees and interacts with.

**What belongs here**:
- Next.js pages / components
- Server Components
- API Routes
- Forms, buttons, animations

**Example**:
- A Next.js Server Component that calls `GetArticleBySlugUseCase` and renders the article
- Client Components with Framer Motion animations

---

## Dependency Flow (Very Important)

```
Presentation Layer
        ↓ (depends on)
Application Layer
        ↓ (depends on)
Domain Layer
        ↑
Infrastructure Layer (implements interfaces from Domain)
```

- **Presentation** uses Use Cases
- **Application** uses Domain Entities and Interfaces
- **Infrastructure** implements the interfaces defined in Domain

---

## Why This Matters for Your Project

| Benefit                        | How Clean Architecture Helps                          |
|--------------------------------|-------------------------------------------------------|
| Easy to change CMS             | Just change the Infrastructure layer (Sanity → Payload) |
| Easy to test                   | Business logic in Use Cases can be unit tested        |
| Better organization            | Clear separation of concerns                          |
| Long-term maintainability      | Code doesn't become messy as project grows            |
| Scalability                    | Easier to add new features without breaking old ones  |

---

## Simple Real-World Example for PongKdor

**Feature**: Show an article on the website

1. **Domain**: `Article` entity + `ArticleRepository` interface
2. **Application**: `GetArticleBySlugUseCase`
3. **Infrastructure**: `SanityArticleRepository` (implements the interface)
4. **Presentation**: Next.js page that calls the Use Case and displays the article

If you later switch to Payload CMS, you only change the **Infrastructure** layer. Everything else stays the same.

---

## Summary

| Layer            | Focus                     | Changes Often? | Depends On          |
|------------------|---------------------------|----------------|---------------------|
| **Domain**       | Business rules & entities | Rarely         | Nothing             |
| **Application**  | What the app does         | Sometimes      | Domain              |
| **Infrastructure** | How data is stored/fetched | Often       | Application + Domain |
| **Presentation** | User interface            | Often          | Application         |

---

This is the foundation of clean, professional, and scalable backend architecture.

Would you like me to show you a **complete folder structure** based on these layers for your Next.js project? Or explain any layer in more detail?