# In The N.O. — Design System

Site palette and chart conventions. Companion to the charts README; any new chart page should
be checked against this file.

## Core brand palette

| Role | Hex | Notes |
|---|---|---|
| Paper (site background) | `#F4F0E8` | warm cream |
| Ink / accent | `#071523` | dark navy — headings, buttons, links, tag pills |
| Body copy | `#16232E` | softened navy, easier on long reads than full strength |
| Meta | `#5F6B77` | bylines, dates, read time, card excerpts, chart captions & axis labels |
| Gold | `#B88A2A` | masthead hairline, pull-quote borders — use sparingly |
| Text on navy | `#F4F0E8` | cream on buttons and filled chart segments, warmer than white |
| Hairlines / dividers | `#DCD5C6` | also chart gridlines, card borders, empty bar tracks |
| Card surface | `#FBFAF6` | slight lift off the paper; chart card background |

## Extended categorical set (charts)

Derived tones, harmonious with the warm paper — used where charts need more categories than the
core palette provides.

| Role | Hex | Used for |
|---|---|---|
| Terracotta | `#A65A45` | paid believers · "out of the league" · picks 11-14 · defensive-role bars · the −2 wall line |
| Slate | `#3E5C76` | starter line · position bars · late 1st · class-of-2022 pending triangles |
| Olive | `#6B7A48` | 2nd round |
| Warm gray | `#8A8175` | undrafted · "no yr-5 salary" |

## Standing mappings (keep consistent across all charts)

**Draft pedigree:** top 5 = ink `#071523` · picks 6-10 = gold `#B88A2A` · picks 11-14 = terracotta
`#A65A45` · late 1st = slate `#3E5C76` · 2nd round = olive `#6B7A48` · undrafted = warm gray `#8A8175`

**Semantics:** navy = the good outcome (second chances, cheap-flier fills, redemptions) ·
gold = pending / intermediate (and the star marker, sparingly) · terracotta = the bad outcome
(out of the league, the wall, paid believers as the contrast class)

**Reference lines:** starter line (+1) = slate dashed · star line (+3) = gold dashed ·
the −2 wall = terracotta dotted

**Markers:** small dot = one player · larger dot + ink ring = sustained · gold triangle =
pending (yr 5+ underway) · slate triangle = just completed yr 4

## Typography

- **Headings / chart titles:** Cardo, weight 700 (Google Fonts)
- **Body / labels / captions:** Inter (Google Fonts)
- Chart pages must load both via a Google Fonts link — iframes do not inherit site fonts.

## CSS variables (copy-paste block for new chart pages)

```css
:root{
  --paper:#F4F0E8; --card:#FBFAF6; --ink:#071523; --body:#16232E;
  --meta:#5F6B77; --gold:#B88A2A; --hair:#DCD5C6;
  --terra:#A65A45; --slate:#3E5C76; --olive:#6B7A48; --gray:#8A8175;
}
```
