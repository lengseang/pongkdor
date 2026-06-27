# PongKdor.com - Project Stack, Best Practices & Integration Guide
**Version**: 1.0  
**Date**: June 22, 2026

---

## 1. Overall Recommended Stack

| Layer              | Technology                          | Type              | Purpose                          |
|--------------------|-------------------------------------|-------------------|----------------------------------|
| **Frontend**       | Next.js 15 (App Router)             | Framework         | Main application framework       |
| **Language**       | TypeScript                          | Language          | Type safety                      |
| **Styling**        | Tailwind CSS + shadcn/ui            | Styling + UI      | Rapid UI development             |
| **Animations**     | Framer Motion                       | Animation         | Complex reactive animations      |
| **State**          | Zustand                             | Client State      | Lightweight client state         |
| **Forms**          | React Hook Form + Zod               | Forms             | Type-safe form handling          |
| **CMS**            | Sanity.io                           | Headless CMS      | Blog & structured content        |
| **E-commerce**     | Shopify (Headless)                  | E-commerce        | Merch store & checkout           |
| **Hosting**        | Vercel                              | Platform          | Frontend hosting + deployment    |
| **Architecture**   | Clean Architecture (in Next.js)     | Pattern           | Maintainable & scalable code     |

---

## 2. Detailed Stack Breakdown + Best Practices

### 2.1 Next.js 15 (App Router)

**Why Chosen**:
- Best performance and SEO in 2026
- Server Components by default
- Excellent integration with React 19

**Best Practices**:
- Use **Server Components** by default
- Use `loading.tsx` and `error.tsx` for better UX
- Leverage Route Groups for organization
- Use Server Actions for mutations when possible

**Integration Points**:
- Fetches data from Sanity and Shopify directly in Server Components
- Passes data to Client Components when interactivity is needed

---

### 2.2 TypeScript

**Best Practices**:
- Strict mode enabled
- Use interfaces/types for all data shapes
- Generate types from Sanity schema (using `@sanity-codegen` or similar)
- Use Zod for runtime validation

---

### 2.3 Tailwind CSS + shadcn/ui

**Best Practices**:
- Use shadcn/ui as base component library
- Extend with custom components when needed
- Follow consistent spacing and typography scale
- Use Tailwind’s responsive utilities heavily (mobile-first)

**Integration**:
- Works seamlessly with Framer Motion for animated components
- shadcn components can be easily customized with Tailwind classes

---

### 2.4 Framer Motion

**Best Practices**:
- Use `motion` components for animations
- Prefer `animate` prop + variants for complex sequences
- Use `layout` animations for smooth list/item transitions
- Keep animations performant (avoid animating expensive properties)

**Integration**:
- Animate shadcn/ui components
- Create scroll-triggered and gesture-based interactions
- Works great with Next.js App Router

---

### 2.5 Zustand (Client State)

**Best Practices**:
- Use for global client state only (cart, theme, user preferences)
- Keep store simple and focused
- Use middleware (persist) when needed

**Integration**:
- Cart state can be synced with Shopify cart
- Works alongside Server Components (only use in Client Components)

---

### 2.6 React Hook Form + Zod

**Best Practices**:
- Always use Zod schemas for validation
- Use `useForm` with proper typing
- Handle errors gracefully with proper UI feedback

**Integration**:
- Used in newsletter signup, contact forms, and future user features

---

### 2.7 Sanity.io (Headless CMS)

**Best Practices**:
- Define schemas clearly with TypeScript
- Use GROQ for powerful and efficient queries
- Leverage real-time preview during development
- Structure content for reusability (portable text, references)

**Integration**:
- Data fetched in Next.js Server Components
- Types generated for full type safety
- Works excellently with Next.js caching and revalidation

---

### 2.8 Shopify (Headless)

**Best Practices**:
- Use Shopify Storefront API (GraphQL)
- Handle cart and checkout through Shopify
- Keep product data in Shopify, enhance with Sanity if needed
- Use webhooks for order events (future)

**Integration**:
- Product data fetched in Server Components
- Cart managed via Shopify + Zustand for optimistic UI
- Checkout redirected to Shopify-hosted checkout

---

### 2.9 Vercel

**Best Practices**:
- Use Edge Runtime where beneficial
- Leverage ISR (Incremental Static Regeneration) for blog posts
- Set up proper environment variables and preview deployments
- Use Vercel Analytics + Speed Insights

**Integration**:
- Automatic deployments from GitHub
- Works perfectly with Next.js
- Supports Edge Functions for dynamic routes

---

### 2.10 Clean Architecture Pattern

**Best Practices**:
- Keep Domain logic independent
- Use Repository pattern for data access
- Separate Use Cases in Application layer
- Inject dependencies properly

**Integration**:
- All external services (Sanity, Shopify) are accessed through repository implementations
- Business logic stays clean and testable

---

## 3. Integration Architecture Overview

```
Next.js App (Vercel)
├── Server Components
│   ├── Fetch from Sanity (via GROQ)
│   ├── Fetch from Shopify (Storefront API)
│   └── Render pages with data
├── Client Components
│   ├── Framer Motion animations
│   ├── Zustand state (Cart, UI)
│   └── Interactive forms (React Hook Form + Zod)
└── Clean Architecture Layers
    ├── Domain (Entities + Interfaces)
    ├── Application (Use Cases)
    └── Infrastructure (Sanity & Shopify Repositories)
```

---

## 4. Recommended Tech Decisions Summary

| Decision                    | Recommendation              | Reason |
|----------------------------|-----------------------------|--------|
| Framework                  | Next.js 15 App Router       | Performance + DX |
| CMS                        | Sanity.io                   | Best DX + flexibility |
| E-commerce                 | Shopify Headless            | Reliable checkout + features |
| Animations                 | Framer Motion               | Powerful + React-friendly |
| State Management           | Zustand                     | Lightweight & simple |
| Forms                      | React Hook Form + Zod       | Best developer experience |
| Hosting                    | Vercel                      | Best Next.js hosting |
| Architecture               | Clean Architecture          | Long-term maintainability |

---

**This stack gives you a modern, performant, maintainable, and scalable foundation** while remaining practical for a solo founder.

Would you like me to create a **project initialization script** or **recommended folder structure** based on this stack next?