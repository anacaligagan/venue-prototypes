# Project Instructions

## Design System Usage

When adding new pages or features, always use the design tokens and components from the `style-guide/` directory. Do not hardcode colors, spacing, typography, or shadows when a token exists.

### Design Tokens

Reference: `style-guide/tokens.css` — import this in every new page.

Use CSS custom properties instead of raw values:

| Category | Token prefix | Example | Instead of |
|---|---|---|---|
| Colors | `--color-*`, `--palette-*` | `var(--color-text-strong)` | `#080820` |
| Typography | `--font-family-*`, `--font-size-*`, `--font-weight-*`, `--font-line-height-*` | `var(--font-family-body)` | `'Lato', sans-serif` |
| Spacing | `--space-*` | `var(--space-m)` | `16px` |
| Border radius | `--radius-*` | `var(--radius-m)` | `8px` |
| Shadows | `--shadow-*` | `var(--shadow-card)` | `0px 12px 32px 0px rgba(0,0,0,0.08)` |

### Components

Before creating new markup, check if an existing component fits:

| Component | File | Key classes |
|---|---|---|
| Button | `style-guide/button.css` | `.btn`, `.btn--primary`, `.btn--secondary`, `.btn--tertiary`, `.btn--destructive` |
| Card | `style-guide/card.css` | `.card`, `.card--stroke`, `.card--shadow` |
| Modal | `style-guide/modal.css` | `.modal`, `.modal__header`, `.modal__content`, `.modal__footer` |
| Input | `style-guide/input.css` | `.field`, `.field__label`, `.field__input` |
| Lozenge | `style-guide/lozenge.css` | `.lozenge`, `.lozenge--primary`, `.lozenge--success`, `.lozenge--error` |
| Navbar | `style-guide/navbar.css` | `.navbar`, `.navbar__nav-link`, `.navbar__venue` |
| Banner | `style-guide/banner.css` | `.banner`, `.banner--info`, `.banner--warning`, `.banner--success`, `.banner--error` |
| Table | `style-guide/table.css` | `.table-wrap`, `.table__th`, `.table__td`, `.table__tr` |
| Tabs | `style-guide/tabs.css` | `.tabs`, `.tab`, `.tab__label`, `.tab__bar` |

### Rules

- **Never hardcode** hex colors, font sizes, spacing pixel values, or shadow values when a matching token exists in `tokens.css`
- **Always check** the style guide for an existing component before writing new markup patterns
- **Import `tokens.css`** in every new page
- **When no token or component fits**, hardcoding is acceptable — flag it with a `/* TODO: audit token */` comment so it can be reviewed and potentially promoted to the style guide later
