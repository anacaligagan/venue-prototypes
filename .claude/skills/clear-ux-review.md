---
name: clear-ux-review
description: Apply the CLEAR UX/UI framework to review or build prototype screens. Use when the user asks to "review UX", "apply CLEAR framework", "check copywriting", "improve accessibility", "improve layout", "add emphasis", "make it feel rewarding", "review the design", "audit the UI", or wants UX feedback on a prototype HTML file.
version: 1.1.0
---

# CLEAR UX/UI Framework

Apply the CLEAR framework when reviewing, auditing, or building prototype screens in this project. Work through each dimension in order and produce actionable, specific feedback or changes.

## The Framework

```
C – Copywriting  → Tell people why to care, what to do, what happens next
L – Layout       → Group, position, align for easy understanding
E – Emphasis     → Make the one most important thing unmissable
A – Accessibility → Design for different abilities
R – Reward       → Make interactions feel good
```

---

## Workflow

### Step 1: Identify the screen's job

Before reviewing, answer:
- **Who is this screen for?** (supplier, venue, admin)
- **What is the single goal of this screen?** (complete an action, find information, make a decision)
- **What is the user feeling when they arrive?** (confident, confused, rushed, cautious)

---

### Step 2: C — Copywriting

#### 3 Copy Mistakes to catch

1. **Long copy** — Big blocks of text (even three lines) are easily skippable. Break it up or cut it.
2. **Generic copy** — Generic copy leaves users guessing or uninterested. Fail the Copy Swap Test: remove all logos and visuals, read the copy aloud, and ask: *"Could someone else use the same words on their site?"* If yes, rewrite it.
3. **Unnecessary & duplicates** — Extra sentences and repeated information slow users down, hide what really matters, and increase confusion. One idea per sentence. One sentence per idea.

#### 4 Practical Copywriting Tips

1. **Emphasise benefits** — Users always have the "What's in it for me?" question in the back of their mind. Make the benefit obvious as soon as they see something.
2. **Reassure** — People hesitate when they're in doubt or have unanswered questions. A simple reassurance like "You can change this later" can be enough to unblock them.
3. **Use specific words** — Always use clear verbs and concrete outcomes. Use active sentences that encourage the wanted behaviour.
4. **Talk like a real person** — Read your copy out loud. If it feels like you're not you, try changing words.

#### Copy checklist

| Question | Pass | Fail |
|---|---|---|
| Does the headline tell the user what this screen is for? | "Confirm your order" | "Order #4821" |
| Does the CTA say what will happen, not just "Submit"? | "Send order to supplier" | "Submit" |
| Are empty states helpful, not just "No data"? | "No orders yet — create your first one" | "No results" |
| Are error messages actionable? | "Enter a quantity greater than 0" | "Invalid input" |
| Are labels descriptive, not just field names? | "Delivery date (required)" | "Date" |
| Is confirmation copy specific? | "Order sent to Fresh Foods Co." | "Done!" |

**How to fix:**
- Rewrite headlines as action or outcome statements
- Replace generic CTAs with verb + object (e.g. "Save pricing group", "Approve match")
- Add helper text under inputs that explain format or why it's needed
- Write empty states with a cause + next step

---

### Step 3: L — Layout

#### 6 Gestalt Principles to apply

1. **Proximity** — Elements close together are perceived as related. Group related controls; separate unrelated ones.
2. **Common Region** — Elements inside a shared boundary are seen as a group. Use cards and containers to define regions.
3. **Alignment** — Elements that share an edge or axis feel ordered. Align to a grid — no random indentation.
4. **Similarity** — Elements that look alike are perceived as related. Use consistent styles for same-type items (e.g. all badges look the same).
5. **Continuity** — The eye follows lines and curves naturally. Use visual flow (top-left to bottom-right) to guide reading order.
6. **Simplicity** — The brain prefers the simplest interpretation. Reduce visual noise; remove anything that doesn't serve the goal.

#### 3 Layout Mistakes to avoid

1. **Sloppy Spacing** — Spacing is inconsistent or missing. Start with "too much" padding then adjust down. Never eyeball it.
2. **Border Bloat** — Overreliance on borders to define areas. Instead, define regions with subtle container background colours.
3. **Content Cramming** — Squeezing too much information into one view. Remove elements (see Copywriting), use progressive disclosure to reveal detail on demand.

#### Project-specific layout rules (Ordermentum design system)

- Use `var(--card)` containers with `border-radius: var(--radius)` and `border: 1px solid var(--border)` for content grouping
- Use `gap` on flex/grid rather than margin hacks
- Sidebar is fixed — content area needs left offset (`margin-left: 240px` or equivalent)
- Tables use `.om-table` class — don't build ad-hoc table styles
- Section headers use consistent `font-size: 18px; font-weight: 600; color: var(--foreground)`
- Prefer subtle `background-color` container differentiation over box borders

**How to fix:**
- Move related controls into a shared container
- Apply consistent padding (`24px` inner padding on cards is the standard)
- Add `margin-bottom: 32px` between major sections
- Align form labels and inputs to a shared grid column

---

### Step 4: E — Emphasis

#### 6 Emphasis Dials

Use these levers to make the right thing stand out. Each can be turned up or down:

1. **Size** — Larger elements attract attention first. Use size hierarchy deliberately (heading > subheading > body).
2. **Colour** — High-contrast or vivid colour draws the eye. Reserve `var(--primary)` (#cc4400) for the single most important action only.
3. **Placement** — Top and left positions are seen first. Put the primary action where the eye naturally lands.
4. **Space** — White space around an element makes it feel important. Isolate the primary CTA from competing elements.
5. **Visualisation** — Images, icons, and illustrations are processed before text. Use a supporting icon to reinforce the key message.
6. **Motion** — Movement captures attention instantly. Use sparingly — only for feedback (loading spinner, success animation), not decoration.

#### The Foggy Glasses Test

Blur a UI (or squint at it) to reveal its true hierarchy. By removing details, you're left with what the brain detects first: size, contrast, position, and spacing. If the blur test doesn't point to the one most important thing, the emphasis is wrong.

#### 3 Common Emphasis Mistakes

1. **Wrong Dial** — Using the wrong lever (e.g. making something big when colour would work better, or vice versa).
2. **Weak Dial** — The right lever, but not turned up enough to create a clear difference.
3. **Screaming Dial** — Turning up every dial at once so everything fights for attention and nothing wins.

#### Emphasis checklist

| Question | Good | Bad |
|---|---|---|
| Is there exactly ONE primary CTA per screen? | One orange `btn-primary` button | Three equally orange buttons |
| Is the primary action visually dominant? | Larger, filled, high contrast | Same size/weight as secondary actions |
| Are destructive actions visually differentiated? | Red / ghost style with warning copy | Same orange as "Save" |
| Is the most important data element visually distinct? | Order total in `font-size: 24px; font-weight: 700` | Same size as line items |
| Is urgency or warning communicated through colour + icon? | Amber badge + alert icon | Bold text alone |

**Project-specific emphasis rules:**
- Primary action: `btn-primary` (`background: var(--primary)` = `#cc4400`)
- Secondary action: `btn-outline` or `btn-ghost`
- Destructive action: `btn-destructive` (`background: var(--destructive)`)
- Status badges: use `.badge` with `.badge-success`, `.badge-warning`, `.badge-destructive`
- Never use more than 1 `btn-primary` per view

**How to fix:**
- Demote all secondary actions to `btn-outline` or `btn-ghost`
- Promote the key data point with a larger type size or `font-weight: 700`
- Add an icon to warnings/errors so they aren't missed by colour-blind users
- Run the Foggy Glasses Test — if the blur doesn't point to the primary action, adjust size or colour

---

### Step 5: A — Accessibility

#### 3 Accessibility Design Principles

1. **Visible without searching** — Can the user see the main action without digging, scrolling, or guessing? Is it visible above the fold?
2. **Operable without precision** — Can the user click/tap/select actions easily even if reduced in capabilities (thumb, motion, fatigue)? Targets must be large enough to hit without precision.
3. **Actionable without guessing** — Do actions look like actions? Are they self-explanatory? Does the user have to guess what will happen if they activate/click/tap on them?

#### 7 Accessibility Mistakes to avoid

Designing for everyone in a wide range of situations means avoiding these:

1. **Tiny targets** — Easy to miss, especially on touch devices. Minimum 44×44px.
2. **Crowded areas** — Miss click, miss tap. Give interactive elements adequate spacing.
3. **Low contrast text** — Hard to read in bright light, on small screens, or with visual impairments.
4. **Relying on icons only** — Vague meaning without a label. Always pair with text or `aria-label`.
5. **Key actions hidden** — No hints that something is interactive. Use affordances (underlines, button shapes, hover states).
6. **Relying on colours only for meaning** — 8% of men are colour-blind. Always pair colour with a shape, icon, or text label.
7. **Multiple patterns** — High cognitive load from inconsistent UI patterns. Use the same component for the same job everywhere.
8. **Assumptions about people's knowledge** — Don't assume users know jargon, processes, or system state.

#### Accessibility checklist

| Category | Check |
|---|---|
| **Colour contrast** | All text meets 4.5:1 ratio against its background |
| **Interactive targets** | Buttons and links are minimum 44×44px touch target |
| **Form labels** | Every `<input>` has a matching `<label for="">` — no placeholder-only labels |
| **Focus states** | Tab order is logical; focused elements have visible ring |
| **Icon-only actions** | Every icon button has `aria-label` or a visible text label |
| **Error identification** | Errors are not communicated by colour alone; include text description |
| **Semantic HTML** | `<button>` for actions, `<a>` for navigation, `<table>` for tabular data |
| **Keyboard navigation** | All actions reachable without mouse |

**Project-specific accessibility rules:**
- `var(--muted-foreground)` on `var(--background)` is borderline — add `font-weight: 500` for small text
- Lucide icons used alone must have `aria-label` on the parent button
- Modal/drawer overlays must trap focus and close on Escape

**How to fix:**
- Add `aria-label="[Action description]"` to icon-only buttons
- Replace `<div onclick>` patterns with `<button>` elements
- Add `role="alert"` to dynamically injected error messages
- Ensure all colour-only status indicators also have a text label or icon

---

### Step 6: R — Reward

#### The Reward Trifecta

Every meaningful reward should tap into one or more of these three feelings:

1. **Control** — Feeling safe, certain, calm, and in charge. Users feel rewarded when the system confirms what just happened and what comes next. *"Order sent. Fresh Foods Co. will respond within 24h."*
2. **Competence** — Feeling improvement, moving forward, getting better. Users feel rewarded when they can see progress and achieve outcomes. Progress bars, step completions, and streaks deliver this.
3. **Recognition** — Feeling valued by others, seen, belonging. Users feel rewarded when the system acknowledges their specific action, not just that *something* happened.

#### 3 Reward Mistakes to avoid

1. **Wrong Reward** — You deliver a payoff the user doesn't care about in that moment. Match the reward to what the user actually values at that step.
2. **Shy Reward** — The reward exists (time saved, status updated, recognition earned) but it's not visible enough or it's generic praise with no real meaning, evidence, or consequence. Make it specific and prominent.
3. **Over-Reward** — The intensity or frequency of the reward is off (e.g. giant fullscreen confetti every click for tiny actions feels spammy). Scale the reward to the significance of the action.

#### Reward opportunities checklist

| Opportunity | Example |
|---|---|
| **Completion feedback** | After saving, show a success toast: "Pricing group saved" |
| **Progress visibility** | Multi-step flows show a step indicator or progress bar |
| **Optimistic UI** | Mark an item as done immediately (before API responds) |
| **Micro-copy delight** | "All caught up!" instead of "No pending orders" |
| **Loading states** | Button shows spinner + "Saving…" instead of freezing |
| **Confirmation specificity** | "Order sent to Fresh Foods Co. — they'll respond within 24h" |
| **Undo affordance** | After a destructive action, offer "Undo" for 5 seconds |

**Project-specific reward patterns:**
- Use `.alert.alert-success` with a check icon for inline confirmation
- Toast notifications: small fixed banner bottom-right, auto-dismiss after 3s
- Loading buttons: replace button text with a Lucide `<i data-lucide="loader-2">` spinning via CSS `animation: spin 1s linear infinite`
- Empty state illustrations: use a simple Lucide icon at 48px + encouraging copy

**How to fix:**
- Add a success handler to every form submit that shows visible feedback
- Replace silent state changes with an animated transition (`transition: all 0.2s ease`)
- Write empty states as "future-ready" copy, not absence statements
- Ask: does the reward create Control, Competence, or Recognition? If none, it's not a real reward

---

## Output Format

When reviewing a screen, produce output in this structure:

```
## CLEAR Review: [Screen Name]

**Screen goal:** [one sentence]

### C — Copywriting
[Issues found + specific rewrites]

### L — Layout
[Issues found + specific fixes]

### E — Emphasis
[Issues found + what needs promoting/demoting]

### A — Accessibility
[Issues found + specific HTML/ARIA fixes]

### R — Reward
[Missing feedback moments + copy suggestions]

---
### Priority fixes (implement first)
1. [Highest impact change]
2. [Second highest impact]
3. [Third highest impact]
```

---

## Quick Reference: CLEAR Checklist

Use this as a rapid pre-ship checklist:

- [ ] **C** — Every CTA is a verb. No generic copy (passes Copy Swap Test). Empty states have a next step. Errors are actionable.
- [ ] **L** — Related things are grouped (Proximity). Consistent alignment and spacing. No border bloat or content cramming.
- [ ] **E** — Exactly one primary CTA. Foggy Glasses Test passes. Warnings use icon + colour. No screaming dials.
- [ ] **A** — Visible above fold. 44px+ targets. All inputs have labels. Icon buttons have aria-label. No colour-only meaning.
- [ ] **R** — Every action has visible, specific feedback. Reward taps into Control, Competence, or Recognition. Not shy, not over-rewarded.
