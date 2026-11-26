# axel

Mosaic axolotl logo.

| File | Description |
|---|---|
| `logo.svg` | Original auto-traced logo (dark palette) |
| `logo-clean.svg` | Original dark palette, smoothed lines, thicker strokes |
| `logo-light-clean.svg` | Lighter palette, smoothed lines, thicker strokes |
| `logo-clean-animated.svg` | `logo-clean.svg` with a CSS float, blink, and gold-spot shimmer |

<img src="logo-clean-animated.svg" width="256">

## Previews

| Size | `logo-clean.svg` | `logo-light-clean.svg` |
|---|---|---|
| 256px | <img src="logo-clean.svg" width="256"> | <img src="logo-light-clean.svg" width="256"> |
| 128px | <img src="logo-clean.svg" width="128"> | <img src="logo-light-clean.svg" width="128"> |
| 64px | <img src="logo-clean.svg" width="64"> | <img src="logo-light-clean.svg" width="64"> |
| 32px | <img src="logo-clean.svg" width="32"> | <img src="logo-light-clean.svg" width="32"> |
| 16px | <img src="logo-clean.svg" width="16"> | <img src="logo-light-clean.svg" width="16"> |

## Reproducing the line cleanup

Paste this prompt into Claude Code with the SVG to clean in the working directory:

```
Clean up the lines in <file>.svg and write the result to <file>-clean.svg.
The SVG is an auto-traced logo: each <g> layer has one <path> with a solid
fill. The dark line color is the fill shared by the base outline layer and
the eyes/mouth/tail strokes layer.

1. Smooth the lines. The traced edges are wobbly. For every subpath, sample
   points along it, resample at ~1.5 unit spacing, apply Taubin smoothing
   (lambda 0.5, mu -0.53) with iterations = clamp(perimeter * 0.3, 3, 400)
   so small spots and glints barely change, simplify with Douglas-Peucker
   (tolerance 0.35), and refit as cubic Béziers (Catmull-Rom). Keep each
   layer's shapes aligned with the others.

2. Thicken the lines. On the two dark line-color paths, add a stroke in the
   same color: stroke-width="3" stroke-linejoin="round".

3. Remove the seams. Smoothing layers independently leaves hairline gaps
   where the dark base shows through as slivers around spots and gill
   dashes. On every other (colored) path add a stroke in its own fill color:
   stroke-width="2.5" stroke-linejoin="round". Drop any subpath in the dark
   line layers smaller than 4 units across.

Render with Inkscape (`inkscape <file> --export-type=png -w 1000`, plus
zoomed --export-area crops) and check for leftover slivers or wobbles.
```

Tuning:

- Line weight: the stroke width in step 2. Use `2` for subtler lines, `4`–`5` for bolder.
- Slivers still visible: raise the step 3 stroke width to `3`.
- Smoothing: raising the strength above `0.3` makes little difference. Uneven line thickness comes from the drawing itself, so it needs a centerline redraw rather than more smoothing.
