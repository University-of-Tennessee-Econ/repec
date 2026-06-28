# RePEc:ten — University of Tennessee, Department of Economics Working Papers

This repository hosts the RePEc archive for the UT Department of Economics
working paper series. It used to live at `web.utk.edu/~jhollad3/RePEc/`; the
university retired personal web pages, so the archive moved here.

RePEc's crawlers visit this repo daily to pull metadata into IDEAS,
EconPapers, and Google Scholar. Nobody needs to "submit" anything — it's all
automatic, as long as the files below stay reachable at a stable URL.

## Structure (required by RePEc, do not rename folders)

```
RePEc/ten/tenarch.rdf      archive-level info (handle: RePEc:ten)
RePEc/ten/tenseri.rdf      series-level info (handle: RePEc:ten:wpaper)
RePEc/ten/wpaper/*.rdf     one metadata file per paper (.rdf = Windows-1252,
            /*.redif       .redif = UTF-8 — keep whichever extension a file
                           already has; don't convert between them)
pdf/*.pdf                  the actual paper PDFs
```

Each metadata file's `Number:` field (e.g. `2025-04`) matches its filename
and the corresponding PDF in `pdf/`.

## Adding a new working paper

1. Pick the next number in sequence, e.g. `2026-01`.
2. Drop the PDF in `pdf/` as `2026-01.pdf`.
3. Copy any existing file in `RePEc/ten/wpaper/` as a template, save it as
   `2026-01.rdf` (or `.redif` if it has accented characters), and update:
   `Author-*`, `Title`, `Abstract`, `Creation-Date`, `Number`, `File-URL`,
   and `Handle` (`RePEc:ten:wpaper:2026-01`).
4. Commit and push. RePEc picks it up within a day or two.

## If this archive ever moves again

Update the `URL:` line in `tenarch.rdf` to the new address, confirm it loads
in a browser, then email RePEc (repec@repec.org) to tell them the archive
moved. That's the only step that requires contacting anyone outside this repo.

## Known gaps (as of the 2026 migration off UTK servers)

A few records reference papers whose only copy lived on a coauthor's UTK
personal page, which is also gone:

- `2015-01` (Carruthers & Wanamaker) — was at `~mwanamak/Wage_Gap.pdf`
- `2015-04` (Carruthers & Welch) — was at `~ccarrut1/CarruthersWelch_SEPT2015.pdf`
- `2016-01` (Kim, Carruthers & Harris) — was at `~ccarrut1/Kim_Carruthers_Harris...pdf`

Their `File-URL` lines were removed rather than left pointing at a dead link.
Drop a replacement PDF in `pdf/` and restore the `File-URL`/`File-Format`/
`File-Function` lines once you have a copy.

`2015-02` and `2015-06` never had a `File-URL` at all (a pre-existing gap,
not caused by this move).

Three PDFs in the old `RePEc/` folder had no metadata record at all and
weren't carried over: `2018-05.pdf`, `Coal Generator Retirements.pdf`,
`CoverPage 2025-04.pdf`. Worth a look to see whether `2018-05` is a paper
that's missing its metadata entirely.
