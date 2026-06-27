# PongKdor.com - Complete Enterprise System Design Document
**Version**: 2.0 (Final Master Document)  
**Date**: June 22, 2026  
**Status**: Validated & Production-Ready

---

## Part 1: Project Overview & Vision

**Project Name**: PongKdor.com  
**Tagline**: "Pong Kdor Energy – Real Talk, Real Laughs, Real Cambodian Bros"

A meme-driven men’s lifestyle platform combining humorous content and merchandise e-commerce, built with enterprise-grade practices while remaining realistic for a solo founder.

---

## Part 2: System Requirements Specification (SRS)

### Functional Requirements (MVP)
- Homepage with hero, featured content, and shop teaser
- Blog system (listing + detail pages)
- Merchandise store with cart and checkout (via Shopify)
- Age gate (18+)
- Basic newsletter signup

### Non-Functional Requirements
- Mobile-first performance (< 3s load on 4G)
- Strong security and compliance with Cambodian content laws
- Scalable architecture
- Maintainable and well-documented code

---

## Part 3: System Requirements to Boot

### Development Environment
- Node.js 20+
- pnpm or npm
- VS Code + Git
- Figma
- Accounts: Vercel, Shopify, Sanity.io, GitHub

### Production Environment
- Vercel (Frontend)
- Shopify (E-commerce)
- Sanity.io (CMS)
- Automatic SSL + CDN

---

## Part 4: Frontend System Design

**Stack**: Next.js 15 (App Router) + TypeScript + Tailwind + Framer Motion + shadcn/ui

**Key Decisions**:
- Server Components by default for performance and SEO
- Zustand for client state
- Framer Motion for complex fluid animations
- Clean folder structure with clear separation

---

## Part 5: UI/UX Concept Design

**Design Philosophy**: Bold, humorous, meme-first, mobile-first, with powerful reactive and fluid animations.

**Creative Animation Goals**:
- Scroll-linked animations
- Physics-based interactions (spring, drag)
- Micro-interactions on every clickable element
- Playful meme-style reveals and transitions

---

## Part 6: Frontend Compliance Document

**Strict Rules**:
- Only cartoon, illustration, and text-based memes allowed
- Mandatory 18+ age gate
- Clear disclaimers on all pages
- No real explicit content under any circumstances

---

## Part 7: Backend System Design (Clean Architecture - Enterprise Grade)

### 7.1 Architecture Overview

We follow **Clean Architecture** principles with clear separation of concerns. Even though we use managed services (Shopify + Sanity), we structure the code properly for long-term scalability and maintainability.

**Layers** (from outside to inside):

1. **Presentation Layer** (Next.js App Router / Server Components)
2. **Application Layer** (Use Cases / Services)
3. **Domain Layer** (Core business logic & entities)
4. **Infrastructure Layer** (External services: Shopify, Sanity, Database)

### 7.2 Recommended Backend Structure

```
src/
├── app/                    # Presentation (Next.js)
├── application/            # Use cases & business logic
│   ├── blog/
│   ├── shop/
│   └── user/
├── domain/                 # Core entities & interfaces
│   ├── entities/
│   └── repositories/
├── infrastructure/         # External integrations
│   ├── shopify/
│   ├── sanity/
│   └── database/
├── lib/
│   └── utils.ts
└── types/
```

### 7.3 Technology Choices (Backend)

| Layer                  | Technology                  | Reason |
|------------------------|-----------------------------|--------|
| API / Server           | Next.js API Routes          | Simple, fast, no extra server needed |
| Complex Backend Logic  | NestJS (optional later)     | Only if custom backend grows significantly |
| Database               | Supabase / Neon (PostgreSQL)| Only if we need custom user data |
| Caching                | Upstash Redis or Vercel KV  | For performance |
| Authentication         | NextAuth.js or Clerk        | When user accounts are added |
| Payments               | Shopify Payments            | Handled by Shopify |
| File Storage           | Sanity Assets + Vercel Blob | Simple and scalable |

### 7.4 Clean Architecture Principles Applied

- **Dependency Rule**: Inner layers do not depend on outer layers
- **Dependency Inversion**: Use interfaces/repositories
- **Single Responsibility**: Each module has one clear job
- **Separation of Concerns**: Business logic is independent of frameworks

### 7.5 Scalability Strategy

**Short Term (MVP)**:
- Use Shopify + Sanity heavily (managed services)
- Keep custom backend minimal

**Medium Term (6-12 months)**:
- Add custom PostgreSQL database when needed
- Introduce caching layer
- Add background jobs if required

**Long Term (Enterprise Level)**:
- Consider moving to microservices or modular monolith
- Add message queue (if high traffic)
- Multi-region deployment if needed

### 7.6 Security & Compliance (Backend)

- Never store sensitive payment data
- Proper input validation and sanitization
- Environment variables for all secrets
- Rate limiting on public APIs
- Regular dependency updates
- Logging and monitoring (Sentry)

### 7.7 Performance Optimization

- Heavy use of Server Components and caching
- Edge functions where possible
- Image optimization at build + runtime
- Database query optimization (when custom DB is added)

---

## Part 8: Overall Project Compliance

- Strict content rules (cartoon only)
- Age verification
- Legal disclaimers
- Performance targets
- Security best practices
- Clean, maintainable, and scalable code

---

## Part 9: Implementation Roadmap (Summary)

**Phase 0** (Week 1): Branding + Requirements finalization  
**Phase 1** (Week 2-5): MVP Development (Next.js + Shopify + Sanity)  
**Phase 2** (Week 6): Soft Launch  
**Phase 3** (Month 2-4): Content growth + first sales  
**Phase 4** (Month 5-12): Scaling features and traffic

---

## Part 10: Final Validation

This document represents a **realistic, clean, scalable, and enterprise-grade** approach while remaining practical for a solo founder starting in Cambodia.

No over-engineering. No fluff. Production-ready thinking from day one.

---

**End of Complete Enterprise System Design Document**

**Status**: Ready to start development.