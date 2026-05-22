---
name: ig-story-fixture
description: Creates premium, realistic Instagram Story prompts for tennis club fixture announcements. Can use fixture data produced by fixture-aat.
---

# IG Story Fixture

## Goal

Create a highly specific image-generation prompt for a premium Instagram Story announcing tennis club fixtures.

The skill outputs a prompt ready to generate an image. It does not invent fixture data.

## Required Inputs

Before creating the final prompt, make sure the user provides:

1. Background photo of the tennis club / court
2. Club logo
3. Fixture data

Fixture data can be provided as:

- Manual list of matches
- Output from the `fixture-aat` skill

## fixture-aat Integration

If the user asks to extract fixtures automatically, first use `fixture-aat`.

Then use the extracted fixture data as the input for this skill.

Expected fields:

- date
- time
- category
- matchup
- local / visitor condition, if available

Do not invent missing fixtures.

## Output Rules

Return only the final image-generation prompt.

Do not generate the image unless the user explicitly asks.

Do not recreate or redesign the logo.

Use the provided logo and background photo.

Preserve realism.

Avoid anything that looks obviously AI-generated.

## Default Style

- Instagram Story
- 9:16 vertical
- 1080x1920
- premium tennis club aesthetic
- realistic photography
- luxury sports branding
- clean hierarchy
- mobile-first readability

## Prompt Template

Create an ultra-realistic premium Instagram Story design for a tennis club fixture announcement.

The final result must look authentic, realistic, and professionally designed by a real sports club media designer.

It must NOT look obviously AI-generated.

Avoid:
- fake HDR
- exaggerated lighting
- over-sharpening
- neon colors
- plastic-looking typography
- unrealistic reflections
- excessive glow
- cluttered effects
- fake generated courts

Format:
- Instagram Story
- Vertical 9:16
- Resolution 1080x1920
- Mobile-first readability

Background:
Use the uploaded real tennis club / clay court photo as the main background.
Preserve the real court, lighting, imperfections, and photographic feel.
Apply only subtle cinematic enhancement.
Do not replace the background.

Logo:
Use the uploaded official club logo.
Place it centered near the top.
Preserve its original proportions.
Do not redraw, redesign, or reinterpret the logo.

Panel:
Create a large centered information panel with rounded corners.

Use the dark navy blue from the “C” and “S” of the uploaded logo as the panel color.

Panel style:
- semi-transparent navy overlay
- light opacity
- glassmorphism effect
- subtle blur
- soft reflections
- refined premium look
- not fully opaque
- background still visible through the panel

The panel should occupy around 65–75% of the story height.

Fixture content:
Use exactly this fixture data:

{{FIXTURE_DATA}}

Layout:
Group matches by date.

For each date:
- show date centered in small uppercase elegant text
- add thin horizontal lines around the date

For each match:
- time on the left
- category as the largest bold condensed text
- matchup below category in smaller uppercase text

Typography:
Use premium condensed sports typography.
Use strong hierarchy and clean spacing.

Hierarchy:
1. Category: largest
2. Matchup: medium
3. Date: small
4. Time: left-side label

Visual details:
- subtle divider lines between matches
- elegant spacing
- balanced composition
- realistic opacity
- clean editorial layout
- no overcrowding

Bottom message:
At the bottom outside the panel, add a handwritten white script message:

“Muchos éxitos a todos los equipos”

Final mood:
The story should communicate:
- premium tennis culture
- competition
- elegance
- club identity
- realistic social media branding

## Recommended Workflow

1. If the user wants automatic fixture extraction, first use `fixture-aat`.
2. Extract only the requested categories.
3. Present the extracted fixture list to the user for approval.
4. Ask the user for:
   - background photo
   - club logo
5. After approval and assets are provided, generate the final Instagram Story prompt.
6. Generate the image only if the user explicitly asks for it.

Do not skip the approval step when fixture data comes from an external source.
Do not invent missing fixture data.
Do not generate the image before the user confirms the extracted fixtures.

## Defaults

### Bottom Message

If no bottom message is provided, use:

“Muchos éxitos a todos los equipos”

---

### Panel Style Defaults

If no panel styling is specified:

- use semi-transparent navy overlay
- use subtle glassmorphism
- keep low opacity
- preserve background visibility
- use soft blur only
- avoid aggressive reflections

---

### Color Defaults

If logo color extraction fails:

- use dark navy blue
- avoid pure black
- avoid saturated blue
- maintain premium sports aesthetic

---

### Typography Defaults

Use:

- uppercase typography
- condensed sports sans serif
- clean hierarchy
- strong readability on mobile devices

If typography is not specified:

- prioritize ATP Finals / Wimbledon inspired aesthetics

---

### Fixture Layout Defaults

Group fixtures by date automatically.

For each date:
- center the date
- add thin divider lines

For each match:
- left-align time
- prioritize category readability
- keep matchup smaller than category

---

### Spacing Rules

If fixture count increases:

- reduce vertical spacing proportionally
- preserve readability
- never crop text
- never overlap matches
- keep safe margins near screen edges

If there are too many fixtures:
- reduce panel padding slightly
- reduce category font size proportionally
- preserve visual hierarchy

---

### Background Defaults

If the background photo is visually busy:

- increase panel opacity slightly
- add subtle dark gradient overlay behind panel only
- preserve realism

Do not:
- replace the background
- generate synthetic courts
- heavily blur the photo

---

### Realism Rules

Always prioritize realism over visual spectacle.

The final result should resemble:
- real Instagram sports branding
- luxury tennis club media
- authentic photography

Avoid:
- fake AI aesthetics
- excessive cinematic effects
- unrealistic textures
- over-sharpening
- exaggerated HDR
- glowing edges
- artificial lens flares

---

### Image Generation Behavior

Do not generate the image automatically.

Only generate the image if the user explicitly requests:
- “generate it”
- “create the image”
- “render the story”
or similar instructions.

Otherwise, output only the final image-generation prompt.