# ui-kit-shadcn

Portable dark-dashboard design system: theme tokens (colors, type scale, spacing, radius) plus
a set of shadcn-style React components (`Button`, `Card`, `Table`, `Dialog`, `Tabs`, `Sidebar`,
etc.), built on `@base-ui/react` and Tailwind CSS 4.

This is meant to be **installed as a dependency in each project, not copy-pasted**. That's the
whole point: code that lives in `node_modules` is naturally hands-off, because the next
`npm install` would wipe local edits anyway. Want a change? Edit it here, in this repo, then
update the version in whichever project consumes it. Never edit `ui-kit-shadcn` files from
inside a project that installed it.

See `docs/DESIGN-SYSTEM.md` for the rationale behind the palette/spacing/component choices —
useful background when building something new that isn't in the kit yet, not something to
copy values out of by hand.

## Installing in a project

From the consuming project:

```
npm install ~/Github/ui-kit-shadcn
```

(or, once this repo has a remote: `npm install git+ssh://git@github.com/<you>/ui-kit-shadcn.git`)

This adds a `file:` — or git — dependency, which npm installs into `node_modules` as a real
copy. It is **not** a symlink into this repo, so editing the installed copy won't touch this
repo and won't survive a reinstall — which is exactly the boundary you want.

To pick up changes after editing this repo, re-run `npm install` in the consuming project.

## Using it

**1. Layer the theme CSS into your app's stylesheet**, after Tailwind/shadcn's own imports:

```css
@import "tailwindcss";
@import "tw-animate-css";
@import "shadcn/tailwind.css";
@import "ui-kit-shadcn/theme.css";
```

Light mode is left as shadcn's stock neutrals (`shadcn/tailwind.css` already provides it) —
this kit only overrides `.dark`.

**2. Wrap your app in the theme provider:**

```tsx
import { ThemeProvider, Toaster } from "ui-kit-shadcn"

function App() {
  return (
    <ThemeProvider storageKey="myapp-theme">
      {/* ...your app... */}
      <Toaster />
    </ThemeProvider>
  )
}
```

**3. Import components as needed:**

```tsx
import { Button, Card, CardContent, Dialog, DialogContent } from "ui-kit-shadcn"
```

**4. Dialog footer buttons** — every dialog in the reference app pairs an orange `cancel`
button with a green `save` button (see `docs/DESIGN-SYSTEM.md` → Buttons). Use the variants
directly instead of rebuilding the color classes:

```tsx
<DialogFooter>
  <Button variant="cancel" onClick={onClose}>Cancel</Button>
  <Button variant="save" type="submit">Save</Button>
</DialogFooter>
```

## Peer dependencies

Your project needs `react`, `react-dom`, `tailwindcss`, `@tailwindcss/vite` (or whatever
Tailwind 4 build integration you use), `tw-animate-css`, and the `shadcn` package itself for its
base Tailwind layer (`shadcn/tailwind.css`) — everything else this kit needs
(`@base-ui/react`, `class-variance-authority`, `lucide-react`, `sonner`, etc.) is declared as a
regular dependency and installs automatically.

## Adding a new component to the kit

Use the `shadcn` CLI from inside this repo (it reads `components.json`) so new components land
consistent with the existing ones:

```
npx shadcn add <component>
```

Then add the new file to `src/index.ts`'s exports.

## What's deliberately *not* in this kit

App-specific things stay in the consuming project, not here: page layouts, entity-specific
dialogs (e.g. an "Employee" form), API clients, routing, branding/logo. This kit only ships the
generic design-system layer — the pieces that should look identical across every project that
uses it.
