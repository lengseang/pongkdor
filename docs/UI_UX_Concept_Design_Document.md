# PongKdor.com - UI/UX Concept Design Document
**Version**: 1.0  
**Date**: June 22, 2026

---

## 1. Design Philosophy

**Core Vibe**: Bold, unapologetic, humorous, chaotic-good energy.  
Think “streetwear brand meets meme culture” with strong Cambodian flavor.

**Key Design Principles**:
- Humor first
- Fluid, reactive, and playful interactions
- Mobile-first (most users on phones)
- High visual impact with fast performance

---

## 2. Visual Identity

**Color Palette**:
- Primary: Deep Black (#0A0A0A)
- Accent 1: Vibrant Red (#FF2D55)
- Accent 2: Gold (#FFD700)
- Text: White + Light Gray
- Background accents: Subtle dark gradients

**Typography**:
- Headings: Bold, condensed, impactful (Inter Black or similar)
- Body: Clean sans-serif with good readability
- Meme text: Playful, slightly distorted fonts where appropriate

---

## 3. UI/UX Concept & Interaction Design

### 3.1 Homepage Experience
- Large hero with animated tagline that reacts on scroll
- “Meme of the Day” section with hover reveal animation
- Horizontal scroll for featured merch with momentum scrolling
- Sticky mini cart that expands with spring animation

### 3.2 Blog Experience
- Masonry or grid layout with staggered entrance animations
- Article cards that lift and scale slightly on hover
- Reading progress bar with smooth animation
- Related content that slides in creatively

### 3.3 Shop Experience
- Product grid with filter chips that animate when selected
- Product detail page with image gallery that uses drag/swipe + zoom
- Size selector with satisfying click feedback and micro bounce
- Add to cart button with loading + success fluid animation

### 3.4 Advanced Reactive & Fluid Animations (Creative)

**Complex Interactions**:
- **Scroll-linked animations**: Elements react to scroll position (parallax-lite, scale, opacity)
- **Gesture-based interactions**: Drag to dismiss cart, pull to refresh on mobile
- **Physics-based animations**: Cart items with spring physics when added
- **Sequence animations**: Multi-step reveals when navigating between pages
- **Meme interaction**: Click on meme to trigger funny animation or sound (optional)
- **Hover depth**: 3D tilt effect on product cards using Framer Motion

**Creative Touches**:
- Background elements that subtly react to mouse movement
- Text that distorts playfully on hover (meme style)
- Loading states that feel fun instead of boring
- Success states with celebratory micro-animations

---

## 4. Design System Components

- Buttons with multiple states + animation variants
- Cards with elevation and hover transforms
- Modals with backdrop blur + scale entrance
- Tabs and filters with smooth transitions
- Toast notifications with slide + bounce

---

## 5. Validation

This UI/UX direction is **highly engaging** while remaining performant when implemented correctly with Framer Motion + Next.js.

The combination of **reactive animations + strong meme personality** will make PongKdor.com feel unique and memorable.

**End of UI/UX Concept Design Document**