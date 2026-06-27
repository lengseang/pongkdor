# PongKdor.com - Frontend Compliance Document
**Version**: 1.0  
**Date**: June 22, 2026

---

## 1. Purpose

This document outlines the compliance, legal, accessibility, and safety requirements that must be followed in the frontend development of PongKdor.com.

---

## 2. Content Compliance Rules

### 2.1 Visual Content Rules (Critical)
- **Only allowed**: Cartoons, illustrations, text-based memes, edited photos (non-explicit)
- **Strictly prohibited**:
  - Real photographs of private parts
  - Explicit sexual imagery
  - Nudity (even artistic)
  - Any content that could be classified as pornography under Cambodian law

### 2.2 Tone & Language
- Humor must be framed as satire and entertainment
- Self-deprecating and playful tone preferred over aggressive
- Clear disclaimers on all pages: “This is humor and satire only”

---

## 3. Legal & Safety Requirements

### 3.1 Age Verification
- Implement a mandatory **18+ Age Gate** modal on first visit
- Store user confirmation in localStorage (30-day expiry)
- Block access to content until confirmed

### 3.2 Disclaimers & Legal Pages
Must include visible links to:
- Terms of Service
- Privacy Policy
- Disclaimer (“Humor & Satire Only”)
- Age Restriction Notice

### 3.3 User-Generated Content (Future)
- All user submissions must be moderated before publishing
- Clear reporting system for inappropriate content

---

## 4. Accessibility (a11y) Requirements

- Follow WCAG 2.2 Level AA guidelines where practical
- Proper semantic HTML
- Keyboard navigation support
- Sufficient color contrast
- Alt text on all meaningful images
- Focus indicators on interactive elements

---

## 5. Performance & Technical Compliance

- Mobile-first responsive design
- Fast loading (< 3 seconds on mobile)
- Proper meta tags and Open Graph for sharing
- No tracking of sensitive personal data without consent

---

## 6. Validation

All frontend development must strictly follow the visual content rules to minimize legal risk in Cambodia.

**Recommendation**: Add a visible footer note on every page:  
“PongKdor.com contains humorous and satirical content intended for adults 18+.”

---

**End of Frontend Compliance Document**