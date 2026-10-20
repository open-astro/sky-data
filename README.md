# sky-data — OpenAstro self-hosted catalog snapshots

Immutable snapshots of the astronomical catalogs that the OpenAstro Ara
daemon's **Data Manager** (`/api/v1/data-manager`) downloads on demand. They
are hosted here so the download URLs survive upstream repo moves/deletions;
the daemon verifies each download against a pinned SHA-256 before installing.

The daemon downloads the catalog CSVs as **commit-pinned raw URLs** (e.g.
`https://raw.githubusercontent.com/open-astro/sky-data/<commit>/NGC.csv`);
the catalog releases mirror the same files for humans.

## ASTAP plate-solver star databases (release `astap-2026.10`)

The ASTAP star databases are too large for raw URLs (GitHub caps a raw file at
100 MB), so they are published only as **release assets** on the
[`astap-2026.10`](https://github.com/open-astro/sky-data/releases/tag/astap-2026.10)
release. Release assets 302-redirect to `objects.githubusercontent.com`; the
daemon's sky-data client follows a redirect only to an https URL on github.com
or `*.githubusercontent.com` (OpenAstro Ara #1136). Each asset is a flat
`tar.gz` of the database tiles (`<abbrev>_<tile>.<ext>`, no directory prefix),
repacked unmodified from Han Kleijn's SourceForge `.deb` packages
(`astap-program/star_databases`) by the `ASTAP databases` workflow in this repo
(`.github/workflows/astap-databases.yml`, reproducible tar: sorted names, fixed
mtime, no owner, `gzip -n`).

| Asset | Database | Uncompressed | Fields | Licence (as shipped upstream) |
|---|---|---|---|---|
| `astap-d80.tar.gz` | D80 — Gaia DR3 to 8000 stars/deg² (1476 tiles) | 1 332 217 055 B | ~0.25° and up | "Free if credit is given" — copyright ESA, compiled by Han Kleijn |
| `astap-g05.tar.gz` | G05 — Gaia DR3 to 500 stars/deg² (290 tiles) | 105 388 420 B | above ~6° | GPL-3+ (Han Kleijn) |
| `astap-w08.tar.gz` | W08 — wide-field, magnitude 8 (1 file) | 495 184 B | above ~20° | GPL-3+ (Han Kleijn) |

```
397b267181d3f5d742e1bb220ad3ab3629e12141f365f30f6ea5b54c8f797bfc  astap-d80.tar.gz
515f5ce998a3ec9c807199e3ee8258e0ce098b8bebf0b60e1c820928f0834c64  astap-g05.tar.gz
ba09f32dad097c415c88b416e264f3e2eb1dbbc8266b13753ea2221fe658a911  astap-w08.tar.gz
```

Credit: this work has made use of data from the European Space Agency (ESA)
mission Gaia (https://www.cosmos.esa.int/gaia), processed by the Gaia Data
Processing and Analysis Consortium (DPAC,
https://www.cosmos.esa.int/web/gaia/dpac/consortium). Funding for the DPAC has
been provided by national institutions, in particular the institutions
participating in the Gaia Multilateral Agreement. The databases were compiled
by Han Kleijn for [ASTAP](https://www.hnsky.org/astap.htm). The H17/H18
databases are obsolete upstream and are not mirrored.

To refresh: cut a new `astap-YYYY.MM` release, run the workflow against it (never replace assets on an existing tag), and update
`DataManagerService.Catalog` + `CatalogSha256` in `open-astro/openastro-ara`.

These files are redistributed **unmodified** from their upstream sources,
under their share-alike licenses, with attribution:

| Asset | Upstream | Pinned source | License |
|---|---|---|---|
| `hygdata_v40.csv.gz` | [HYG star database v4.0](https://www.astronexus.com/hyg) — David Nash / Astronomy Nexus | [`astronexus/HYG-Database` @ `c7f7f88`](https://github.com/astronexus/HYG-Database/blob/c7f7f883fe678cc7680169a50ccd7dcc49b060ce/hyg/CURRENT/hygdata_v40.csv.gz) | CC BY-SA 2.5 |
| `NGC.csv` | [OpenNGC](https://github.com/mattiaverga/OpenNGC) — Mattia Verga | [`mattiaverga/OpenNGC` @ `36cb178`](https://github.com/mattiaverga/OpenNGC/blob/36cb178a0f69dba8bfc03a99c10512831edf1c6b/database_files/NGC.csv) | CC BY-SA 4.0 |

SHA-256 of the snapshot assets (identical to the pinned upstream bytes):

```
8e3ff9e67445e558a759b117910850cff1b1d4d492f45f715c2ee2db3d869bac  hygdata_v40.csv.gz
840fe0c9ee1332e551b2e722a0e92726cd7b157914a3d2177602832aadd3aa9e  NGC.csv
```

## Updating

Commit the refreshed files to `main` and cut a matching release (e.g. `v2`) —
never rewrite history or replace assets on an existing tag, since the daemon
pins a commit SHA + digest. Update the `DataManagerService.Catalog` raw URLs
(new commit SHA) and `CatalogSha256` digests in `open-astro/openastro-ara` in
the same change.
