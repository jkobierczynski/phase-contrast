# Phase Contrast Bench

A self-contained, interactive simulation of a phase-contrast microscope. Everything — markup, styling, and a from-scratch 2D FFT — lives in the one file: `index.html`.

## Made with Claude

Made using Claude Sonnet 5

## Screenshot

![phase-contrast](phase-contrast.jpg)

## How to use it

Double-click `index.html`, or drag it into any modern browser (Chrome, Firefox, Safari, Edge). No install, no server, no build step.

It works fully offline. The only thing that needs an internet connection is the Archivo / Source Sans 3 / IBM Plex Mono typefaces, loaded from Google Fonts — without a connection the page just falls back to your system's default fonts, and everything else works exactly the same.

## Is it actually computing this?

Yes. There's no server, no pre-rendered images, and no lookup table. The page includes a hand-written radix-2 Cooley–Tukey FFT (about 30 lines of vanilla JavaScript), and every time you move a control it re-runs the real optics calculation on the spot:

1. Forward FFT of the specimen's phase map.
2. For each illumination angle, shift that spectrum, apply the phase ring's mask (radius, width, X/Y offset, retardation, transmission), and inverse-FFT it.
3. Sum the resulting intensities across all angles and redraw the four panels.

That whole pipeline runs in roughly 50–100 ms, which is why it feels instant as you drag a slider. Nothing in the visuals — including the flat bright-field panel, or the halo/shadow artifacts you get from detuning the ring — is scripted; it all falls out of the live math.

## The four panels

| # | Panel | What it shows |
|---|-------|----------------|
| 01 | Specimen | The phase object's optical path length map — a false-colour picture of a structure that's otherwise invisible |
| 02 | Objective back focal plane | The actual diffraction pattern, with the phase ring overlaid so you can see the undiffracted beam converge onto it while the specimen's diffracted spatial frequencies spread out around it |
| 03 | Bright-field image | The image with the ring removed — should render essentially flat, since a pure phase object can't modulate intensity without it |
| 04 | Phase-contrast image | The image with the ring engaged — the same phase structure, now visible as brightness |

## Worth trying

- Switch specimen presets — **Eukaryotic cell** and **Epithelial sheet** are built from an irregular, textured cell model (organic membrane wobble, nucleus, nucleolus, scattered organelles) rather than simple circles, so they read like real phase-contrast micrographs.
- Drag the phase ring's **radius** or **X/Y offset** away from the condenser annulus (Phase Plate group) and watch panel 04 lose contrast, or pick up an asymmetric shadow — the same fault a centring telescope corrects on a real microscope. **Match ring to condenser** snaps it back into alignment.
- Toggle **On-axis point** vs **Annular** illumination in the Condenser group to compare a simple central phase spot against the real Zernike annular design.
- Flip the **Retardation** slider negative to switch from positive to negative phase contrast.

## Layout

The four instrument panels sit on the left; the control console sits to their right as a single scrollable column. Under about 980px of window width it reflows to panels-on-top, controls-below (spreading back out into a row of groups) so it stays usable on a laptop or tablet; under ~520px it collapses to one column for phones.
