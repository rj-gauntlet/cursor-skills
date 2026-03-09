---
name: stunner
description: Transform a functionally complete project into a visually stunning application. Creates HTML/CSS prototype mockups for approval before writing any code. Works with any frontend framework. Use when the user wants to improve the UI, make the app look better, redesign the interface, add visual polish, create a design system, or make the UI a show-stopper.
---

# Stunner — Make It Beautiful

Take a functionally complete project and transform its UI into something visually stunning through a mockup-driven, iterative process.

## Philosophy

Show, don't tell. The user sees visual mockups before any code changes. Iterate on the visuals until they're excited. Only then implement.

## Mockup Method

All mockups are built as **standalone HTML/CSS prototypes** — not generated images. Each mockup is a single HTML file with inline CSS, opened in the browser and screenshotted for the user.

Why this approach:
- **Fast** — generating HTML/CSS is near-instant vs. slow image generation
- **Pixel-perfect** — real fonts, real spacing, real colors, crisp text
- **Iterable** — tweak a color or font size and re-screenshot, no full regeneration
- **Reusable** — the prototype code informs the final implementation directly

Save mockup files to a `stunner-mockups/` directory in the project. Name them descriptively (e.g., `direction-bold-vibrant.html`, `page-dashboard.html`).

## Trigger

The user has a working project and wants to improve the visual design. The app should be functionally sound — stunner focuses on aesthetics, not features.

## Workflow

### Phase 1: Audit

1. **Detect the framework** — scan for React, Vue, Svelte, Next, Nuxt, Angular, Astro, or plain HTML/CSS. Read config files and dependencies to understand the styling approach (Tailwind, CSS Modules, styled-components, plain CSS, etc.).
2. **Map the UI surface** — identify all pages, layouts, and key components. Build a list:
   ```
   Pages: Home, Dashboard, Settings, Profile, Login
   Shared: Navbar, Sidebar, Footer, Card, Button, Modal, Form
   ```
3. **Screenshot the current state** — use browser automation to capture every page as-is. These are the "before" shots.
4. **Identify the app's personality** — what kind of product is this? SaaS dashboard? Consumer app? Developer tool? Portfolio? E-commerce? This informs aesthetic direction.

Present the audit summary and page list to the user. Confirm which pages/components are in scope.

### Phase 2: Aesthetic Direction

Present **5-6 visual directions** as HTML/CSS prototype mockups of the app's most important screen.

Always include these core directions:

| Direction | Vibe |
|-----------|------|
| **Bold & Vibrant** (default) | Rich color palette, strong contrast, dynamic layouts, confident typography, energetic feel |
| **Modern Minimal** | Generous whitespace, sharp typography, monochromatic with one accent, subtle shadows |
| **Dark & Premium** | Dark backgrounds, glowing accents, glassmorphism, sleek and techy |
| **Warm & Organic** | Rounded shapes, earthy tones, soft gradients, friendly and approachable |

Plus **2-3 out-of-the-box ideas** tailored to the specific product. These should be unexpected, creative directions that push beyond safe choices. Examples:

- **Retro terminal** — monospace type, green-on-black accents, CRT scan lines, nostalgic hacker aesthetic
- **Editorial/magazine** — dramatic typography, asymmetric layouts, bold imagery, print-inspired
- **Neon brutalist** — raw, intentionally rough, oversized type, neon accents, anti-design that still works
- **Illustrated/playful** — hand-drawn elements, quirky illustrations, bouncy animations, whimsical
- **Luxury/fashion** — extreme whitespace, thin serif fonts, muted palette, photography-forward

Choose the out-of-the-box options based on what would actually work for the product type. A developer tool might get "retro terminal." A consumer app might get "illustrated/playful." Don't just pick randomly.

**For each direction:**
1. Create a standalone HTML file with inline CSS that renders the app's main screen in that aesthetic
2. Use Google Fonts (via CDN link) for typography. Use real CSS for shadows, gradients, border radius, spacing.
3. Include realistic content — not lorem ipsum
4. Open each in the browser using browser automation and screenshot it
5. Save the HTML file to `stunner-mockups/direction-[name].html`

**Present all directions and ask the user to react:**
- Pick one
- Combine elements ("A's colors with C's layout")
- Ask for variations ("like B but warmer")
- Reject all and describe what they want instead

**Iterate until the user is satisfied with the direction.** No limit on rounds. Modify the HTML/CSS prototype and re-screenshot — this is fast since it's just code changes, not image regeneration.

### Phase 3: Brand Vibe

Once the aesthetic direction is locked:

1. **Suggest a brand personality** — 3-5 adjectives that capture the vibe (e.g., "confident, modern, energetic, approachable")
2. **App name typography** — suggest how the app name should be styled (font, weight, treatment)
3. **Favicon concept** — describe a simple favicon that fits the aesthetic, or create one as an SVG
4. **Color naming** — give the palette colors meaningful names that reinforce the brand (not "blue-500" but "electric," "midnight," "ember")
5. **Voice alignment** — suggest whether UI copy should be formal, casual, playful, technical, etc.

Present this as a brand vibe card. The user approves or adjusts.

### Phase 4: Design System

Codify the approved direction into a concrete design system:

**Colors:**
- Primary, secondary, accent palette with hex values
- Semantic colors (success, warning, error, info)
- Neutral scale (backgrounds, borders, text)
- Dark mode variants (not just inverted — properly adjusted)

**Typography:**
- Font pairing (display/heading font + body font)
- Type scale (hero → h1 → h2 → h3 → body → small → caption) with specific sizes, weights, and line heights
- Letter spacing adjustments per level

**Spacing:**
- Spacing scale (e.g., 4, 8, 12, 16, 24, 32, 48, 64, 96, 128)
- Component padding conventions
- Section spacing conventions

**Shadows:**
- Shadow scale (subtle → medium → elevated → floating)
- Shadow color (tinted to match the palette, not plain gray)

**Borders:**
- Border radius scale (none → sm → md → lg → full)
- Border width and color conventions

**Animations:**
- Timing curves (ease-out for entrances, ease-in-out for transitions)
- Duration scale (fast: 150ms, normal: 300ms, slow: 500ms)
- Standard animations: fade-in, slide-up, scale-in, hover-lift

**Component states:**
Define the visual treatment for every interactive state:
- Default → Hover → Active/Pressed → Focus → Disabled → Loading

Save this as a design system file in the project (e.g., `DESIGN_SYSTEM.md` or framework-appropriate tokens file like `theme.ts`, `tailwind.config` extensions, CSS custom properties, etc.).

Present the design system to the user for approval.

### Phase 5: Page Mockups

For **every page** in the scope list from Phase 1:

1. **Create an HTML/CSS prototype** of the page fully redesigned using the approved design system. Save to `stunner-mockups/page-[name].html`.
2. **Open in browser and screenshot** — present to the user
3. **Iterate** based on feedback — "make the hero bigger," "I don't like the sidebar here," "can we try a different layout for the cards." Edits are fast — modify the HTML/CSS and re-screenshot.
4. **No limit on rounds** — keep iterating until the user is satisfied with that page

Move to the next page only when the current one is approved.

**Prototype quality guidelines:**
- Use realistic content, not placeholders
- Include the navigation/chrome so it feels like a real app
- Load the design system's fonts via Google Fonts CDN
- Match the design system's exact color values, spacing scale, and shadow system
- Show the page at desktop resolution (1440px) primarily
- If the user asks, resize the browser viewport and screenshot at mobile/tablet widths too

**Shared stylesheet:** After the design system is approved in Phase 4, create a `stunner-mockups/shared-styles.css` with all the design tokens as CSS custom properties. Each page prototype links to this file, ensuring consistency and making iteration even faster (change a token once, all pages update).

After all pages are individually approved, present the **full set side by side** as a final consistency check. The user should feel like they're looking at a cohesive product, not a collection of unrelated pages.

### Phase 6: Implement

Now write the code. For each page:

1. **Update the design system tokens** in code if not already done (first page only)
2. **Restyle the page** to match the approved mockup
3. **Add component states** — hover, focus, active, disabled, loading, empty, error
4. **Add animations** — entrance animations, hover effects, transitions between states
5. **Open in browser** — use browser automation to view the live result
6. **Screenshot and compare** to the approved mockup prototype
7. **Adjust** until the live version matches the mockup
8. **Present to the user** — show the before (from Phase 1) and after side by side

Move to the next page only when the user approves the implementation.

### Phase 7: Polish Pass

After all pages are implemented:

1. **Responsive refinement** — test at mobile (375px), tablet (768px), and desktop (1440px). Adjust layouts, font sizes, and spacing per breakpoint. This is not just "stack on mobile" — each breakpoint should feel intentionally designed.
2. **Micro-interactions** — add finishing touches:
   - Button press feedback (scale/color shift)
   - Form field focus animations
   - Success/error state animations
   - Scroll-triggered reveals for content sections
   - Page transition effects (if applicable)
   - Loading skeletons instead of spinners
3. **Dark mode** (if included in design system) — implement with proper palette adjustments, not just color inversion. Test every page in both modes.
4. **Consistency sweep** — walk through every page in the browser to check:
   - Same spacing patterns used consistently
   - Typography hierarchy is consistent
   - Colors match the design system
   - All interactive elements have all states
   - Animations feel cohesive (same timing, same style)
5. **Final screenshots** — capture every page in the finished state. These are the "after" shots.
6. **Present before/after** — show Phase 1 screenshots alongside Phase 7 screenshots for every page.

### Phase 8: Handoff

1. **Save the design system document** to the project
2. **Create a `UI_CHANGELOG.md`** summarizing:
   - Aesthetic direction chosen
   - Design system overview
   - Pages restyled
   - Key visual decisions and rationale
3. **Commit** with message: `style: transform UI with stunner design system`

## Interaction Guidelines

- **Visuals first, code second.** Never restyle code without an approved mockup. The user should always see what they're getting before it's built.
- **Iterate without frustration.** The user said they don't know what they like until they see it. Be patient with iteration. Each round of feedback gets closer to their vision.
- **Be opinionated but flexible.** Start with a strong aesthetic recommendation. If the user pushes back, adapt without resistance.
- **Details are the difference.** The gap between "nice" and "show-stopper" is hover states, shadow quality, animation timing, and spacing rhythm. Don't skip these.
- **Real content matters.** Mockups with lorem ipsum feel fake. Use realistic text, plausible names, actual-looking data. The design should feel like a product.
- **Respect the framework.** Implement using whatever styling approach the project already uses. Don't introduce Tailwind into a styled-components project.
- **Before and after.** Always show the transformation. The contrast is what makes the user feel the improvement.
