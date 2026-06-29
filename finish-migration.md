# Finishing the RePEc:ten → GitHub migration

Context: the University of Tennessee retired personal web pages, which used
to host the `RePEc:ten` working paper archive at `web.utk.edu/~jhollad3/RePEc/`.
The archive has been rebuilt locally (in `ten-repec/`) with corrected file
encodings and updated `File-URL`/`URL` fields pointing at its new home:
`https://university-of-tennessee-econ.github.io/repec/`. A profile README for
the GitHub organization has also been drafted (in `org-profile/`). What's left
is purely mechanical: create the two repos on GitHub, push the files, turn on
GitHub Pages, confirm it's reachable, and tell RePEc it moved.

GitHub org already exists: `https://github.com/University-of-Tennessee-Econ`

---

## 1. Create the org profile repo

This is the special repo whose README displays on the org's main GitHub page.

1. In the org, **New repository**
2. Name it **exactly** `University-of-Tennessee-Econ` (must match the org name)
3. Visibility: **Public**
4. Initialize with a README, then replace its contents with the file at
   `org-profile/README.md` (provided)
5. Commit directly on `main` via the GitHub web UI — no need for git locally
   for this one, it's a single file

## 2. Create and push the `repec` archive repo

1. In the org, **New repository** → name it `repec` → **Public** → do **not**
   initialize with a README, `.gitignore`, or license (we're bringing our own
   files, and an auto-created README would conflict with the push below)
2. From inside the unzipped `ten-repec/` folder, run:

   ```bash
   git init
   git add .
   git commit -m "Initial RePEc:ten archive migration off web.utk.edu"
   git branch -M main
   git remote add origin https://github.com/University-of-Tennessee-Econ/repec.git
   git push -u origin main
   ```

3. Expect ~90 files, ~60MB, to push (46 metadata records, 41 PDFs, plus
   `README.md`, `index.html`, `.nojekyll`). If the push is rejected for size,
   check `git status` for anything unexpected — nothing in this repo should
   trip GitHub's 100MB single-file limit.

## 3. Enable GitHub Pages

1. In the `repec` repo: **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. **Save**. Takes a minute or two to build.

## 4. Verify it's actually live before telling anyone

Check each of these load in a browser (not a 404, not a Jekyll-mangled page):

- `https://university-of-tennessee-econ.github.io/repec/` → the paper listing
- `https://university-of-tennessee-econ.github.io/repec/RePEc/ten/tenarch.rdf`
- `https://university-of-tennessee-econ.github.io/repec/RePEc/ten/wpaper/2025-04.redif`
- `https://university-of-tennessee-econ.github.io/repec/pdf/2025-04.pdf`

If any of these 404, the most likely cause is Pages still building (wait a
couple minutes) or `.nojekyll` not having pushed correctly.

## 5. Notify RePEc that the archive moved

RePEc's crawler only re-checks an archive's location when told to. Email the
RePEc team (this is their documented process, not a cold request):

```
To: repec@repec.org
Subject: Archive RePEc:ten has moved

Hello,

The RePEc:ten archive (Department of Economics, University of Tennessee)
has moved from:

  http://web.utk.edu/~jhollad3/RePEc/ten/

to:

  https://university-of-tennessee-econ.github.io/repec/RePEc/ten/

The tenarch.rdf URL field has been updated accordingly. Could you point
the crawler at the new location?

Thanks,
Scott Holladay
jhollad3@utk.edu
```

## 6. Known data gaps — not blockers, can be done anytime after the push

These were flagged during the migration and don't need to hold up steps 1–5:

- **`2015-01`, `2015-04`, `2016-01`** — the only known copies of these PDFs
  lived on coauthors' now-retired UTK personal pages (`~mwanamak/`,
  `~ccarrut1/`). Their `File-URL` lines were removed rather than left
  pointing at a dead link. Get replacement PDFs from Marianne Wanamaker /
  Celeste Carruthers, drop them in `pdf/`, and restore the `File-URL` /
  `File-Format` / `File-Function` lines in the corresponding `.redif` file.
- **`2015-02`, `2015-06`** — never had a `File-URL` at all, even on the old
  server. Pre-existing gap, not caused by this move.
- **`2018-05.pdf`** — sitting in the old `RePEc/` folder with no metadata
  record at all. Worth checking whether this paper's metadata went missing
  at some point; it was not carried into the new repo.
- **`Coal Generator Retirements.pdf`, `CoverPage 2025-04.pdf`** — also not
  carried over; unclear if these need a place in the series (the cover page
  may be worth attaching to `2025-04` as a second `File-URL` entry).

## 7. Adding a new working paper in the future

Documented in `repec/README.md` once pushed — short version: copy an
existing `.redif` file as a template, update `Number`/`Handle`/`File-URL`/
metadata, drop the PDF in `pdf/`, commit, push. RePEc's crawler picks it up
automatically within a day or two; no manual notification needed for new
papers (only needed when the archive's *location* changes, as in step 5).
