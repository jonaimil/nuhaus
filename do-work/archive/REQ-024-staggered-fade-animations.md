---
id: REQ-024
title: Staggered fade animations for page transitions
status: done
created_at: 2025-02-04T14:50:00Z
---

# Staggered fade animations for page transitions

## What
Add smooth GSAP-powered fade in/out animations to Concept and Team page content. Headings and paragraphs should fade in with staggered timing rather than appearing abruptly.

## Changes
- Concept sections: h2 fades in first, p follows 150ms later
- Between sections: outgoing content fades up/out, incoming fades down/in
- Team page: header fades in, then member cards stagger in (100ms apart)
- All transitions use GSAP with power2 easing

## Also
- Reverted Connect button to match nav styling (undoes REQ-023 CTA treatment)

---
*Source: "when it goes from the modeler page to concept page, I want it to fade in a little bit more"*
