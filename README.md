# PDOS Viewer

A standalone, English-language viewer for orbital-resolved projected density of states. Open `index.html` in a browser or publish it on GitHub Pages. No installation, Python, API key, server, or external JavaScript library is required.

## Quick start

1. Open <a herf ="https://meenarittiruam2022.github.io/PDOS-Viewer/"> https://meenarittiruam2022.github.io/PDOS-Viewer/ </a> and click **Load PDOS files**.
2. Select multiple `.txt` or `.csv` files. Up/down files are paired from their names.
3. Select datasets and orbitals for the active panel. **All 5 d orbitals** shows individual curves; **Sum d** shows their sum and calculates its band center.
4. Use **Add** to copy the current panel, then edit its title, datasets, orbitals and Y limit. Set **Columns** to arrange panels.
5. Set the shared **Energy Min/Max**. Each panel's **Y limit (±)** is independent: 5 means −5 to +5; blank means automatic symmetric limits based on that panel's peaks within the energy window.
6. Choose the global spin filter. Spin-up is solid and positive; spin-down is dashed and negative for display.
7. View the active panel's band-center table and click **Export XLSX**. The exported columns, row order and five-decimal values match the table. A Settings sheet records the panel, energy window and calculation method.
8. Export **SVG** for a vector figure, or set **PNG DPI** and **Width (cm)** before exporting PNG. Height follows the figure's aspect ratio; PNG includes DPI metadata.

The page includes the same usage and format instructions. Reloading clears loaded files and panel settings. Export before closing when needed. The release contains no embedded research data.

## File naming

Use `Name.EXT`, for example:

- `PDOS-Example-Demo-up.txt`
- `PDOS-Example-Demo-dw.txt`
- `PDOS-Example-Demo-up.csv`
- `PDOS-Example-Demo-dw.csv`

`SPIN` must be `up` or `dw`. `ATOM` must not contain a hyphen; `SYSTEM` may contain hyphens. Use identical capitalization for paired atom/system names. Loading another file for an existing atom/system/spin replaces that channel, including when switching from TXT to CSV.

## TXT: spaces or tabs

A `.txt` file must contain whitespace-separated columns: one or more spaces or tabs. `.dat` is accepted using the same format.

```text
#Energy s py pz px dxy dyz dz2 dxz dx2 tot
-1.0    0.10 0 0 0 0.20 0.10 0.30 0.10 0.20 1.00
 0.0    0.05 0 0 0 0.10 0.05 0.15 0.05 0.10 0.50
 1.0    0.02 0 0 0 0.04 0.02 0.06 0.02 0.04 0.20
```

## CSV: commas

A `.csv` file must contain comma-separated columns. Semicolon-separated files and decimal commas are not supported. Quoted fields are supported, but multiline fields are not. Use UTF-8 text.

```csv
Energy,s,py,pz,px,dxy,dyz,dz2,dxz,dx2,tot
-1.0,0.10,0,0,0,0.20,0.10,0.30,0.10,0.20,1.00
0.0,0.05,0,0,0,0.10,0.05,0.15,0.05,0.10,0.50
1.0,0.02,0,0,0,0.04,0.02,0.06,0.02,0.04,0.20
```

These examples are synthetic and demonstrate formatting only. Paired TXT and CSV examples are in `examples/`. For these three-point examples, use **Full range** or set Min = −1, Max = 1 before calculating band centers. Load either format; loading both replaces the matching channels.

## Column rules

- The first column must be `Energy` or `#Energy` in eV. At least two numeric rows are required, with strictly increasing, non-duplicate energies.
- Supported orbital columns: `s`, `py`, `pz`, `px`, `dxy`, `dyz`, `dz2`, `dxz`, `dx2`, `tot`. Use any subset and order after Energy. Headers must be unique.
- `dz2` is d(z²); `dx2` is d(x²−y²).
- An H-s file may use only `Energy s` (TXT) or `Energy,s` (CSV).
- Sum d requires all five d columns. Sum p requires all three p columns. Missing orbitals are skipped and reported.
- `tot` is read from the input, not recomputed.
- Blank rows and # comment lines are ignored. Empty numeric fields, NaN, infinity and inconsistent column counts are rejected. E/D scientific notation is accepted.
- Use a decimal point. No thousands separators or units in numeric cells.
- PDOS is assumed to have units of states/eV. Use consistent signs in each spin channel. Positive and already-negative down-spin values are supported.

## Energy reference and band centers

The **Energy is already referenced to E_F = 0** checkbox changes the label and reference line only. It does not shift the input energy. Subtract each system's Fermi energy beforehand if required. There is no smoothing or normalization.

For each selected orbital, the center over the selected energy window is:

`center = integral(E * rho(E)) / integral(rho(E))`

Positive PDOS magnitudes are used for both spins. Integration is exact for a piecewise-linear PDOS interpolation, including interpolated window boundaries. No extrapolation is used: **Incomplete range** means the requested window exceeds the available data. **Zero PDOS** means the denominator is zero.

The combined up/down center uses the sum of both first moments divided by the sum of both integrated PDOS areas. It is not the arithmetic average of the two centers. Both channels must be available and selected for a combined value. The spin filter affects all plots and the active-panel table. The plotted Y limit does not affect integration.

For a d-band center, choose **Sum d**. Individual d-orbital centers are reported independently; they are not added to Sum d again. Use a consistent energy window and reference when comparing systems.

## Publish on GitHub Pages (free public repository)

1. Sign in at https://github.com and create a **new public repository** named, for example, `pdos-viewer`.
2. Extract the release ZIP. In the repository, choose **Add file → Upload files**. Upload `index.html` and `README.md` at the repository root. The examples folder is optional. Commit the upload to `main`.
3. Do not upload only the ZIP. Do not place `index.html` inside an extra release folder. The root must contain `index.html` with exactly that lowercase filename.
4. Open **Settings → Pages**.
5. Under **Build and deployment → Source**, select **Deploy from a branch**.
6. Select branch **main**, folder **/(root)**, and click **Save**.
7. Check the **Actions** tab for the Pages deployment. Return to Settings → Pages and use the published site link. For a project repository named `pdos-viewer`, the usual URL is `https://YOUR-USERNAME.github.io/pdos-viewer/`.
8. Publishing may take up to 10 minutes. After deployment, open the site and test loading files, panel selection and exports.

No build command or Streamlit wrapper is needed. GitHub Pages is available with GitHub Free for public repositories. A custom domain is optional.

### Update the website

Replace `index.html` in the same repository and branch, then commit. Wait for the Pages deployment to finish. If an old version remains visible, hard-refresh the browser (Cmd+Shift+R on Mac / Ctrl+Shift+R on Windows).

### Troubleshooting

- **404:** confirm root-level `index.html`, branch `main`, folder `/(root)`, completed deployment and the correct repository name in the URL.
- **Old content:** check the Actions run and refresh the browser cache.
- **No data plotted:** check the filename, delimiter, selected datasets/orbitals and energy limits.
- **Incomplete range:** select a window covered by both spin files.
- **PNG too large:** lower DPI/width or use SVG. Browser memory limits apply to large multi-panel figures.

### Data handling

The viewer reads selected PDOS files locally in the browser. It has no upload endpoint or analytics code. Files selected through the viewer are not sent to GitHub by this code. Files committed to the public repository, including examples, are public. GitHub serves the website and may keep ordinary hosting request logs. Publish only data you intend to share.

### Official GitHub documentation

- https://docs.github.com/en/pages/quickstart
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

Deployment instructions checked on 20 September 2026. This package is prepared for publishing; it has not been deployed to your GitHub account.
