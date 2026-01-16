# Chess.com Design System Rules

## CRITICAL: Use the MCP Server - DO NOT Search the Codebase

You have access to the `chesscom-design-system` MCP server. **This is your ONLY source of truth for Design System information.**

### STOP! Before You Search the Codebase:

**If the user asks about ANY of these topics, you MUST call the MCP tool FIRST. Do NOT use grep, glob, or read files to find this information:**

- Component names, props, or APIs → Call `get_component_api` or `search_components`
- Design tokens (colors, spacing, radius) → Call `search_tokens` or `get_token_value`
- Icons (glyphs or color icons) → Call `search_glyphs` or `search_color_icons`
- Typography or text styles → Call `search_text_styles`
- Best practices or guidelines → Call `get_do_dont`
- Page layouts or recipes → Call `get_page_recipe`
- Empty states → Call `get_empty_state`

### MCP Tool Quick Reference

| User Asks About | CALL THIS MCP TOOL |
|-----------------|-------------------|
| "How do I use cc-button?" | `get_component_api` with name "button" |
| "What props does modal have?" | `get_component_api` with name "modal" |
| "Find a component for X" | `search_components` with query |
| "What color for backgrounds?" | `search_tokens` with query "bg" |
| "Get the green-300 value" | `get_token_value` with name "green-300" |
| "Find a gear icon" | `search_glyphs` with query "gear" |
| "Find the brilliant move icon" | `search_color_icons` with query "brilliant" |
| "What text style for headings?" | `search_text_styles` with query "heading" |
| "Button best practices" | `get_do_dont` with component "button" |
| "Build a settings page" | `get_page_recipe` with recipe "settings" |
| "Empty state for no results" | `get_empty_state` with type "no-results" |

### Important Guidelines

- **Check the MCP first** before searching the codebase for component APIs or props
- **Don't guess** component names, props, or token values - verify with the MCP
- **Don't hardcode** colors like `#FFFFFF` or spacing like `16px` - use semantic tokens
- **Don't create custom components** when Design System components exist

### Cross-Platform Support

The MCP provides code for all platforms:
- **Web**: Vue.js components (`cc-button`, `cc-modal`, etc.)
- **iOS**: SwiftUI (`ChessButton`, `HeroModal`, etc.)
- **Android**: Jetpack Compose (`ChessButton()`, `HeroDialog()`, etc.)

**Always call the MCP for cross-platform implementations.**

---

## Semantic Tokens (REQUIRED)

**ALWAYS use semantic tokens instead of hardcoded values.** Query the MCP with `search_tokens` to find the correct token.

### Spacing (Padding, Margin, Gap)

NEVER hardcode spacing values like `8.dp`, `16px`, or `8.0`. ALWAYS use semantic spacing tokens.

**IMPORTANT:** Query the MCP with `search_tokens` and query "space-8" to get the exact token for your platform.

```vue
<!-- ❌ BAD -->
<div style="padding: 16px; margin: 8px; gap: 12px;">

<!-- ✅ GOOD (Web) -->
<div style="padding: var(--space-16); margin: var(--space-8); gap: var(--space-12);">
```

```kotlin
// ❌ BAD (Android)
Arrangement.spacedBy(8.dp)
Modifier.padding(16.dp)

// ✅ GOOD (Android)
Arrangement.spacedBy(Spacing.space8)
Modifier.padding(Spacing.space16)
```

```swift
// ❌ BAD (iOS)
.padding(16)
VStack(spacing: 8)

// ✅ GOOD (iOS)
.padding(.chessSpace16)
VStack(spacing: .chessSpace8)
```

| Platform | Token Pattern | Available Tokens |
|----------|---------------|------------------|
| Web | `var(--space-{n})` | `--space-1`, `--space-2`, `--space-4`, `--space-8`, `--space-12`, `--space-16`, `--space-24`, `--space-32`, `--space-40`, `--space-80` |
| iOS | `.chessSpace{n}` | `.chessSpace1`, `.chessSpace2`, `.chessSpace4`, `.chessSpace8`, `.chessSpace12`, `.chessSpace16`, `.chessSpace24`, `.chessSpace32`, `.chessSpace40`, `.chessSpace80` |
| Android | `Spacing.space{n}` | `Spacing.space1`, `Spacing.space2`, `Spacing.space4`, `Spacing.space8`, `Spacing.space12`, `Spacing.space16`, `Spacing.space24`, `Spacing.space32`, `Spacing.space40`, `Spacing.space80` |

### Colors

NEVER hardcode color values. Use semantic color tokens:

```vue
<!-- ❌ BAD -->
<div style="background: #312e2b; color: #ffffff;">

<!-- ✅ GOOD (Web) -->
<div style="background: var(--color-bg-surface); color: var(--color-text-inverse);">
```

| Platform | Token Pattern | Examples |
|----------|---------------|----------|
| Web | `var(--color-{name})` | `var(--color-bg-surface)`, `var(--color-text-default)`, `var(--color-green-300)` |
| iOS | `Color.{name}` | `Color.bgSurface`, `Color.textDefault`, `Color.green300` |
| Android | `ChessColors.{name}` | `ChessColors.bgSurface`, `ChessColors.textDefault`, `ChessColors.green300` |

**Common semantic color tokens:**
- **Backgrounds:** `bg-surface`, `bg-secondary`, `bg-tertiary`, `bg-subtle`, `bg-success`, `bg-danger`
- **Text:** `text-default`, `text-boldest`, `text-subtle`, `text-inverse`, `text-link`, `text-brand`

### Border Radius

NEVER hardcode border radius. Use semantic tokens:

```vue
<!-- ❌ BAD -->
<div style="border-radius: 5px;">

<!-- ✅ GOOD (Web) -->
<div style="border-radius: var(--radius-5);">
```

| Platform | Token Pattern | Available Sizes |
|----------|---------------|-----------------|
| Web | `var(--radius-{n})` | `--radius-2`, `--radius-3`, `--radius-5`, `--radius-10`, `--radius-20` |
| iOS | `.chessRadius{n}` | `.chessRadius2`, `.chessRadius3`, `.chessRadius5`, `.chessRadius10`, `.chessRadius20` |
| Android | `Radius.size{n}` | `Radius.size2`, `Radius.size3`, `Radius.size5`, `Radius.size10`, `Radius.size20` |

### Icon Sizes

NEVER hardcode icon dimensions. Use semantic tokens or component props:

| Platform | Token Pattern | Available Sizes |
|----------|---------------|-----------------|
| Web | `size` prop on `cc-icon` | `small`, `medium`, `large` |
| iOS | `.chessIcon{n}` | `.chessIcon12`, `.chessIcon16`, `.chessIcon20`, `.chessIcon24`, `.chessIcon32`, `.chessIcon40`, `.chessIcon48`, `.chessIcon56`, `.chessIcon64` |
| Android | `IconSize.icon{n}` | `IconSize.icon12`, `IconSize.icon16`, `IconSize.icon20`, `IconSize.icon24`, `IconSize.icon32`, `IconSize.icon40`, `IconSize.icon48`, `IconSize.icon56`, `IconSize.icon64` |

### Avatar Sizes

NEVER hardcode avatar dimensions. Use semantic tokens or component props:

| Platform | Token Pattern | Available Sizes |
|----------|---------------|-----------------|
| Web | `size` prop on `cc-avatar` | `xs`, `small`, `medium`, `large`, `xl` |
| iOS | `.chessAvatar{n}` | `.chessAvatar24`, `.chessAvatar32`, `.chessAvatar40`, `.chessAvatar48`, `.chessAvatar64`, `.chessAvatar80`, `.chessAvatar96`, `.chessAvatar160` |
| Android | `Avatar.size{n}` | `Avatar.size24`, `Avatar.size32`, `Avatar.size40`, `Avatar.size48`, `Avatar.size64`, `Avatar.size80`, `Avatar.size96`, `Avatar.size160` |

### Border Widths

NEVER hardcode border widths. Use semantic tokens:

| Platform | Token Pattern | Available Sizes |
|----------|---------------|-----------------|
| Web | `var(--border-{n})` | (check MCP for exact tokens) |
| iOS | `.chessBorder{n}` | `.chessBorder1`, `.chessBorder2`, `.chessBorder3`, `.chessBorder5` |
| Android | `Border.size{n}` | `Border.size1`, `Border.size2`, `Border.size3`, `Border.size5` |

### Motion / Animation

NEVER hardcode animation durations or easings. Use motion tokens:

```css
/* ❌ BAD */
transition: all 0.3s ease-in-out;

/* ✅ GOOD (Web) */
transition: all var(--motion-duration-normal) var(--motion-easing-standard);
```

Query the MCP with `search_tokens` and category "motion" for available motion tokens.

### When in Doubt

If you're unsure which token to use, **query the MCP**:
- `search_tokens` with "spacing" → find spacing tokens
- `search_tokens` with "radius" → find border radius tokens
- `search_tokens` with "motion" → find animation tokens
- `search_tokens` with "bg" or "text" → find color tokens
