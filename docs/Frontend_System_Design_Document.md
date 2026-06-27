# PongKdor.com - Frontend System Design Document
**Version**: 1.0  
**Date**: June 22, 2026

---

## 1. Overview

This document describes the frontend architecture, component structure, state management, and technical decisions for the PongKdor.com web application.

**Goal**: Build a fast, reactive, SEO-friendly, and highly animated frontend that delivers a bold meme lifestyle experience.

---

## 2. Technology Stack

- **Framework**: Next.js 15 (App Router) with React 19
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS 4 + CSS Modules / Tailwind variants
- **UI Components**: shadcn/ui + custom components
- **Animations**: Framer Motion (complex fluid animations)
- **State Management**: 
  - Server State: React Server Components + TanStack Query (if needed)
  - Client State: Zustand (lightweight)
- **Forms**: React Hook Form + Zod
- **Data Fetching**: Server Components + Shopify + Sanity GraphQL

---

## 3. Project Structure (Recommended)

```
src/
├── app/                    # Next.js App Router
│   ├── (marketing)/
│   │   ├── page.tsx        # Homepage
│   │   ├── blog/
│   │   └── about/
│   ├── shop/
│   ├── layout.tsx
│   └── globals.css
├── components/
│   ├── ui/                 # Reusable UI components
│   ├── sections/           # Page sections
│   ├── blog/
│   └── shop/
├── lib/
│   ├── sanity.ts
│   ├── shopify.ts
│   └── utils.ts
├── hooks/
├── types/
├── stores/                 # Zustand stores
└── styles/
```

---

## 4. Key Frontend Features

### 4.1 Reactive & Fluid Animations
- Smooth page transitions using Framer Motion
- Scroll-triggered animations
- Hover micro-interactions on cards and buttons
- Meme reveal animations
- Cart drawer with spring physics

### 4.2 Performance Optimizations
- Server Components by default
- Image optimization (Next.js Image + WebP)
- Lazy loading of heavy sections
- Code splitting

### 4.3 SEO & Accessibility
- Proper semantic HTML
- Meta tags and Open Graph
- ARIA labels where needed
- Fast loading for mobile users

---

## 5. Component Architecture

**Core Reusable Components**:
- `MemeCard`
- `ArticleCard`
- `ProductCard`
- `AnimatedButton`
- `AgeGateModal`
- `CartDrawer`
- `Navbar` (with mobile menu)

**Page-Level Components**:
- Homepage sections
- Blog listing + filters
- Product detail with variants

---

## 6. Validation & Recommendations

- Using **Next.js App Router** is the best choice in 2026 for performance and SEO.
- **Framer Motion** gives powerful yet developer-friendly fluid animations.
- Keeping state management light with **Zustand** is ideal for this project size.

**End of Frontend System Design Document**