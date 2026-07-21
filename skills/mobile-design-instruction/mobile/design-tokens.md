# Mobile Design Tokens — NDS Vietnam

> Sourced from Figma Mobile page (id: 104:2) via figma-mcp-go.  
> Last updated: 2026-06-24.  
> App: BonBonShop / VisibilityPRO mobile (iOS-first, 393px viewport).

---

## 1. Viewport & Device

| Token | Value | Notes |
|---|---|---|
| Viewport width | `393px` | iPhone 14 / 15 logical width |
| Status bar height | `48px` | iOS status bar |
| Home indicator container | `34px` | Bottom safe area container |
| Home indicator bar | `134 × 5px`, radius=`100`, fill=`#000000` | Centered in container |
| Safe area — top | `48px` | = status bar |
| Safe area — bottom | `34px` | = home indicator container |
| Standard screen width | `393px` | Full bleed |
| Content width | `361px` | 393 − 16 margin × 2 |
| Standard horizontal margin | `16px` | |

---

## 2. Color System

### Primary

| Token | Hex | Usage |
|---|---|---|
| `primarySystem` | `#013D50` | CTA buttons, active states (main), progress active, toggle ON, radio/checkbox selected |
| `primarySystem2` | `#026483` | Tab active stroke, chip active, price input focus/value — lighter teal accent |
| `primarySystem3` | `#E6ECEE` | Brand-tinted light background, loading spinner ring |

### Secondary

| Token | Hex | Usage |
|---|---|---|
| `secondarySystem` | `#EF4429` | Destructive actions, loading spinner accent, alert icon fill, image placeholders |

### Neutral / Text

| Token | Hex | Usage |
|---|---|---|
| `textPrimary` | `#333333` | Body text, titles, all main content |
| `textSecondary` | `#A1A1A1` | Placeholder, inactive label, caption, empty state |
| `textDisabled` | `#C0C0C0` | Disabled text, disabled icons |
| `borderDefault` | `#DFDFDF` | Input borders (default), card strokes, dividers |
| `backgroundPage` | `#F5F5F6` | Page background, disabled input fill, title section expandable bg |
| `backgroundWhite` | `#FFFFFF` | Cards, inputs, popup fills |

### Status / Semantic

| Token | Hex | Usage |
|---|---|---|
| `colorInfo` | `#007BFF` | Link, info icon, scanner icon, action text button (link style) |
| `colorInfoBg` | `#E6F2FF` | Info banner background |
| `colorWarning` | `#B58905` | Warning icon |
| `colorWarningBg` | `#F8F3E6` | Warning banner background |
| `colorSuccess` | `#28A745` | Success icon |
| `colorSuccessBg` | `#EAF6EC` | Success banner background |
| `colorError` | `#D50032` | Error stroke, error text, required asterisk, badge |
| `colorErrorBg` | `#FBE6EB` | Error banner background |

### Overlay

| Token | Value | Usage |
|---|---|---|
| `overlayDark` | `#000000CC` (80%) | QR scanner scan area mask |
| `overlayMedium` | `#00000066` (40%) | Generic modal/bottom sheet backdrop |
| `overlayLight` | `#FFFFFF40` (25%) | Icon background on dark camera screens |

---

## 3. Typography

**Font family:** `Roboto` (all UI text)  
**System font (device UI only):** `SF Pro` — used exclusively in keyboard, status bar, and system popups. Do not use in app UI.

### Type Scale

| Style token | Size | Weight | Line Height | Usage |
|---|---|---|---|---|
| `headingBold28` | 28px | 700 Bold | 36px | Page hero titles (rare) |
| `headingBold24` | 24px | 700 Bold | 32px | Title Section label |
| `headingBold20` | 20px | 700 Bold | 28px | App bar title, popup image label |
| `headingBold18` | 18px | 700 Bold | 24px | Section header, Title Section simple |
| `bodyBold16` | 16px | 700 Bold | 24px | Popup title, notification title, key-value value (emphasis) |
| `bodyMedium16` | 16px | 500 Medium | 24px | Button label (large), tab active, chip active, nav button |
| `bodyRegular16` | 16px | 400 Regular | 24px | Input value, body text, notification description, key-value |
| `captionMedium14` | 14px | 500 Medium | 18px | Button label (medium) |
| `captionRegular14` | 14px | 400 Regular | 18px | Floating label, error message, tagline text, item sub-label |
| `labelMedium12` | 12px | 500 Medium | 16px | Button label (small) |
| `labelRegular12` | 12px | 400 Regular | 16px | Notification timestamp, badge count sub-text |
| `otpDigit` | 20px | 500 Medium | 28px | OTP input digit |
| `priceLarge` | 20px | 700 Bold | 28px | Price input value (large) |
| `priceMedium` | 16px | 700 Bold | 24px | Price input value (medium) |

**Figma style path format:** `ROBOTO/{Group}Group/{styleName}` — e.g. `ROBOTO/BodyGroup/bodyBold16`

---

## 4. Spacing

| Token | Value | Typical usage |
|---|---|---|
| `space-2` | `2px` | — |
| `space-4` | `4px` | Label horizontal padding (floating label), badge horizontal padding |
| `space-8` | `8px` | Button gap in split-row, bottom action bar top padding, badge horizontal padding (large) |
| `space-12` | `12px` | Price input (medium) corner radius alias |
| `space-16` | `16px` | Standard component padding, page margin, card padding |
| `space-24` | `24px` | Button horizontal padding |
| `space-32` | `32px` | Large component internal spacing |
| `space-40` | `40px` | Bottom action bar bottom padding (safe area buffer) |
| `space-48` | `48px` | Status bar, button heights (large), header inner content |

---

## 5. Border Radius

| Token | Value | Usage |
|---|---|---|
| `radius-4` | `4px` | Checkbox (unique — only component using this value) |
| `radius-5` | `5px` | Keyboard keys (system UI only) |
| `radius-10` | `10px` | Notification app icon |
| `radius-12` | `12px` | Price input (medium), system dialog |
| `radius-16` | `16px` | **Standard** — inputs, cards, popups, banners, snackbars, bottom sheets, sections |
| `radius-24` | `24px` | Notification container |
| `radius-9999` | `9999px` | Pill shape — buttons, chips, radio, toggle, badge dot, scroll indicator |
| `radius-100` | `100px` | Home indicator bar, drag handle |

---

## 6. Stroke / Border

| Token | Value | Usage |
|---|---|---|
| `strokeWidth` | `1pt` | All input borders, outlined buttons, card strokes |
| `strokeDefault` | `#DFDFDF` | Default state borders |
| `strokeFocus` | `#013D50` | Focus/active state — text input, radio, checkbox |
| `strokeFocusPrice` | `#026483` | Price input focus state |
| `strokeError` | `#D50032` | Error state borders |
| `strokePrimary` | `#013D50` | Outlined button stroke |

---

## 7. Icon Sizes

| Size | Usage |
|---|---|
| `16px` | Small button icon |
| `18px` | Medium button icon |
| `20px` | Checkbox check icon, quantity selector icon (medium) |
| `24px` | **Standard icon** — input trailing/leading, app bar actions, status icons, standard UI |
| `32px` | Camera controls, section icons |
| `48px` | Status icon circles (in popups), app notification icon slot |

---

## 8. Component Specs

### 8.1 Button

**Shape:** pill (`radius-9999`), horizontal padding `24px` on all sizes.

| Variant | Height | Text style | Icon size |
|---|---|---|---|
| Large | `48px` | `bodyMedium16` | `24px` |
| Medium | `40px` | `captionMedium14` | `18px` |
| Small | `32px` | `labelMedium12` | `16px` |

| Type | Fill | Stroke | Text color |
|---|---|---|---|
| Primary (filled) | `#013D50` | — | `#FFFFFF` |
| Outlined | `#FFFFFF` | `#013D50` | `#013D50` |
| Text / Link | transparent | — | `#007BFF` |
| Disabled (filled) | `#C0C0C0` | — | `#FFFFFF` |
| Disabled (outlined) | `#FFFFFF` | `#DFDFDF` | `#C0C0C0` |

---

### 8.2 Form Inputs

#### Input Field (Text)

| Property | Value |
|---|---|
| Height | `56px` |
| Width | `361px` (content width) |
| Corner radius | `16px` |
| Padding horizontal | `16px` |
| Border width | `1pt` |
| Label pattern | Floating label — overlaps top border (y=−9), bg=`#FFFFFF`, padding 4px h |

| State | Fill | Stroke | Label color | Text color |
|---|---|---|---|---|
| Default | `#FFFFFF` | `#DFDFDF` | `#A1A1A1` | `#333333` |
| Focus | `#FFFFFF` | `#013D50` | `#013D50` | `#333333` |
| Error | `#FFFFFF` | `#D50032` | `#A1A1A1` | `#333333` |
| Disabled | `#F5F5F6` | `#DFDFDF` | `#C0C0C0` | `#C0C0C0` |
| Filled | `#FFFFFF` | `#DFDFDF` | `#A1A1A1` | `#333333` |

- Placeholder: `#A1A1A1`, `bodyRegular16`
- Floating label: `captionRegular14`
- Required asterisk: `#D50032`
- Error message text: `#D50032`, `captionRegular14`, padding-top `8px`
- Leading icon slot: `24×24px`
- Trailing icon (arrow/barcode): `24×24px`, fill `#A1A1A1` or `#007BFF`

#### Search Field

| Property | Value |
|---|---|
| Large height | `56px` |
| Medium height | `48px` |
| Corner radius | `16px` |
| Padding horizontal | `16px` |
| Leading icon | `search`, `24×24`, fill `#A1A1A1` |
| Trailing icon | `cancel`, `24×24`, fill `#A1A1A1` (when filled) |

States: same fill/stroke as Input Field.

#### OTP Input

| Property | Value |
|---|---|
| Cell size | `56×56px` (square) |
| Corner radius | `16px` |
| Digit style | `otpDigit` — 20px Medium |
| Digit color | `#333333` |
| Cursor | stroke `#013D50` |

States: Default=`#DFDFDF`, Focus=`#013D50`, Filled=`#DFDFDF`, Error=`#D50032`.

#### Textarea

| Property | Value |
|---|---|
| Min height | `142px` |
| Max height | `168px` (with text) |
| Width | `361px` |
| Corner radius | `16px` |
| Padding | `16px` all sides |

States: identical to Input Field. No trailing icon.

#### Price Input

| Property | Large | Medium |
|---|---|---|
| Height | `56px` | `48px` |
| Corner radius | `16px` | `12px` |
| Focus stroke | `#026483` | `#026483` |
| Value text | `priceLarge` — 20px Bold | `priceMedium` — 16px Bold |
| Value color | `#026483` | `#026483` |
| Currency suffix | `#026483` | `#026483` |
| Label focus | `#026483` | `#026483` |

Note: Price Input uses `#026483` (primarySystem2) for focus and value color, NOT `#013D50`.

#### Radio Button

| Property | Large | Medium |
|---|---|---|
| Outer circle | `32×32px` | `24×24px` |
| Inner dot | `12×12px` | `8×8px` |
| Radius | `9999px` | `9999px` |
| Label style | `bodyRegular16` | `captionRegular14` |

| State | Outer fill | Outer stroke | Dot fill |
|---|---|---|---|
| Unselected | `#FFFFFF` | `#DFDFDF` | — |
| Selected | `#FFFFFF` | `#013D50` | `#013D50` |
| Disabled unselected | `#F5F5F6` | `#DFDFDF` | — |
| Disabled selected | `#F5F5F6` | `#C0C0C0` | `#C0C0C0` |

#### Checkbox

| Property | Value |
|---|---|
| Size | `32×32px` |
| Corner radius | `4px` ← unique, only component with this radius |
| Check icon | `check` 20×20px, fill `#FFFFFF` |
| Indeterminate icon | `remove` 20×20px, fill `#FFFFFF` |
| Label | `bodyRegular16`, `#333333` |

| State | Fill | Stroke | Check color |
|---|---|---|---|
| Unselected | `#FFFFFF` | `#DFDFDF` | — |
| Checked | `#013D50` | `#013D50` | `#FFFFFF` |
| Indeterminate | `#013D50` | `#013D50` | `#FFFFFF` |
| Disabled unselected | `#F5F5F6` | `#DFDFDF` | — |
| Disabled checked | `#C0C0C0` | `#C0C0C0` | `#FFFFFF` |

#### Toggle (named "Toogle" in Figma — misspelled)

| Property | Large | Medium |
|---|---|---|
| Track | `48×32px` | `40×24px` |
| Thumb | `24×24px` | `18×18px` |
| Radius | `9999px` | `9999px` |

| State | Track fill | Thumb fill |
|---|---|---|
| ON | `#013D50` | `#FFFFFF` |
| OFF | `#DFDFDF` | `#FFFFFF` |
| Disabled ON | `#C0C0C0` | `#FFFFFF` |
| Disabled OFF | `#F5F5F6` | `#C0C0C0` |

#### Quantity Selector

| Property | Large | Medium |
|---|---|---|
| Height | `40px` | `32px` |
| Width | `141px` | `125px` |
| Button size | `40×40px` | `32×32px` |
| Button radius | `9999px` | `9999px` |
| Icon (+/-) size | `24×24px` | `20×20px` |
| Icon fill | `#013D50` | `#013D50` |
| Value style | `bodyRegular16` | `captionRegular14` |

| State | Button fill | Button stroke | Value color |
|---|---|---|---|
| Default | `#FFFFFF` | `#DFDFDF` | `#333333` |
| Active | `#FFFFFF` | `#013D50` | `#333333` |
| Disabled | `#F5F5F6` | `#DFDFDF` | `#C0C0C0` |

---

### 8.3 Navigation

#### App Bar / Header

| Property | Value |
|---|---|
| Total height | `104px` |
| Status bar area | `48px` (top padding) |
| Content height | `48px` |
| Bottom gap | `8px` |
| Width | `393px` (full bleed) |
| Padding horizontal | `16px` |
| Back button | `48×48px` hit area |
| Title style | `headingBold20`, centered |

Themes:

| Theme | Background | Title color | Icon color |
|---|---|---|---|
| Colored | `#013D50` | `#FFFFFF` | `#FFFFFF` |
| White | `#FFFFFF` | `#333333` | `#333333` |
| Transparent | transparent | `#333333` | `#333333` |

#### Bottom Tab Bar

| Property | Value |
|---|---|
| Height | `48px` |
| Tab text style | `bodyRegular16` (inactive) / `bodyMedium16` (active) |
| Inactive fill | `#FFFFFF` |
| Inactive text | `#333333` |
| Inactive stroke | `#DFDFDF` |
| Active stroke | `#026483` |
| Active text | `#026483` |

#### Bottom Sheet Header

| Property | Value |
|---|---|
| Height | `73px` |
| Drag handle | `56×5px`, fill `#333333`, radius `100` |
| Handle position | Centered horizontally at top |

#### Chips

| Property | Value |
|---|---|
| Height | `40px` |
| Radius | `9999px` (pill) |
| Padding horizontal | `16px` |
| Default stroke | `#DFDFDF` |
| Active stroke | `#026483` |
| Active icon/text | `#026483` |

#### Title Section

| Variant | Height | Background | Title style |
|---|---|---|---|
| Simple | `24px` | transparent | `headingBold18`, `#333333` |
| With ViewAll | `24px` | transparent | + "Xem tất cả" link `#007BFF` |
| Expandable | `48px` | `#F5F5F6`, radius `16px` | `headingBold18` + chevron |

---

### 8.4 Status Components

#### Banner

| Variant | Background | Icon color | Icon name |
|---|---|---|---|
| Info | `#E6F2FF` | `#007BFF` | info |
| Warning | `#F8F3E6` | `#B58905` | warning |
| Success | `#EAF6EC` | `#28A745` | check_circle |
| Error | `#FBE6EB` | `#D50032` | error |

Common: radius `16px`, padding `16px`, icon `24×24px`, body `bodyRegular16`.

#### Snackbar

| Property | Value |
|---|---|
| Fill | `#FFFFFF` |
| Stroke | `#DFDFDF` |
| Corner radius | `16px` |
| Padding | `16px` |
| Body text | `bodyRegular16`, `#333333` |
| Close icon | `close`, `24×24px`, `#333333` |
| Action link | `bodyRegular16`, `#007BFF` |

Variants: basic / with 1 action / with 2 actions.

#### Tagline (Status Pill)

| Variant | Fill | Text color |
|---|---|---|
| Default | `#F5F5F6` | `#A1A1A1` |
| Information | `#E6F2FF` | `#007BFF` |
| Pending | `#F8F3E6` | `#B58905` |
| Success | `#EAF6EC` | `#28A745` |
| Reject | `#FBE6EB` | `#D50032` |

Common: radius `9999px`, padding 8px horizontal, text style `captionRegular14`.

#### Badge

| Variant | Size | Fill | Text |
|---|---|---|---|
| Dot | `8×8px` | `#D50032` | — |
| Number (1–9) | `18×18px` | `#D50032` | white, 14px Regular |
| 99+ | `18×32px` (pill) | `#D50032` | white, 14px Regular, padding 4px |

Radius: `9999px` for all.

#### Progress Indicator (Step)

| State | Fill | Number text | Icon |
|---|---|---|---|
| Pending | `#F5F5F6` | `#C0C0C0`, 16px Bold | — |
| Active | `#013D50` | `#FFFFFF`, 16px Bold | — |
| Done | `#FFFFFF` | — | `check_circle` filled, `#013D50` |

Size: `32×32px` circle, radius `9999px`.

#### Carousel Indicator

| State | Shape | Size | Fill |
|---|---|---|---|
| Active | Rounded rect | `12×6px` | `#013D50` |
| Inactive | Circle | `6×6px` | `#A1A1A1` |

#### Loading

| Variant | Description |
|---|---|
| Full-screen | White card overlay + brand spinner (3-color: `#E6ECEE` + `#EF4429` + `#013D50`) + "Loading..." white text |
| Inline | Same spinner + "Loading..." `#333333` text |
| Error state | "Đã có lỗi xảy ra" `#A1A1A1` + "Thử lại" link `#007BFF` |

---

### 8.5 Bottom Action Bar

**Standard bar:** `393×96px` (single button) or `393×152px` (double stacked buttons).

| Property | Value |
|---|---|
| Width | `393px` |
| Padding top | `8px` |
| Padding bottom | `40px` (absorbs safe area) |
| Padding horizontal | `16px` |
| Fill (default) | `#FFFFFF` |
| Corner radius | mixed — top corners `16px`, bottom corners `0` (bottom sheet style) |

Layouts:
1. **Single primary button** — full width, 48px, fill `#013D50`
2. **Two stacked buttons** — primary (48px) + outline (48px), 8px gap, total `152px`
3. **Two side-by-side buttons** — each `176.5px` wide, 8px gap, height `48px`, total `96px`
4. **No background** — same layouts, transparent wrapper
5. **Dark screen (QR / camera)** — icon + label action items (`48px` circular bg `#FFFFFF40`)
6. **Keyboard open** — compact `64–120px` header floating above keyboard

Camera shutter button: `72×72px`, outer `fill=#F5F5F6`, inner `56×56` fill=`#FFFFFF` stroke=`#DFDFDF`.

---

### 8.6 Popups / Dialogs

**Container:** width `361px`, radius `16px`, fill `#FFFFFF`, padding `16px`.

| Element | Spec |
|---|---|
| Title text | `bodyBold16`, `#333333`, centered |
| Description text | `bodyRegular16`, `#333333`, centered |
| Status icon | `48×48px` circle, fill `#FFFFFF`, radius `9999px` |
| Alert icon | `48×48px`, fill `#EF4429` (secondary — destructive alert) |
| Warning icon | fill `#B58905` |
| Close button | `32×32px` icon `close`, fill `#333333`, inside `64×64px` container |

Variants:
1. Text + 2 side-by-side buttons (208px)
2. Icon alert + 2 side-by-side buttons (120px — compact)
3. Full image 3:4 + close (481px)
4. Text + 2 stacked buttons (264px)
5. Image 16:9 (203px) + text + 2 side-by-side buttons (347px)
6. Image 16:9 (203px) + text + 2 stacked buttons (403px)

**Button sizes in popups:** always `48px` height, pill radius, left=outline stroke `#013D50`, right=primary fill `#013D50`.

---

### 8.7 Overlay

| Variant | Fill | Usage |
|---|---|---|
| QR Scanner | `#000000CC` full screen + `361×361px` cutout (radius `16px`) | QR code scan area mask |
| Generic | `#00000066` | Bottom sheet / modal backdrop |

QR subtext: `bodyRegular16`, `#FFFFFF`, centered, 16px padding.

---

### 8.8 Notifications (In-App)

| Property | Value |
|---|---|
| Container | `361×80px`, radius `24px`, fill `#00000001` (transparent), padding `16px` |
| App icon slot | `48×48px`, radius `10px` |
| Title | `bodyBold16`, `#FFFFFF` (white — on dark backgrounds) |
| Timestamp | `labelRegular12`, `#FFFFFF`, right-aligned |
| Description | `bodyRegular16`, `#FFFFFF` |

Note: Text is white — notifications display over dark overlays or lock screens.

---

## 9. Device System Reference (System UI — do not override)

These are Figma reference components for device chrome. Developers should use native OS APIs for these, not custom implementation.

| Component | Key Specs |
|---|---|
| Status Bar | 48px height, time=SF Pro 18px Semibold, icons `#333333` |
| Home Indicator | 34px container, bar 134×5px fill `#000000` radius 100, horizontally centered |
| Keyboard (QWERTY) | 393×291px (no suggestions), 393×336px (with suggestions) |
| Keyboard keys | 31.5×42px letter keys, radius `5px`, primary `#FFFFFF`, modifier `#ADB3BC` |
| Keyboard font | SF Pro Text, 25px (keys), SF Pro 17px (suggestions) |
| Keyboard bg | `#D1D5DBE5` |
| Auto-suggestion bar | 39px height |
| Home indicator container | 375×34px, bar 134×5px at x=121, y=21 |

---

## 10. Z-Index / Layering Order (Top → Bottom)

1. System overlays (alerts, permissions) — device native
2. Notifications (in-app)
3. Popups / Dialogs
4. Snackbar / Toast
5. Bottom Sheet (with overlay behind)
6. Overlay backdrop (`#00000066`)
7. Bottom Action Bar (sticky)
8. App Bar / Header (sticky top)
9. Page content
10. Page background (`#F5F5F6`)
