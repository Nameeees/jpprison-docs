# JPPrison Documentation

Source of the JPPrison documentation site, built with [MkDocs](https://www.mkdocs.org/) + [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Every page is a plain Markdown file under `docs/` — the whole site is yours to edit.

## Preview locally

Requires Python 3 (already installed if `py --version` works).

```bash
py -m pip install -r requirements.txt
mkdocs serve
```

Open <http://127.0.0.1:8000> — the site reloads live while you edit files in `docs/`.

## Publish on GitHub Pages (free)

Everything is prepared for `https://github.com/Nameeees/jpprison-docs`: the site
links in `mkdocs.yml` and the local `origin` remote already point there.

1. Create the **public** repository `jpprison-docs` on the `Nameeees` account —
   **without** adding a README, .gitignore or license (keep it completely empty).
2. Push this folder:

   ```bash
   git push -u origin main
   ```

3. In the repository: **Settings → Pages → Build and deployment → Source: GitHub Actions**.
4. Done. Every push to `main` rebuilds and publishes the site at
   `https://nameeees.github.io/jpprison-docs/`.

If you ever rename the repository or move it to another account, update the three
URLs at the top of `mkdocs.yml` and the git remote
(`git remote set-url origin https://github.com/<user>/<repo>.git`).

## Editing later

- **From GitHub**: open any file under `docs/`, click the pencil (Edit) button, commit — the site rebuilds automatically. The "edit this page" link on every page takes you straight there.
- **Locally**: edit `docs/*.md`, check with `mkdocs serve`, push.
- **From a phone**: GitHub's web editor works fine for text edits.

## Structure

```text
mkdocs.yml            site configuration (theme, navigation, placeholders)
requirements.txt      Python dependencies
docs/                 all the content (Markdown)
  index.md            home
  getting-started/    installation, quick start, configuration
  features.md         full system tour
  reference/          commands, placeholders
  developer/          API for add-on developers
  tools/              enchant creator guide
  changelog.md        changelog
.github/workflows/    automatic deployment to GitHub Pages
```

## Notes

- Never edit anything inside `site/` — that folder is generated output.
- Wherever the pages say "the plugin repository", link your actual plugin repo/download page if you want.
- The documentation was generated from the project's own reference material (`README.md`, `FEATURES.txt`, `comandos.txt`, `placeholders-papi.txt`, `BALANCE.md`, `CHANGELOG.md`).
