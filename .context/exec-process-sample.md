### Process a Sample UI Screenshot (Mantine + React)

Use this guide to convert a screenshot into a responsive Mantine layout with well-scoped components and tasteful micro-interactions.

---

#### 1) Create output directory and file

- Ensure a `processed-samples/` folder exists

```bash
mkdir -p processed-samples
```

- Create a markdown file named using the screenshot base name and current date:
  - Format: `<screenshot-name>-<YYYY-MM-DD>.md`
  - Example: for `landing-hero.png` on 2025-08-21 → `processed-samples/landing-hero-2025-08-21.md`

```bash
DATE=$(date +%F)
BASENAME="landing-hero" # replace with your screenshot base name (no extension)
touch "processed-samples/${BASENAME}-${DATE}.md"
```

---

#### 2) Paste this template into the new file

Copy/paste and fill in each section. Keep instructions focused and actionable for Cursor.

```md
### Processed UI: <Title>

#### Screenshot

- File: <link or path to screenshot>
- Date processed: <YYYY-MM-DD>
- Notes: <short context>

#### Layout outline (boxes → regions)

- Header: <present? contents?>
- Primary navigation: <present? placement?>
- Hero/lead section: <copy, media, CTA>
- Content sections (in order): <cards, lists, forms, features, etc.>
- Footer: <links, legal>

#### UI element inventory (exhaustive, for pixel-faithful recreation)

- Global/background: gradients, patterns, decorative shapes, dividers
- Header/nav: logo (type/mark), nav links, CTA(s), badges/labels, utilities (search, theme, auth)
- Hero: headline, subhead, CTAs, secondary links, social proof, media (image/video), highlights/badges
- Sections (per section, list counts): cards, icons, headings, body text, buttons/links, tags/chips, inputs, tables, lists
- Forms: inputs, selects, textareas, radios/checkboxes, helper/error text, primary/secondary submit actions
- Illustrations/assets: icons (names), logos, screenshots (filenames/paths), avatars
- Footer: columns, headings, link counts, socials, legal text
- Visual tokens used in the screenshot (approximate values are fine):
  - Colors (hex/rgb), radii (px), shadows (token names), borders (width/color), spacing gaps (xs–xl), container max-widths, typography sizes/weights/line-heights

Represent the inventory as a checklist with counts, e.g.:

- [ ] Buttons: 3 primary, 1 outline
- [ ] Cards: 4 feature cards
- [ ] Icons: car, home, heart, shield
- [ ] Inputs: 3 (first, last, email)
- [ ] Logos: 6 integration marks

#### Mantine layout plan (high level)

- App shell: AppShell or Container?
- Layout primitives priority: Use `Flex`/`Stack`/`Group` to place elements exactly like the screenshot. Reserve `Grid` for multi-column, responsive sections only.
- Spacing rhythm: Mantine `gap`/`spacing` scale (`xs`–`xl`) on `Flex`, `Stack`, `Group`.
- Max widths: use `Container size="lg|xl"` or custom.
- Prefer component `gap`, `justify`, `align`, `wrap` over ad‑hoc margins; keep CSS minimal.

#### Components to build

- List and describe reusable components with props & their style for css:
  - <ComponentName> (location: `src/components/<ComponentName>/index.tsx`)
    - Purpose: <brief>
    - Key props: <list>
    - Variants: <if any>
- Map where each component is used in the layout.

#### Responsiveness strategy

- Breakpoints: xs, sm, md, lg, xl – how the layout changes at each.
- Grid spans: define spans per breakpoint for key sections.
- Mobile nav: collapse/Drawer behavior (if applicable).
- Typography and spacing adjustments for mobile.

#### Subtle animations and interactions (keep it light)

- Hover states: elevate cards slightly (`shadow`, `transform: translateY(-2px)`) and soften with `transition`.
- Entrance transitions: fade/slide small elements with Mantine `Transition`.
- Button feedback: minimal scale or background tint on hover/active.
- Respect reduced motion: avoid large animations; keep durations ~120–200ms, easing `ease-out`.
- Add smooth animations and micro interactions like:
  - smooth hover effects
  - gentle tilt effects
  - scroll-based animations
  - animated glitch-style
  - inertia-based scroll

#### Implementation tasks for Cursor

1. Create/confirm layout container(s) using Mantine (`AppShell` or `Container`). Place elements with `Flex`/`Stack`/`Group` first; introduce `Grid` only for multi-column responsive sections.
2. Implement components listed above under `src/components/*` with typed props and story-like usage in the page.
3. Compose the page in `src/pages/<PageName>.tsx` using the components.
4. Ensure responsive behavior using Mantine `Grid` spans and responsive props. Verify on xs/sm/md.
5. Add tasteful transitions via `Transition` and CSS for hover/focus states; keep subtle.
6. Run lint/build.

#### Acceptance checklist

- [ ] Visual structure matches screenshot at common breakpoints (xs/md/lg)
- [ ] Components are reusable and colocated in `src/components`
- [ ] No layout overflow on small screens
- [ ] Animations are subtle and respect reduced motion
- [ ] Lint/build pass
```

---

#### 2.1) Add a JSON Design Profile section (embed in this same .md)

Do not create a separate `.json` file. In this markdown file, add a heading `JSON Design Profile` and include a fenced `json` block. Create a JSON profile design system that extracts visual data from these
screenshots so that I can use the JSON output in Cursor to give it context on how to
replicate such design systems in a consistent style. Avoid including the contents of
the specific images. The output should include the design style, the structure and
anything that'll help an AI replicate such designs. Make the design appealing with subtle UX such as transitions and hover interactions.

---

#### 2.2) Add a Theme Component section (embedded JSON)

In this same `.md`, add a `Theme Component` section that re-embeds the JSON profile from 2.1 (so Cursor can ingest a single artifact). Use this template:

````md
### Theme Component: <Title>

Description: Generic theme definition for <screen category>. No brand text included.
Date: <YYYY-MM-DD>

```json
{
  /* paste the JSON Design Profile from section 2.1 here */
}
```
````

Guidance:

- Keep the JSON identical to the profile in 2.1 (no differences).
- The output is ONE markdown file containing both the narrative and the JSON blocks.
- Cursor can now read this single file to derive a Mantine theme consistently.

---

#### 3) Quick Mantine patterns (reference)

- Flex first: horizontally align/space elements

```tsx
import { Flex, Button, Title, Text } from "@mantine/core";

export function HeroRow() {
  return (
    <Flex
      align="center"
      justify="space-between"
      gap="lg"
      wrap={{ base: "wrap", md: "nowrap" }}
    >
      <Flex direction="column" gap="sm" style={{ flex: 1 }}>
        <Title order={1}>Headline</Title>
        <Text c="dimmed">Subheadline that wraps nicely on small screens.</Text>
        <Flex gap="sm">
          <Button>Primary</Button>
          <Button variant="light">Secondary</Button>
        </Flex>
      </Flex>
      <div style={{ flex: 1 }}>{/* media / image */}</div>
    </Flex>
  );
}
```

- Stack for vertical rhythm (mobile-first)

```tsx
import { Stack, Title, Text } from "@mantine/core";

export function SectionStack() {
  return (
    <Stack gap="md">
      <Title order={2}>Section title</Title>
      <Text c="dimmed">Body copy goes here</Text>
      {/* children */}
    </Stack>
  );
}
```

- Group for compact inline clusters

```tsx
import { Group, Button } from "@mantine/core";

export function Actions() {
  return (
    <Group gap="sm">
      <Button>Save</Button>
      <Button variant="light">Cancel</Button>
    </Group>
  );
}
```

- Grid example with responsive spans (use when true columns are required):

```tsx
import { Container, Grid } from "@mantine/core";

export function ExampleLayout() {
  return (
    <Container size="lg">
      <Grid gutter="lg">
        <Grid.Col span={{ base: 12, sm: 6 }}>{/* Left content */}</Grid.Col>
        <Grid.Col span={{ base: 12, sm: 6 }}>{/* Right content */}</Grid.Col>
      </Grid>
    </Container>
  );
}
```

- Subtle transition example:

```tsx
import { Card, Transition } from "@mantine/core";
import { useState } from "react";

export function FadingCard({ children }: { children: React.ReactNode }) {
  const [mounted, setMounted] = useState(true);
  return (
    <Transition
      mounted={mounted}
      transition="fade"
      duration={150}
      timingFunction="ease-out"
    >
      {(styles) => (
        <Card style={styles} withBorder shadow="sm" radius="md">
          {children}
        </Card>
      )}
    </Transition>
  );
}
```

- Tasteful hover affordance (SCSS/CSS-in-JS sketch):

```scss
.cardHover {
  transition: transform 160ms ease-out, box-shadow 160ms ease-out;
}
.cardHover:hover {
  transform: translateY(-2px);
  box-shadow: var(--mantine-shadow-md);
}
```

---

#### 4) Mobile responsiveness guidance & overall style

- remember we can use incline css & class modules to fine tune the design
- Prefer `Flex` with `wrap` and `gap` for most horizontal layouts; switch to `direction="column"` on small screens. Use `Grid.Col span={{ base: 12, sm: 6, md: 4 }}` only when true columns are needed.
- Replace side-by-side elements with stacked `Stack` on small screens; keep CTAs in a `Group` for consistent spacing.
- Keep images fluid (`w: 100%`, constrained with `maxWidth`).
- Reduce paddings/margins at `base`/`sm` to avoid horizontal scroll.
- Verify tap targets are ≥ 44px height.

---

#### 5) What to avoid

- Overly heavy animations or long durations.
- Complex parallax or physics effects.
- Hard-coded pixel dimensions; prefer Mantine spacing and responsive props.
- Overusing `Grid` for simple alignment; reach for `Flex`/`Stack`/`Group` first to match screenshots accurately with fewer wrappers.
