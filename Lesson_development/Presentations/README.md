## Slides (Quarto)

The training slides are written in [Quarto](https://quarto.org) (`.qmd` files) and rendered as [reveal.js](https://revealjs.com) presentations that run in the browser. All decks share one look through `styles.scss`.

```text
Lesson_development/Presentations/
├── _quarto.yml            # project config: list of decks + reveal.js settings
├── index.qmd              # landing page with links to all decks
├── styles.scss            # shared theme (colours, typography, boxes, pills, layout helpers)
├── fonts.css              # @font-face rules for the brand fonts
├── fonts/                 # Aspekta and Space Grotesk (woff2) with their licences
├── 01-intro.qmd           # welcome, schedule, team, code of conduct
├── 02-lesson-example.qmd  # starter lesson deck showing the building blocks
├── img/                   # images used in the slides
└── .gitignore             # ignores the build output (_site/, .quarto/)
.github/workflows/slides.yml          # automatic build and publish
```

### Install

1. Install Quarto (version 1.4 or newer): <https://quarto.org/docs/get-started/>
2. Check the installation:

   ```bash
   quarto --version
   quarto check
   ```

Nothing else is needed: the Mermaid diagrams used in the slides are built into Quarto.

### Render and preview

Run these commands from the slides folder:

```bash
cd Lesson_development/Presentations

# Live preview in the browser, reloads automatically when you save a file
quarto preview

# Preview a single deck
quarto preview 02-lesson-example.qmd

# Build the whole site into _site/
quarto render
```

Open `_site/index.html` in a browser to see the built result. Inside a deck, press `F` for fullscreen, `S` for speaker view, `O` for the slide overview, and `?` for all shortcuts. To export a deck to PDF, open it with `?print-pdf` appended to the URL (for example `_site/02-lesson-example.html?print-pdf`) and print to PDF from Chrome.

### Add a new deck

1. Copy `02-lesson-example.qmd` to a new file, for example `03-my-lesson.qmd`.
2. Add the file to the `render:` list in `_quarto.yml`.
3. Add a link to it in `index.qmd`.
4. Use `##` for a new slide. The available building blocks (`.box`, `.pill`, `.row`, `.focus`, `.checks`, incremental bullets, Mermaid diagrams, speaker notes) are demonstrated in `02-lesson-example.qmd`.

### Change the look

Colours, fonts and box styles live in `styles.scss`, so a change there applies to every deck.

The colours follow the 4TU.ResearchData brand palette: Warm Orange `#F29100`, Fresh Blue `#B9DBEF` (slide background), Vibrant Orange `#FF6829`, Soft Grey `#969696`, Dark Grey `#3C3C3C` and Off White `#FBF5E9`. To keep the slides readable:

- All text is Dark Grey. Soft Grey is never used for text (only about 2:1 contrast on Fresh Blue), only for borders and decoration.
- Off White text is only used on Dark Grey (`.dark-box`).
- Vibrant Orange is for accents and large bold labels, not for body text.

The typography follows the brand guidelines, with the fonts included in `fonts/` so the slides also work offline:

- **Aspekta** for titles, introductions and body text (weight 300 for regular text and titles, 500 for bold).
- **Space Grotesk** for subtitles (`###` headings and the subtitle of a title slide), pills, the footer and the slide number, always in capitals.
- Line height is 140% for body text and about 100% for titles.

The font families, weights and line heights are variables at the top of `styles.scss` (`$font-aspekta`, `$font-weight-body`, ...). If the light weight is hard to read on a projector, set `$font-weight-body: 400`. Both fonts are licensed under the SIL Open Font License (see the `LICENSE-*` files in `fonts/`).

Code size is set once, in the variable `$code-font-size` at the top of `styles.scss` (default `.85em`). Increase it to make all code blocks bigger. Code colours come from `highlight-style: a11y` in `_quarto.yml`. A logo can be added by putting a file in `img/` and uncommenting the `logo:` line in `_quarto.yml`. The footer text is set in `_quarto.yml` under `footer:`.

### Automatic build and publishing

The workflow `.github/workflows/slides.yml` renders the slides on GitHub with Quarto and runs when:

- something in `Lesson_development/Presentations/` (or the workflow file itself) is pushed,
- the lesson site workflow **01 Build and Deploy Site** has finished successfully on `main` (so the slides are re-published after the site is rebuilt), or
- you start it manually from the **Actions** tab (**Build and publish slides → Run workflow**).

On every branch, the rendered site is attached to the run as a downloadable artifact called `slides`, so you can check a build before merging. On `main` only, the site is also published into the `slides/` folder of the `gh-pages` branch, next to the rest of the lesson site (existing files in `gh-pages` are kept).

The slides are then available at `https://<user-or-organisation>.github.io/<repository>/slides/`.

One-time setup in the repository on GitHub: go to **Settings → Pages** and set **Source** to **Deploy from a branch**, with branch `gh-pages` and folder `/ (root)`. The workflow only needs the default `GITHUB_TOKEN` (it has `contents: write` permission), so no secrets have to be added.

If your site workflow has a different name than `01 Build and Deploy Site`, change the `workflows:` entry under `workflow_run:` in `slides.yml` to match it exactly.

### Reuse as a template for a new training

1. Copy this repository folder (or mark the repository as a GitHub template) for the new training.
2. Replace the placeholders `TRAINING NAME` and `ORGANISATION` in `_quarto.yml`, `index.qmd` and `01-intro.qmd`.
3. Update the team, schedule and code of conduct in `01-intro.qmd`.
4. Delete `02-lesson-example.qmd` once you have your own lesson decks, and update `_quarto.yml` and `index.qmd` accordingly.
