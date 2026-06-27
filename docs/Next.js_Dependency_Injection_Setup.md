# Next.js Dependency Injection Setup
**For PongKdor.com (Clean Architecture)**

---

## Why We Need Dependency Injection in Next.js

Even in Next.js, Dependency Injection helps us:
- Follow the **Dependency Inversion Principle**
- Keep business logic decoupled from external services (Sanity, Shopify)
- Make code more testable
- Make it easier to swap implementations later

---

## Recommended Approach: Simple Manual DI

For most Next.js projects (especially solo founder), we recommend **manual dependency injection** instead of heavy DI containers. It's simpler, has zero overhead, and works great with Server Components.

---

## Step-by-Step Setup

### 1. Create the Container File

Create a file to hold all your dependencies:

**`src/lib/container.ts`**

```ts
import { SanityArticleRepository } from '@/infrastructure/sanity/SanityArticleRepository';
import { GetArticleBySlugUseCase } from '@/application/blog/GetArticleBySlugUseCase';

// Repositories
export const articleRepository = new SanityArticleRepository();

// Use Cases
export const getArticleBySlugUseCase = new GetArticleBySlugUseCase(articleRepository);

// You can add more repositories and use cases here as the project grows
```

### 2. Example: Article Repository Interface

**`src/domain/repositories/ArticleRepository.ts`**

```ts
import { Article } from '@/domain/entities/Article';

export interface ArticleRepository {
  findBySlug(slug: string): Promise<Article | null>;
  findAll(): Promise<Article[]>;
}
```

### 3. Example: Use Case

**`src/application/blog/GetArticleBySlugUseCase.ts`**

```ts
import { ArticleRepository } from '@/domain/repositories/ArticleRepository';
import { Article } from '@/domain/entities/Article';

export class GetArticleBySlugUseCase {
  constructor(private articleRepository: ArticleRepository) {}

  async execute(slug: string): Promise<Article | null> {
    return this.articleRepository.findBySlug(slug);
  }
}
```

### 4. Using It in a Next.js Server Component

**`src/app/blog/[slug]/page.tsx`**

```tsx
import { getArticleBySlugUseCase } from '@/lib/container';

interface Props {
  params: { slug: string };
}

export default async function ArticlePage({ params }: Props) {
  const article = await getArticleBySlugUseCase.execute(params.slug);

  if (!article) {
    return <div>Article not found</div>;
  }

  return (
    <article>
      <h1>{article.title}</h1>
      {/* Render content */}
    </article>
  );
}
```

### 5. Using It in a Route Handler (API Route)

**`src/app/api/articles/[slug]/route.ts`**

```ts
import { NextRequest } from 'next/server';
import { getArticleBySlugUseCase } from '@/lib/container';

export async function GET(
  request: NextRequest,
  { params }: { params: { slug: string } }
) {
  const article = await getArticleBySlugUseCase.execute(params.slug);

  if (!article) {
    return Response.json({ error: 'Not found' }, { status: 404 });
  }

  return Response.json(article);
}
```

---

## Alternative: Using a Lightweight DI Library (Optional)

If your project grows and you want more advanced features, you can use:

- `tsyringe` (popular)
- `inversify` (more powerful but heavier)

However, for most Next.js projects in 2026, **manual DI** is cleaner and sufficient.

---

## Best Practices

1. **Create one container file** (`lib/container.ts`)
2. **Instantiate dependencies once** at the top level
3. **Never create new instances** of repositories/use cases inside components
4. **Keep the container simple** — avoid complex hierarchies early on
5. **Use it mainly in Server Components** and Route Handlers

---

## Folder Structure Reminder

```
src/
├── lib/
│   └── container.ts              ← Dependency Injection container
├── domain/
│   └── repositories/
├── application/
│   └── blog/
├── infrastructure/
│   └── sanity/
└── app/
    └── blog/
```

---

## Summary

| Approach              | Complexity | Recommended For          | Recommendation |
|-----------------------|------------|--------------------------|----------------|
| **Manual DI**         | Low        | Most Next.js projects    | **Best choice** |
| `tsyringe`            | Medium     | Larger projects          | Optional later |
| Full DI Container     | High       | Very large enterprise    | Usually overkill |

For **PongKdor.com**, start with **manual dependency injection** using a simple `container.ts` file. It's clean, performant, and follows Clean Architecture principles well.

---

Would you like me to show you how to expand this container as the project grows, or add examples for Shopify repositories as well?