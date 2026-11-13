# Diag This

Hands-on automotive diagnostic training and ASE test prep that runs entirely in the browser. There's no build step and nothing to install: every page is a single self-contained HTML file.

## Pages

| File | What it is |
|---|---|
| `index.html` | **Garage** home screen. A 3D engine bay (three.js) and the jobs board. Tap a part to open its lab. |
| `quick.html` | **Quick Diagnosis.** A five-job shift: run tests, make the call, beat the clock. |
| `fan.html` | **Cooling Fan Circuit Lab.** A real circuit model with a rotary-dial DMM and back-probing at the battery, fuse/relay box, fan connector, ground and ECM. Seven hidden faults. |
| `ign.html` | **Ignition Circuit Lab.** Coil-on-plug circuit, 2-channel scope with secondary kV pickups, coil swap, spark test. Six hidden faults. |
| `ase.html` | **ASE Prep A1–A8.** 537 original ASE-style questions, study by content area, timed exam simulation weighted like the real tests, smart review, and bookmarks. |

Career stats and ASE progress are saved in the browser's `localStorage` (`ce_career`, `ce_ase2`). Nothing is sent to a server.

## Run it

Open `index.html` in a browser. To serve it locally:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

### Host it on GitHub Pages

Settings → Pages → Source: **Deploy from a branch** → Branch: `main`, folder `/ (root)`. The site will be at `https://<user>.github.io/Diag-This/`.

Only `index.html` loads anything from the internet: three.js from cdnjs and Google Fonts. Every other page works offline except for fonts.

## Adding ASE questions

Questions live in `ase.html` inside the `Q` array (and the `Q.push(...)` blocks after it). Each question looks like this:

```js
{t:'A6', a:'B', q:'Question text?', o:['Choice 1','Choice 2','Choice 3','Choice 4'], c:1, w:'Why the answer is right.'}
```

- `t`: test (`A1`–`A8`). `a`: content-area letter. Must exist in that test's `areas` in `TESTS`.
- `o`: exactly four choices. `c`: index (0–3) of the correct one. Choices are shuffled in the app, except Technician A/B questions, which keep their order.
- Exam simulation draws questions in proportion to each content area's scored-question count, so keep every area stocked with at least as many questions as the real test uses.

Content areas and scored-question counts come from ASE's 2025 Automobile Study Guide. Check ase.com for current specs. The questions are original and are not actual ASE test items.

## Adding a fault to a lab

- **Fan lab (`fan.html`):** add an entry to `FAULTS` (complaint, correct repair id, explanation), then handle it in `solve()` (node voltages and current) and `edges()` (resistance paths used by the ohmmeter).
- **Ignition lab (`ign.html`):** add to `FAULTS`, then update `wave()` (primary signals) and `kv()` (secondary waveform), and add the codes in `codes()`.
