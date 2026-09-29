# Musical nebula

A copy of [Nebulae](https://github.com/franpiaggio/nebulosas) with a timeline: a white scan line sweeps the sky from left to right, drawn like a live signal, and the brightest stars play a [Strudel](https://strudel.cc) sequence as the line crosses them.

Everything from the original is still here (nebula types, transform, palettes, shapes, star editing, painting). The music is new, and since the stars are the instrument, the Stars tab comes first.

## The music

Press the round **play** button under the image, or the **space bar**, to play and pause. Pausing freezes the line where it is; playing again continues from there. The timeline next to the button shows where the line is in the arc, and clicking or dragging it jumps elsewhere (arrow keys work too when it has focus). The first play loads Strudel and a piano sample set from the internet, so it needs a connection.

- Play starts gently: the pedal swells in alone while the line waits three seconds off the left edge, then the stars join one by one as the line reaches them.
- One sweep of the line is one arc of the sequence: 8 cycles at 18 cycles per minute, about 27 seconds.
- **The most luminous stars sound**, ranked by total light (core plus halo): by default the top 14, adjustable with **Stars that sound**. In the Lagoon the ten stars with a diffraction cross come first, then the large round ones, which ring softer because presence follows size. There are three voices, chosen in the Music tab:
  - **Harmonics** (default): one note per star, with long reverb and delay, shaped by what the star looks like:
    - **height** picks the register on the chosen scale, from A4 at the bottom to D7 at the top;
    - **color** (core plus halo) shifts it: white stays, blue climbs two or three steps, red drops two. It also picks the timbre: blue rings bright with its octave, white and yellow a warm triangle, red a soft sine;
    - **size** sets loudness and how long it rings;
    - a star never repeats one of the last four notes: it takes the nearest free step, above if it sits higher than the star that played it, below if lower. Stars ringing within 1.5 seconds keep their vertical order.
  - **Piano notes**: one piano note per star, chosen exactly like the harmonics (height, color, scale, no recent repeats); bigger stars strike harder and ring longer.
  - **Piano chords**: a chord that starts an octave low and opens upward in 35 ms steps, with a soft sine halo. A star near the top plays the arc's opening chord and one near the bottom its resolution to D.
- **Background pedal**: a low drone (filtered sawtooths on the pedal note and its fifth in octave 2, plus a sine an octave up), with long attack and release, retriggered with overlaps so it never breaks. It can be turned off in the Music tab.
- **Key, scale and pedal are editable** in the Music tab:
  - **Key**: any of the twelve notes (D by default).
  - **Scale**: Major pentatonic (default), Minor pentatonic, Major, Dorian, Lydian, Mixolydian, Natural minor, Phrygian, Hirajoshi or Whole tone. The stars' notes are every note of the scale between A4 and D7, so a seven-note scale gives height finer steps. The piano chords transpose to the key.
  - **Pedal note**: "From the nebula's color" or a fixed note. In the automatic mode the page measures the dominant hue of the image after each render and spreads the scale's notes around the hue wheel, ordered by fifths from the key and starting at red (in D major pentatonic: red D, then A, E, B, F#); grey images keep the key's note. When the color changes, the pedal crossfades to the new note, shown next to the timeline.
- **Crossing the nebula.** Inspired by the climax pad of a second Strudel piece, where the filter opens as the spectrum grows: a sawtooth pad on the pedal's root, fifth, octave and ninth swells and opens with the amount of gas under the line (around 400 Hz in thin gas, 3 kHz in the core), and a sub-bass sine breathes where the gas is dense. Empty sky stays quiet. The gas per column is measured from the rendered image, so palettes, types and painting all change it.
- The star pulses and the line spikes like a signal at that height. Over the nebula the line's glow takes the gas color and widens with its density.
- In the Music tab: **Volume**, the voice, the pedal and **Stars that sound**.
- Add or erase spiked stars in the Stars tab to write more notes.

The sequence is the one in `buildSequence()`: twelve voicings in D, three openings, four cadences and two rhythms, picked at random each arc (`irand(...).segment(1)`), four voices with their own dynamics. Strudel builds the patterns and is queried at the star's position with `queryArc`; each event is played with `superdough` at an exact audio time, with the same envelopes, reverb, pan and delay as the original code.

If the tab goes to the background the animation pauses; when it comes back, the stars the line skipped stay silent instead of all ringing at once.

Strudel is loaded from jsDelivr (`@strudel/web@1.3.0`, AGPL-3.0-or-later).

## Opening it

Locally, double-click `index.html` in any modern browser. The first render takes a second or two.

To serve it instead:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## What you can do

The panel on the right has three tabs: **Nebula** (type, transform, color and shape), **Stars** (star field and click editing) and **Paint** (paint your own gas and dust).

**Type.** A dropdown that starts on the Lagoon, the fixed reconstruction of a real photo. The other five types are generated:

| Type | Inspired by | Features |
| --- | --- | --- |
| Planetary | Helix, Ring | Clumpy red shell, teal interior, dark knots on the rim |
| Bipolar | Butterfly | Two uneven lobes with bright edges and a dusty waist |
| Supernova remnant | Veil | Tangled filaments along a shell, broken into arcs |
| Pillars | Pillars of Creation | Dust columns lit on the side facing the light |
| Reflection | Pleiades | Streaked blue haze around a cluster of hot stars |

"New variant" generates another nebula of the same type. Each variant has a number, and the same variant always gives the same image.

**Transform.** Generated types have a free transform, like Photoshop's. "Transform on the image" puts a box around the nebula: drag inside it to move, drag a corner to resize, drag outside it (or the round handle on top) to rotate. Shift snaps the angle to 15°, and Enter or Esc finishes. The Rotation and Size sliders do the same from the panel, and "Reset" puts it back. While you drag you see a quick preview; on release the nebula is rebuilt exactly at its new place, so nothing gets cut at the edges.

**Color.** Seven palettes (Original, Hubble, Ice, Fire, Emerald, Violet, Mono) and a hue shift slider.

**Shape.** Warps the gas and leaves the stars alone: Swirl, Bulge, Turbulence and Mirror, with a strength slider.

**Stars.**
- Turn the Lagoon's original stars on or off.
- Add generated stars by kind: background, Sun-like, red dwarfs, blue giants and spiked stars with diffraction crosses.
- "New star field" replaces the field with a generated one.
- Click editing: when you open the Stars tab, Add mode is already on, so a click on the image adds a star. Erase removes the nearest one and View turns clicks off. Esc leaves any mode. On the Nebula tab, clicks never touch the stars.

**Paint.** Paint on the image with the mouse and each stroke turns into nebula when you let go. Brushes: Hydrogen (red), Oxygen (teal), Reflection (blue), Dust (darkens whatever is underneath, the Lagoon included) and an Eraser, with Size and Strength sliders, "Undo stroke" and "Clear painting". Painted gas follows the palette and hue shift like the rest of the nebula.

**At the bottom of the panel**: toggle the layers (gas, stars, grain), "Surprise me" for a random combination, "Save PNG", and "Back to the original Lagoon".

## How it works

### The Lagoon

It is not a photo. The image was measured outside the browser and fitted with simple mathematical functions. The `MODEL` object at the top of the script holds the result:

- `gas`: 6,500 Gaussian blobs `[x, y, sigma, R, G, B]`. Many have negative channels: they subtract light to form the dust and cancel each other out.
- `stars`: 1,734 stars, each with a core and a halo.
- `cutouts`: patches that clean up the gas under the ten brightest stars.
- `brightStars`: those ten stars, with their measured shape and diffraction spikes.

The script that did the fitting is not part of this project.

### The engine

`rasterEngine(model)` runs in a Web Worker built from its own source. If the browser doesn't allow workers, it runs on the page. The engine builds separate layers in `Float32Array`s: gas, stars and grain. Each render only combines them and applies the palette, which is why color changes are instant.

The steps of one render:

1. **Gas layer**: the Lagoon's, or one generated by the recipe of the chosen type. For generated types the recipe is sampled through the inverse of the transform (move, rotate, scale), so the result is exact at any transform. The layer is cached until the type, the variant or the transform changes.
2. **Shape**: if a warp is selected, each pixel samples the gas from somewhere else. The warp works on the summed image rather than on the blobs, because the signed blobs would stop cancelling out. Samples that land outside the frame are mirrored back inside, so warps never show the edge of the image.
3. **Color**: a color matrix or a gradient keyed to brightness, plus a soft curve that keeps highlights from burning out to white.
4. **Painting**: painted dust darkens the sky and painted gas is added to it, before the palette.
5. **Stars and grain** are added on top.

### Generated types

Each type is a recipe in `recipe(type, seed)` that returns, for any point, how much light it emits in three colors (hydrogen red, oxygen teal, reflection blue) and how much dust there is. The recipes use deterministic noise, so the same seed always gives the same nebula. The engine evaluates them on a half-resolution grid, interpolates, and adds a fine noise texture.

### Painting

Strokes go into a half-resolution mask with one channel per brush. When a stroke ends the mask goes to the worker, which reads it through a two-scale noise warp so edges turn billowy and ragged, blurs it at two radii for a core and a glow, breaks it up with texture and threads it with thin filaments. The result is a light layer and a dust layer at full resolution.

### Stars

Every star is one entry in a single list, whether it comes from the Lagoon, is generated, or was added by hand:

```
[x, y, sigma, R, G, B, haloSigma, haloR, haloG, haloB,
 spikeLength, spikeThickness, spikeAngle, spikeR, spikeG, spikeB]
```

The page builds the list and the worker only redraws it when it changes. Generated stars come from one sequence per kind, so raising a slider adds stars without moving the ones already there.

## Extending it

- **A new palette**: add it to `PRESETS` with gradient stops `[position, R, G, B]`, `gamma`, `hue` or a `stars` matrix, and add its swatch to the panel.
- **A new nebula type**: add a branch to `recipe()` that fills `d[0..3]` (red, teal, blue, dust) and a button with `name="type"` in the panel.
- **A new kind of star**: add it to `STAR_KINDS` and `makeStar()`, with its slider in the panel.

Internal ids (types, palettes, shapes, star kinds) are still the original Spanish words. The type id feeds the seed hash, so renaming it would change every variant. Saved PNG files get English names.

## Known limitations

- The resolution is fixed at 1536 × 859.
- If you turn off the Lagoon's stars, faint smudges remain where they were. They are leftovers of those stars inside the fitted gas.
- The Lagoon can't be moved, because there is no data outside the photo's frame.
- Some strong warps stretch star leftovers that remained in the gas.

## Layout

```
index.html                                the whole viewer (HTML, CSS and JS in one file)
referencia/nebulosa-canvas.original.html  the original reconstruction, untouched
```

`referencia/` keeps the file exactly as it arrived. With every control at its default, `index.html` renders the same image, pixel for pixel.
