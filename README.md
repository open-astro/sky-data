# sky-data — OpenAstro self-hosted catalog snapshots

Immutable snapshots of the astronomical catalogs that the OpenAstro Ara
daemon's **Data Manager** (`/api/v1/data-manager`) downloads on demand. They
are hosted here so the download URLs survive upstream repo moves/deletions;
the daemon verifies each download against a pinned SHA-256 before installing.

The daemon downloads the files as **commit-pinned raw URLs** (e.g.
`https://raw.githubusercontent.com/open-astro/sky-data/<commit>/NGC.csv`) —
NOT the release assets: release-asset downloads 302-redirect and the daemon's
sky-data HTTP client refuses redirects as an HTTPS-downgrade guard, while raw
URLs serve 200 directly. The GitHub release mirrors the same files for humans.

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
