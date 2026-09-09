---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/design-tokens-reference.md
description: >-
  The full catalog of design-token CSS custom properties available to a widget,
  grouped by purpose, with default values
---

# Design Tokens Reference

The platform injects design tokens into your widget's Shadow DOM as CSS custom properties. Every token below is available on every community.

Reference a token with `var()` and always give it a fallback. The fallback applies whenever the property is not set — before a community's branding has loaded, or outside a community context altogether. Use the token's `Default value` as that fallback, except for the `--font-family-*` tokens: their defaults name a single family, so append a generic family after it (`var(--font-family-sans, Roboto, sans-serif)`).

## Widget tokens

### Theme

| Token | Default value |
|---|---|
| `--theme-brand-100` | `#F7F0FE` |
| `--theme-brand-200` | `#E9D8FD` |
| `--theme-brand-300` | `#CEAAF8` |
| `--theme-brand-400` | `#BC8CF2` |
| `--theme-brand-500` | `#AF7AEB` |
| `--theme-brand-600` | `#A267E4` |
| `--theme-brand-700` | `#9254D9` |
| `--theme-brand-800` | `#7B3BC4` |
| `--theme-brand-900` | `#452965` |
| `--theme-brand-1000` | `#322541` |
| `--theme-neutral-0` | `#FFFFFF` |
| `--theme-neutral-100` | `#FAFAFA` |
| `--theme-neutral-200` | `#F4F5F6` |
| `--theme-neutral-300` | `#DEDFE2` |
| `--theme-neutral-400` | `#B8BBC2` |
| `--theme-neutral-500` | `#8D919B` |
| `--theme-neutral-600` | `#7B808E` |
| `--theme-neutral-700` | `#696E7C` |
| `--theme-neutral-800` | `#4F5663` |
| `--theme-neutral-900` | `#3B4254` |
| `--theme-neutral-1000` | `#2B3346` |

### Color

| Token | Default value | Description |
|---|---|---|
| `--color-action-destructive-default` | `#c92c2f` | color used for actions that perform irreversible or potentially harmful operations |
| `--color-action-destructive-hover` | `#ae2424` | color used when a destructive action is hovered |
| `--color-action-destructive-pressed` | `#5d1a1c` | color used when a destructive action is actively pressed |
| `--color-action-neutral-default` | `#FFFFFF` | color used for less prominent actions that support the primary action |
| `--color-action-neutral-hover` | `#FAFAFA` | color used when a secondary action is hovered |
| `--color-action-neutral-pressed` | `#F4F5F6` | color used when a secondary action is actively pressed |
| `--color-action-primary-default` | `#9254D9` | default color used for the most prominent call-to-action in the interface |
| `--color-action-primary-hover` | `#7B3BC4` | color used when a primary action is hovered |
| `--color-action-primary-pressed` | `#452965` | color used when a primary action is actively pressed |
| `--color-community-answered` | `#1f8444` | color used to indicate that a question or thread has received an accepted answer |
| `--color-community-highlighted` | `#9254D9` | color used to highlight content that is featured or editorially promoted |
| `--color-community-pinned` | `#9254D9` | color used to indicate content that is pinned and prioritized in listings |
| `--color-community-unanswered` | `#696E7C` | color used to indicate that a question or thread has not yet received a response |
| `--color-content-default` | `#2B3346` | default color used for primary body text and high-emphasis content |
| `--color-content-disabled` | `#B8BBC2` | color used for text on disabled elements or unavailable content |
| `--color-content-heading-default` | `#2B3346` | default color used for section and component headings |
| `--color-content-heading-hero` | `#FFFFFF` | color used for prominent page-level headings displayed on hero or banner areas |
| `--color-content-inverse` | `#FFFFFF` | color used for text displayed on high-contrast or brand-colored backgrounds |
| `--color-content-subtle` | `#4F5663` | color used for secondary text such as helper text, metadata, or supporting information |
| `--color-content-subtlest` | `#696E7C` | color used for low-emphasis text such as placeholders or less important UI copy |
| `--color-focus-ring` | `#388bff` | default color used for focus rings on interactive elements |
| `--color-line-default` | `#DEDFE2` | default border and divider color used for standard UI boundaries, separators, and component outlines |
| `--color-line-disabled` | `#DEDFE2` | border and divider color used for disabled components or non-interactive states where visual emphasis is reduced |
| `--color-link-default` | `#9254D9` | default color used for interactive text links |
| `--color-link-hover` | `#7B3BC4` | color used for interactive text links on hover |
| `--color-overlay-backdrop` | `#2b33467a` | semi-transparent color displayed behind modals to obscure background content |
| `--color-overlay-interactive-hover` | `#2b33460a` | overlay color applied to interactive elements to indicate a hover state |
| `--color-overlay-interactive-pressed` | `#2b334614` | overlay color applied to interactive elements to indicate an active or pressed state |
| `--color-skeleton-default` | `#F4F5F6` | base color used for loading placeholders while content is being fetched |
| `--color-skeleton-subtle` | `#2b33460a` | highlight color used in animated skeleton states to simulate loading progress |
| `--color-status-danger-bold` | `#c92c2f` | high-emphasis color used for neutral informational messages |
| `--color-status-information-bold` | `#0c66e4` | high-emphasis color used to indicate errors or critical issues |
| `--color-status-success-bold` | `#1f8444` | high-emphasis color used to communicate successful outcomes or confirmations |
| `--color-surface-default` | `#FFFFFF` | default container surface for standard UI sections (cards, panels, content areas) |
| `--color-surface-disabled` | `#F4F5F6` | background of disabled interactive elements such as inputs, buttons, or selection controls |
| `--color-surface-inverse` | `#2B3346` | color used for inverse surfaces such as tooltips and high-contrast floating UI |
| `--color-surface-muted` | `#F4F5F6` | sub-surface used inside containers to visually group secondary areas (e.g., comment/reply bubbles) |
| `--color-surface-overlay` | `#FFFFFF` | elevated surface for overlays and floating layers (modals, dropdowns, popovers). |
| `--color-surface-page` | `#FAFAFA` | application canvas background used behind all UI surfaces |

### Text

| Token | Default value |
|---|---|
| `--text-body-large-font-size` | `1.25rem` |
| `--text-body-large-font-weight` | `400` |
| `--text-body-large-line-height` | `1.5` |
| `--text-body-medium-font-size` | `1rem` |
| `--text-body-medium-font-weight` | `400` |
| `--text-body-medium-line-height` | `1.5` |
| `--text-body-small-font-size` | `0.875rem` |
| `--text-body-small-font-weight` | `400` |
| `--text-body-small-line-height` | `1.5` |
| `--text-button-font-size` | `0.875rem` |
| `--text-button-font-weight` | `500` |
| `--text-button-line-height` | `1.25rem` |
| `--text-caption-font-size` | `0.75rem` |
| `--text-caption-font-weight` | `400` |
| `--text-caption-line-height` | `1rem` |
| `--text-code-font-size` | `1rem` |
| `--text-code-font-weight` | `400` |
| `--text-code-line-height` | `1.5` |
| `--text-heading-large-font-size` | `1.75rem` |
| `--text-heading-large-font-weight` | `700` |
| `--text-heading-large-line-height` | `1.25` |
| `--text-heading-medium-font-size` | `1.5rem` |
| `--text-heading-medium-font-weight` | `500` |
| `--text-heading-medium-line-height` | `1.25` |
| `--text-heading-small-font-size` | `1.25rem` |
| `--text-heading-small-font-weight` | `500` |
| `--text-heading-small-line-height` | `1.25` |
| `--text-heading-xlarge-font-size` | `2rem` |
| `--text-heading-xlarge-font-weight` | `700` |
| `--text-heading-xlarge-line-height` | `1.25` |
| `--text-hero-font-size` | `2rem` |
| `--text-hero-font-weight` | `700` |
| `--text-hero-line-height` | `1.25` |
| `--text-label-medium-font-size` | `1rem` |
| `--text-label-medium-font-weight` | `500` |
| `--text-label-medium-line-height` | `1.5rem` |
| `--text-label-small-font-size` | `0.875rem` |
| `--text-label-small-font-weight` | `500` |
| `--text-label-small-line-height` | `1.25rem` |
| `--text-menu-font-size` | `0.875rem` |
| `--text-menu-font-weight` | `400` |
| `--text-menu-line-height` | `1.25rem` |
| `--text-navigation-font-size` | `0.875rem` |
| `--text-navigation-font-weight` | `400` |
| `--text-navigation-line-height` | `1.25rem` |

### Font

| Token | Default value |
|---|---|
| `--font-family-button` | `Roboto` |
| `--font-family-heading` | `Roboto` |
| `--font-family-mono` | `Roboto Mono` |
| `--font-family-sans` | `Roboto` |
| `--font-size-100` | `0.75rem` |
| `--font-size-200` | `0.875rem` |
| `--font-size-300` | `1rem` |
| `--font-size-400` | `1.25rem` |
| `--font-size-500` | `1.5rem` |
| `--font-size-600` | `1.75rem` |
| `--font-size-700` | `2rem` |
| `--font-size-base` | `16px` |
| `--font-weight-bold` | `700` |
| `--font-weight-medium` | `500` |
| `--font-weight-regular` | `400` |

### Space

| Token | Default value | Description |
|---|---|---|
| `--space-0` | `0px` |  |
| `--space-025` | `2px` |  |
| `--space-050` | `4px` |  |
| `--space-075` | `6px` |  |
| `--space-100` | `8px` |  |
| `--space-125` | `10px` |  |
| `--space-150` | `12px` |  |
| `--space-200` | `16px` |  |
| `--space-250` | `20px` |  |
| `--space-300` | `24px` |  |
| `--space-400` | `32px` |  |
| `--space-500` | `40px` |  |
| `--space-600` | `48px` |  |
| `--space-800` | `64px` |  |
| `--space-1000` | `80px` |  |
| `--space-layout-gutter` | `10px` | space used as the default gap between columns or layout elements |
| `--space-negative-025` | `-2px` |  |
| `--space-negative-050` | `-4px` |  |
| `--space-negative-075` | `-6px` |  |
| `--space-negative-100` | `-8px` |  |
| `--space-negative-150` | `-12px` |  |
| `--space-negative-200` | `-16px` |  |

### Radius

| Token | Default value |
|---|---|
| `--radius-025` | `2px` |
| `--radius-050` | `4px` |
| `--radius-100` | `8px` |
| `--radius-150` | `12px` |
| `--radius-200` | `16px` |
| `--radius-full` | `9999px` |

### Border

| Token | Default value |
|---|---|
| `--border-width-100` | `1px` |
| `--border-width-200` | `2px` |

### Line

| Token | Default value |
|---|---|
| `--line-height-grid-100` | `1rem` |
| `--line-height-grid-200` | `1.25rem` |
| `--line-height-grid-300` | `1.5rem` |
| `--line-height-grid-400` | `1.75rem` |
| `--line-height-grid-500` | `2rem` |
| `--line-height-grid-600` | `2.25rem` |
| `--line-height-grid-700` | `2.5rem` |
| `--line-height-ratio-relaxed` | `1.5` |
| `--line-height-ratio-tight` | `1.25` |

### Shadow

| Token | Default value |
|---|---|
| `--shadow-100` | `0px 2px 4px -2px rgba(0, 0, 0, 0.1), 0px 4px 6px -1px rgba(0, 0, 0, 0.1)` |
| `--shadow-200` | `0px 4px 6px -4px rgba(0, 0, 0, 0.1), 0px 10px 15px -3px rgba(0, 0, 0, 0.1)` |
| `--shadow-300` | `0px 8px 10px -6px rgba(0, 0, 0, 0.1), 0px 20px 25px -5px rgba(0, 0, 0, 0.1)` |

### Elevation

| Token | Default value | Description |
|---|---|---|
| `--elevation-1` | `0px 2px 4px -2px rgba(0, 0, 0, 0.1), 0px 4px 6px -1px rgba(0, 0, 0, 0.1)` | low elevation used for raised surfaces above the base layout |
| `--elevation-2` | `0px 4px 6px -4px rgba(0, 0, 0, 0.1), 0px 10px 15px -3px rgba(0, 0, 0, 0.1)` | medium elevation used for floating surfaces above regular content |
| `--elevation-3` | `0px 8px 10px -6px rgba(0, 0, 0, 0.1), 0px 20px 25px -5px rgba(0, 0, 0, 0.1)` | high elevation used for top-level surfaces above other floating elements |

### Button

| Token | Default value | Description |
|---|---|---|
| `--button-border-width` | `1px` | Border width used for all button variants. |
| `--button-destructive-background-default` | `#c92c2f` | Destructive button background in default state. |
| `--button-destructive-background-hover` | `#ae2424` | Destructive button background on hover. |
| `--button-destructive-background-pressed` | `#5d1a1c` | Destructive button background on press. |
| `--button-destructive-border-default` | `#00000000` | Destructive button border color in default state (no visible border). |
| `--button-destructive-border-hover` | `#00000000` | Destructive button border color on hover (no visible border). |
| `--button-destructive-border-pressed` | `#00000000` | Destructive button border color on press (no visible border). |
| `--button-destructive-content-default` | `#FFFFFF` | Destructive button label/icon color in default state. |
| `--button-destructive-content-hover` | `#FFFFFF` | Destructive button label/icon color on hover. |
| `--button-destructive-content-pressed` | `#FFFFFF` | Destructive button label/icon color on press. |
| `--button-destructive-shadow` | `none` | Destructive button shadow (none). |
| `--button-font-weight` | `500` | Font weight used for button labels across all variants. |
| `--button-primary-background-default` | `#9254D9` | Primary button background in default state. |
| `--button-primary-background-hover` | `#7B3BC4` | Primary button background on hover. |
| `--button-primary-background-pressed` | `#452965` | Primary button background on press. |
| `--button-primary-border-default` | `#00000000` | Primary button border color in default state (no visible border). |
| `--button-primary-border-hover` | `#00000000` | Primary button border color on hover (no visible border). |
| `--button-primary-border-pressed` | `#00000000` | Primary button border color on press (no visible border). |
| `--button-primary-content-default` | `#FFFFFF` | Primary button label/icon color in default state. |
| `--button-primary-content-hover` | `#FFFFFF` | Primary button label/icon color on hover. |
| `--button-primary-content-pressed` | `#FFFFFF` | Primary button label/icon color on press. |
| `--button-primary-shadow` | `none` | Primary button shadow (none). |
| `--button-radius` | `8px` | Border radius used for all button variants. |
| `--button-secondary-background-default` | `#FFFFFF` | Secondary button background in default state. |
| `--button-secondary-background-hover` | `#FAFAFA` | Secondary button background on hover. |
| `--button-secondary-background-pressed` | `#F4F5F6` | Secondary button background on press. |
| `--button-secondary-border-default` | `#DEDFE2` | Secondary button border color in default state. |
| `--button-secondary-border-hover` | `#DEDFE2` | Secondary button border color on hover. |
| `--button-secondary-border-pressed` | `#DEDFE2` | Secondary button border color on press. |
| `--button-secondary-content-default` | `#2B3346` | Secondary button label/icon color in default state. |
| `--button-secondary-content-hover` | `#2B3346` | Secondary button label/icon color on hover. |
| `--button-secondary-content-pressed` | `#2B3346` | Secondary button label/icon color on press. |
| `--button-secondary-shadow` | `none` | Secondary button shadow (none). |
| `--button-text-transform` | `none` | Text transform applied to button labels (e.g., none/uppercase). |
| `--button-vote-background-default` | `#FFFFFF` | Vote button background in default state. |
| `--button-vote-background-hover` | `#FAFAFA` | Vote button background on hover. |
| `--button-vote-background-pressed` | `#F4F5F6` | Vote button background on press. |
| `--button-vote-border-default` | `#DEDFE2` | Vote button border color in default state. |
| `--button-vote-border-hover` | `#DEDFE2` | Vote button border color on hover. |
| `--button-vote-border-pressed` | `#DEDFE2` | Vote button border color on press. |
| `--button-vote-content-default` | `#2B3346` | Vote button content color in default state. |
| `--button-vote-content-hover` | `#2B3346` | Vote button content color on hover. |
| `--button-vote-content-pressed` | `#2B3346` | Vote button content color on press. |
| `--button-vote-selected-background-default` | `#9254D9` | Vote button background when selected (default). |
| `--button-vote-selected-background-hover` | `#7B3BC4` | Vote button background when selected (hovered). |
| `--button-vote-selected-background-pressed` | `#452965` | Vote button background when selected (pressed). |
| `--button-vote-selected-border-default` | `#00000000` | Vote button border when selected (default) (no visible border). |
| `--button-vote-selected-border-hover` | `#00000000` | Vote button border when selected (hovered) (no visible border). |
| `--button-vote-selected-border-pressed` | `#00000000` | Vote button border when selected (pressed) (no visible border). |
| `--button-vote-selected-content-default` | `#FFFFFF` | Vote button content color when selected (default). |
| `--button-vote-selected-content-hover` | `#FFFFFF` | Vote button content color when selected (hovered). |
| `--button-vote-selected-content-pressed` | `#FFFFFF` | Vote button content color when selected (pressed). |
| `--button-vote-selected-shadow-default` | `none` | Vote button shadow when selected (default) (none). |
| `--button-vote-selected-shadow-hover` | `none` | Vote button shadow when selected (hovered) (none). |
| `--button-vote-selected-shadow-pressed` | `none` | Vote button shadow when selected (pressed) (none). |
| `--button-vote-shadow-default` | `none` | Vote button shadow in default state (none). |
| `--button-vote-shadow-hover` | `none` | Vote button shadow on hover (none). |
| `--button-vote-shadow-pressed` | `none` | Vote button shadow on press (none). |

### Card

| Token | Default value | Description |
|---|---|---|
| `--card-background-default` | `#FFFFFF` | Card background in default state. |
| `--card-background-hover` | `#FFFFFF` | Card background on hover. |
| `--card-background-pressed` | `#FFFFFF` | Card background on press. |
| `--card-border-default` | `#DEDFE2` | Card border color in default state. |
| `--card-border-hover` | `#DEDFE2` | Card border color on hover. |
| `--card-border-pressed` | `#DEDFE2` | Card border color on press. |
| `--card-border-width` | `1px` | Border width used for cards. |
| `--card-content-default` | `#2B3346` | Card body/content color in default state. |
| `--card-content-hover` | `#2B3346` | Card body/content color on hover. |
| `--card-content-pressed` | `#7B3BC4` | Card body/content color on press (link hovered styling). |
| `--card-radius` | `8px` | Border radius used for cards. |
| `--card-shadow-default` | `none` | Card shadow in default state (none). |
| `--card-shadow-hover` | `0px 2px 4px -2px rgba(0, 0, 0, 0.1), 0px 4px 6px -1px rgba(0, 0, 0, 0.1)` | Card shadow on hover. |
| `--card-shadow-pressed` | `0px 2px 4px -2px rgba(0, 0, 0, 0.1), 0px 4px 6px -1px rgba(0, 0, 0, 0.1)` | Card shadow on press. |
| `--card-title-default` | `#2B3346` | Card title color in default state. |
| `--card-title-hover` | `#9254D9` | Card title color on hover (link styling). |
| `--card-title-pressed` | `#2B3346` | Card title color on press. |

## Community page tokens

These style the community's own pages — its navigation, sidebars, and feeds. They are listed so you can recognize them, but a widget should not use them: they are not part of a widget's own styling.

### Back to Top (not for widgets)

| Token | Default value | Description |
|---|---|---|
| `--backtotop-background` | `#2B3346` | Back-to-top button background. |
| `--backtotop-content` | `#FFFFFF` | Back-to-top button icon/content color. |

### Feed (not for widgets)

| Token | Default value | Description |
|---|---|---|
| `--feed-container-background` | `#FFFFFF` | Feed container background. |
| `--feed-container-border-color` | `#DEDFE2` | Feed container border color. |
| `--feed-container-border-width` | `1px` | Border width used for the feed container. |
| `--feed-container-radius` | `8px` | Border radius used for the feed container. |
| `--feed-container-shadow` | `none` | Feed container shadow (none). |
| `--feed-item-background-default` | `#FFFFFF` | Feed item background in default state. |
| `--feed-item-background-hover` | `#FFFFFF` | Feed item background on hover. |
| `--feed-item-background-pressed` | `#FFFFFF` | Feed item background on press. |
| `--feed-item-border-default` | `#DEDFE2` | Feed item border color in default state. |
| `--feed-item-border-hover` | `#DEDFE2` | Feed item border color on hover. |
| `--feed-item-border-pressed` | `#DEDFE2` | Feed item border color on press. |
| `--feed-item-border-width` | `1px` | Border width used for feed items. |
| `--feed-item-content-default` | `#2B3346` | Feed item content color in default state. |
| `--feed-item-content-hover` | `#2B3346` | Feed item content color on hover. |
| `--feed-item-content-pressed` | `#7B3BC4` | Feed item content color on press (link hovered styling). |
| `--feed-item-radius` | `8px` | Border radius used for feed items. |
| `--feed-item-shadow-default` | `none` | Feed item shadow in default state (none). |
| `--feed-item-shadow-hover` | `0px 2px 4px -2px rgba(0, 0, 0, 0.1), 0px 4px 6px -1px rgba(0, 0, 0, 0.1)` | Feed item shadow on hover. |
| `--feed-item-shadow-pressed` | `0px 2px 4px -2px rgba(0, 0, 0, 0.1), 0px 4px 6px -1px rgba(0, 0, 0, 0.1)` | Feed item shadow on press. |
| `--feed-item-title-default` | `#2B3346` | Feed item title color in default state. |
| `--feed-item-title-hover` | `#9254D9` | Feed item title color on hover (link styling). |
| `--feed-item-title-pressed` | `#7B3BC4` | Feed item title color on press (link hovered styling). |

### Navigation (not for widgets)

| Token | Default value | Description |
|---|---|---|
| `--navigation-background` | `#FFFFFF` | Navigation bar background. |
| `--navigation-border-bottom` | `#DEDFE2` | Bottom border of navigation. |
| `--navigation-border-top` | `#00000000` | Top border of navigation (transparent). |
| `--navigation-content` | `#2B3346` | Navigation bar content color (text/icons). |
| `--navigation-dropdown-background` | `#FFFFFF` | Navigation dropdown background. |
| `--navigation-dropdown-content` | `#2B3346` | Navigation dropdown content color. |

### Sidebar (not for widgets)

| Token | Default value | Description |
|---|---|---|
| `--sidebar-background` | `#FFFFFF` | Sidebar widget placeholder background color. |
| `--sidebar-border-color` | `#DEDFE2` | Sidebar widget placeholder border color. |
| `--sidebar-border-radius` | `8px` | Sidebar widget placeholder border radius. |
| `--sidebar-border-width` | `1px` | Sidebar widget placeholder border width. |
| `--sidebar-shadow` | `none` | Sidebar widget placeholder box shadow. |
| `--sidebar-title-color` | `#2B3346` | Sidebar widget title text color. |
| `--sidebar-title-font-family` | `Roboto` | Sidebar widget title font family. |
| `--sidebar-title-font-weight` | `500` | Sidebar widget title font weight. |

### Page Panels (not for widgets)

| Token | Default value | Description |
|---|---|---|
| `--widget-background` | `#FAFAFA` | Default widget background. |
| `--widget-featured-background` | `#0c66e4` | Featured topic widget background. |
| `--widget-featured-content` | `#FFFFFF` | Featured topic widget content color. |
| `--widget-hero-background` | `uploaded image` | Background of the hero section. |
| `--widget-hero-content-shadow` | `none` | Hero widget content shadow (none; handled by image/overlay as needed). |
| `--widget-hero-height` | `240px` | Height of the hero section. |
| `--widget-hero-subtitle` | `#FFFFFF` | Hero widget subtitle color. |
| `--widget-hero-title` | `#FFFFFF` | Hero widget title color. |
| `--widget-introduction-background` | `#FFFFFF` | Introduction widget background. |
| `--widget-introduction-close-content` | `#696E7C` | Introduction widget close button color. |
| `--widget-introduction-content` | `#2B3346` | Introduction widget body/content color. |
| `--widget-introduction-icon-background` | `#1f8444` | Introduction widget icon container background. |
| `--widget-introduction-icon-content` | `#FFFFFF` | Introduction widget icon color. |
| `--widget-shadow` | `none` | Default widget shadow (none). |
| `--widget-subtitle` | `#2B3346` | Default widget subtitle color. |
| `--widget-title` | `#2B3346` | Default widget title color. |

## Next Steps

* [Use Design Tokens](design-tokens) — usage rules and how to reference these tokens in your widget's CSS
* [Widget Runtime Reference](sdk-api-reference) — Properties, methods, and events on the `sdk` object
