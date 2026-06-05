---
name: Professional
colors:
  surface: '#f7f9fb'
  surface-dim: '#d8dadc'
  surface-bright: '#f7f9fb'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f2f4f6'
  surface-container: '#eceef0'
  surface-container-high: '#e6e8ea'
  surface-container-highest: '#e0e3e5'
  on-surface: '#191c1e'
  on-surface-variant: '#45474c'
  inverse-surface: '#2d3133'
  inverse-on-surface: '#eff1f3'
  outline: '#75777d'
  outline-variant: '#c5c6cd'
  surface-tint: '#545f73'
  primary: '#091426'
  on-primary: '#ffffff'
  primary-container: '#1e293b'
  on-primary-container: '#8590a6'
  inverse-primary: '#bcc7de'
  secondary: '#505f76'
  on-secondary: '#ffffff'
  secondary-container: '#d0e1fb'
  on-secondary-container: '#54647a'
  tertiary: '#001815'
  on-tertiary: '#ffffff'
  tertiary-container: '#002f2a'
  on-tertiary-container: '#28a094'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#d8e3fb'
  primary-fixed-dim: '#bcc7de'
  on-primary-fixed: '#111c2d'
  on-primary-fixed-variant: '#3c475a'
  secondary-fixed: '#d3e4fe'
  secondary-fixed-dim: '#b7c8e1'
  on-secondary-fixed: '#0b1c30'
  on-secondary-fixed-variant: '#38485d'
  tertiary-fixed: '#89f5e7'
  tertiary-fixed-dim: '#6bd8cb'
  on-tertiary-fixed: '#00201d'
  on-tertiary-fixed-variant: '#005049'
  background: '#f7f9fb'
  on-background: '#191c1e'
  surface-variant: '#e0e3e5'
typography:
  display-lg:
    fontFamily: Hanken Grotesk
    fontSize: 32px
    fontWeight: '600'
    lineHeight: 40px
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Hanken Grotesk
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
  headline-sm:
    fontFamily: Hanken Grotesk
    fontSize: 20px
    fontWeight: '500'
    lineHeight: 28px
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  body-md:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
  body-sm:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 18px
  label-md:
    fontFamily: JetBrains Mono
    fontSize: 12px
    fontWeight: '500'
    lineHeight: 16px
    letterSpacing: 0.05em
  button-text:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '500'
    lineHeight: 20px
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  base: 4px
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 32px
  2xl: 48px
  gutter: 16px
  margin-mobile: 16px
  margin-desktop: 32px
---

## Brand & Style

The design system focuses on high-utility ergonomics environments. The brand personality is dependable, precise, and unobtrusive, designed specifically to support users through 8-hour workdays without visual fatigue. 

The style is **Corporate / Modern**, leaning heavily into high-legibility typography and soft, tactile elements. It avoids aggressive visual effects in favor of a "quiet" UI that prioritizes information density and clarity. The aesthetic emphasizes spaciousness, even in data-heavy views, using subtle depth and a refined color palette to guide the user's eye naturally toward primary actions and critical status updates.

## Colors

The palette is optimized for long-term screen exposure. The core of the system is built on a "white-dominant" surface strategy to ensure maximum clarity.

- **Primary:** A deep Slate Blue (#1E293B) used for high-level navigation and primary headers, providing a grounded, professional foundation.
- **Secondary:** A muted Steel Blue (#64748B) for secondary information, icons, and less-critical labels, reducing visual noise.
- **Tertiary:** A soft, professional Teal (#0D9488) reserved for success states, active indicators, or specific action highlights that need distinction without being neon.
- **Neutral:** A range of cool greys and off-whites used for borders, backgrounds, and disabled states.
- **Surface:** Strictly #FFFFFF for main content areas to maintain high contrast for text legibility while providing a clean, fresh workspace.

## Typography

This design system utilizes a tiered typography approach to manage complex information hierarchies. **Hanken Grotesk** is used for headlines to provide a modern, sharp edge to the interface. **Inter** is the workhorse for all body copy and input text, chosen for its exceptional legibility and neutral tone. For technical data, order IDs, and SKU numbers, **JetBrains Mono** provides the necessary tabular spacing to prevent errors during rapid scanning.

Line heights are intentionally generous to improve reading speed and reduce eye strain. Text colors should never be pure black; instead, use the Primary or Secondary slate tones to maintain a "soft contrast" feel against the white surface.

## Layout & Spacing

The layout follows a **Fixed Grid** philosophy for desktop dashboards to ensure that data visualizations and tables remain in predictable locations. 

- **Desktop (1440px+):** 12-column grid, 32px side margins, 16px gutters. 
- **Tablet (768px - 1439px):** 8-column grid, 24px side margins, 16px gutters.
- **Mobile (Up to 767px):** 4-column fluid grid, 16px side margins.

A strict 4px baseline grid governs all internal component spacing (padding/margins). Vertical rhythm is maintained by using 16px (md) as the default gap between logical sections.

## Elevation & Depth

To maintain the "easy on the eyes" requirement, the design system avoids heavy shadows. Instead, it utilizes **Tonal Layers** and **Low-contrast outlines**.

- **Level 0 (Base):** White (#FFFFFF) surface.
- **Level 1 (Cards/Sidebar):** Neutral-50 (#F8FAFC) or White with a 1px border in Neutral-200.
- **Level 2 (Popovers/Dropdowns):** White surface with a very soft, highly diffused shadow (0px 4px 20px rgba(0,0,0,0.05)) and a light border.

This approach creates a sense of "stacking" without the visual weight of traditional skeuomorphism, keeping the interface feeling light and airy.

## Shapes

The shape language is defined by a "Soft Modern" approach. By moving away from sharp or minimally rounded corners, the interface feels more approachable and less "industrial." 

Standard components like buttons, input fields, and tags utilize a **0.5rem (8px)** radius as the default. Larger containers like cards and modals utilize a **1rem (16px)** radius. This consistency in rounding helps soften the structured nature, making the software feel more like a modern tool and less like a legacy spreadsheet.

## Components

### Buttons
Primary buttons use the Slate Blue (#1E293B) background with white text. Secondary buttons use a transparent background with a 1px border in Neutral-300. All buttons must have a height of at least 40px for ergonomic clicking, with an 8px (0.5rem) corner radius.

### Input Fields
Inputs feature a 1px border in Neutral-300 and a 0.5rem corner radius. Focus states are indicated by a 2px Teal (#0D9488) ring with a soft outer glow. The background of inputs remains white to stay consistent with the surface strategy.

### Cards
Cards are the primary container for grouping order details. They should use a 1px Neutral-200 border rather than a shadow. The header of the card should be subtly separated by a horizontal rule or a slightly darker background tint (#F8FAFC).

### Status Chips
Status indicators (e.g., "Shipped", "Pending") use a soft, de-saturated background of the status color with a darker text version of the same hue to ensure legibility (e.g., Soft Teal bg with Deep Teal text). This avoids the "neon" effect and keeps the UI professional.

### Data Tables
Tables are the heart of the system. Use "Zebra striping" with Neutral-50 on alternate rows to help the eye track across long data lines. Row height should be set to 48px to provide sufficient "breathing room" for the Inter body text.
