# skills/ig-story-fixture/README.md

# ig-story-fixture

Generate premium Instagram Story prompts for tennis club fixture announcements.

This skill is designed to work together with `fixture-aat` and focuses on:
- realistic rendering
- premium sports branding
- elegant layouts
- authentic Instagram aesthetics
- non-AI-looking visuals

━━━━━━━━━━━━━━━━━━━
PURPOSE
━━━━━━━━━━━━━━━━━━━

This skill creates highly detailed image-generation prompts for:
- tennis club fixture announcements
- Instagram Stories
- premium sports social media content

The generated prompts are optimized for:
- realism
- luxury tennis aesthetics
- mobile readability
- clean editorial hierarchy

━━━━━━━━━━━━━━━━━━━
RECOMMENDED WORKFLOW
━━━━━━━━━━━━━━━━━━━

1. Use `fixture-aat` to extract fixtures
2. Filter desired categories
3. Present fixtures to the user for approval
4. Request:
   - background photo
   - club logo
5. Generate final Instagram Story prompt
6. Generate the image only if explicitly requested

Do not skip the approval step when using external fixture data.

━━━━━━━━━━━━━━━━━━━
SUPPORTED INPUTS
━━━━━━━━━━━━━━━━━━━

## Background Image

A real tennis court or club photo.

Recommended:
- clay court
- realistic photography
- sunset or golden hour
- authentic lighting
- natural imperfections

Do not use:
- fake AI-generated courts
- unrealistic renders

---

## Club Logo

Official club logo.

Recommended:
- PNG
- transparent background

The skill extracts the dark navy tone from the logo and uses it as the main panel color.

---

## Fixture Data

Fixture data can come from:
- manual input
- `fixture-aat`

Expected structure:

- date
- time
- category
- matchup

Example:

SÁBADO 24/05

14:15
CABALLEROS +30 2DA
SOLANA VS SAN LORENZO

17:45
CABALLEROS +19 INTERMEDIA
CUBA VS SOLANA

━━━━━━━━━━━━━━━━━━━
DESIGN PHILOSOPHY
━━━━━━━━━━━━━━━━━━━

The final result should feel:
- premium
- elegant
- cinematic
- authentic
- realistic
- socially believable

The generated story should resemble a real luxury tennis club Instagram Story designed by a professional media team.

It must NOT look obviously AI-generated.

━━━━━━━━━━━━━━━━━━━
REALISM RULES
━━━━━━━━━━━━━━━━━━━

Always prioritize realism over spectacle.

Avoid:
- fake HDR
- excessive glow
- over-sharpening
- unrealistic reflections
- artificial textures
- synthetic-looking courts
- plastic typography
- aggressive gradients
- exaggerated cinematic effects
- neon colors
- cluttered compositions

The story should feel:
- authentic
- premium
- socially realistic
- naturally designed

━━━━━━━━━━━━━━━━━━━
DEFAULT VISUAL STYLE
━━━━━━━━━━━━━━━━━━━

Format:
- Instagram Story
- Vertical 9:16
- 1080x1920

Visual style:
- premium sports branding
- luxury tennis aesthetic
- glassmorphism panel
- semi-transparent navy overlay
- elegant condensed typography
- ATP Finals inspired hierarchy
- Wimbledon inspired elegance

━━━━━━━━━━━━━━━━━━━
DEFAULTS
━━━━━━━━━━━━━━━━━━━

## Bottom Message

If no bottom message is provided, use:

“Muchos éxitos a todos los equipos”

---

## Panel Style Defaults

If no panel styling is specified:
- use semi-transparent navy overlay
- use subtle glassmorphism
- preserve background visibility
- use refined opacity
- use minimal blur
- avoid aggressive reflections

---

## Color Defaults

If logo color extraction fails:
- use dark navy blue
- avoid pure black
- avoid saturated blue

---

## Typography Defaults

Use:
- uppercase typography
- condensed sports sans serif
- clean spacing
- strong hierarchy
- mobile readability

If typography is not specified:
- prioritize ATP Finals / Wimbledon inspired styling

---

## Fixture Layout Defaults

Group fixtures by date automatically.

For each date:
- center the date
- add thin divider lines

For each match:
- left-align time
- prioritize category readability
- keep matchup smaller than category

---

## Spacing Rules

If fixture count increases:
- reduce spacing proportionally
- preserve readability
- never crop text
- never overlap text
- preserve safe margins

If there are many fixtures:
- reduce font size slightly
- reduce panel padding slightly
- preserve hierarchy

---

## Background Defaults

If the background photo is visually busy:
- slightly increase panel opacity
- add subtle dark overlay behind panel only
- preserve realism

Do not:
- replace the background
- generate synthetic courts
- heavily blur the image

━━━━━━━━━━━━━━━━━━━
OUTPUT BEHAVIOR
━━━━━━━━━━━━━━━━━━━

By default, this skill outputs:
- a complete image-generation prompt

It does NOT generate the image automatically.

Generate the image only if the user explicitly requests:
- “generate it”
- “create the image”
- “render the story”
or similar instructions.

━━━━━━━━━━━━━━━━━━━
INTEGRATION
━━━━━━━━━━━━━━━━━━━

Designed to integrate with:

- fixture-aat

Recommended pipeline:

fixture-aat → ig-story-fixture → image generation

━━━━━━━━━━━━━━━━━━━
EXAMPLES
━━━━━━━━━━━━━━━━━━━

# examples/manual-fixtures.json

{
  "input_mode": "manual",
  "fixtures": [
    {
      "date": "SÁBADO 24/05",
      "time": "14:15",
      "category": "CABALLEROS +30 2DA",
      "matchup": "SOLANA VS SAN LORENZO"
    },
    {
      "date": "SÁBADO 24/05",
      "time": "17:45",
      "category": "CABALLEROS +19 INTERMEDIA",
      "matchup": "CUBA VS SOLANA"
    }
  ]
}

---

# examples/fixture-aat-integration.json

{
  "workflow": [
    "use fixture-aat",
    "extract next weekend fixtures",
    "filter categories",
    "present fixtures to user",
    "wait for approval",
    "request logo and background",
    "generate premium IG Story prompt"
  ],
  "categories": [
    "CABALLEROS +19",
    "CABALLEROS +30"
  ]
}

---

# examples/high-density-layout.json

{
  "layout_behavior": {
    "many_matches": true,
    "reduce_spacing_proportionally": true,
    "reduce_font_size_slightly": true,
    "preserve_readability": true,
    "never_crop_text": true,
    "group_by_date": true
  }
}

---

# examples/realistic-rendering.json

{
  "realism_rules": {
    "avoid_ai_look": true,
    "preserve_real_photography": true,
    "avoid_fake_hdr": true,
    "avoid_excessive_glow": true,
    "avoid_over_sharpening": true,
    "avoid_plastic_typography": true,
    "maintain_authentic_instagram_branding": true,
    "prioritize_realistic_social_media_design": true
  }
}