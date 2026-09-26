# Catalog sources — OpenAstro Ara Data Manager deep-sky CSVs

Built 2026-08-07 from VizieR (CDS, Strasbourg) TSV exports plus CDS Sesame/SIMBAD
for a few Abell PNe. All positions J2000 (VizieR `_RAJ2000/_DEJ2000` computed
columns; FK5). Constellations computed from Roman (1987) boundary data
(VizieR VI/42, `constbnd.dat`) after precessing J2000 → B1875.

VizieR data reuse: free for research/education with attribution to the original
paper and to "CDS, Strasbourg Observatory, France" (VizieR licence page:
https://cds.unistra.fr/vizier-org/licences_vizier.html). Cite the papers below.

## sh2.csv — 313 rows
- Source: VizieR VII/20 (`https://vizier.cds.unistra.fr/viz-bin/asu-tsv?-source=VII/20&-out.max=unlimited&-out.add=_RAJ2000,_DEJ2000`) → `sh2.tsv`
- Cite: Sharpless S., 1959, ApJS 4, 257 (1959ApJS....4..257S)
- MajAx = `Diam` column (arcmin, per VizieR column description).

## ldn.csv — 1787 rows (see caveat)
- Source: VizieR VII/7A, same URL pattern → `ldn.tsv`
- Cite: Lynds B.T., 1962, ApJS 7, 1 (1962ApJS....7....1L)
- MajAx = sqrt(Area[deg^2])*60 arcmin (equivalent diameter).
- Caveat: the VII/7A machine-readable revision holds 1791 rows; 4 lack an
  original LDN number (dropped), and 15 original LDN numbers are absent from
  the revision entirely: 184, 366, 457, 465, 924, 1025, 1318, 1342, 1344,
  1413, 1575, 1592, 1593, 1603, 1792. Hence 1787 instead of 1802.

## barnard.csv — 349 rows
- Source: VizieR VII/220A, table `barnard` → `barnard.tsv` (catalog ID was correct)
- Cite: Barnard E.E., 1927, "Catalogue of 349 dark objects in the sky"
  (via Dobek, 2011 update, VII/220A)
- MajAx = `Diam` (arcmin). Names include letter suffixes (e.g. B44a) as published.

## vdb.csv — 158 rows
- Source: VizieR VII/21 → `vdb.tsv`
- Cite: van den Bergh S., 1966, AJ 71, 990 (1966AJ.....71..990V)
- V-Mag = illuminating star V magnitude; Identifiers = BD/CD/CP + HD of that star.
- Caveat: VII/21 has NO nebula size column (only star data + nebula type), so
  MajAx is empty for all rows.

## abell-pn.csv — 86 rows
- Primary source: Strasbourg-ESO Catalogue of Galactic Planetary Nebulae,
  VizieR V/84 tables `main` (→ `v84_main.tsv`) and `diam` (→ `abell_diam.tsv`);
  cite: Acker A. et al., 1992 (SECGPN).
- 72 objects carry main designation "A NN"; 6 more matched via name map
  (A10=K1-7, A22=K1-11, A25=K1-13, A27=K1-1, A38=K1-3, A81=IC1454);
  8 (A9, A11, A17, A32, A37, A64, A76, A85) are not in V/84 and were resolved
  via CDS Sesame/SIMBAD as "PN A66 NN" (`ses_NN.xml`); those have no MajAx.
- MajAx = optical diameter (arcsec)/60 from V/84 `diam`.
- Caveats: A85 = CTB 1, reclassified as a supernova remnant; A64 sometimes
  reclassified as a galaxy; A76 disputed. Kept to preserve the historical
  Abell 1..86 numbering.

## arp.csv — 338 rows
- Source: VizieR VII/192, table `arplist` → `arp.tsv`
- Cite: Arp H., 1966, ApJS 14, 1 (atlas); machine version Webb (1996, VII/192)
- arplist has 594 component-galaxy rows; merged to one row per Arp number
  (182 numbers have multiple components): position/V-mag/size/morph type from
  the first-listed (primary) component; all component names in Identifiers;
  NGC/IC columns filled when a component name parses as NGC/IC.

## wr.csv — 717 rows
- Source: the Galactic Wolf-Rayet Catalogue (P. Crowther, University of Sheffield),
  `http://pacrowther.staff.shef.ac.uk/WRcat/index.php`, main table scraped 2026-09-26.
  Supersedes the 226-row van der Hucht (2001, III/215) build.
- Cite: Rosslowe C.K. & Crowther P.A., 2015, MNRAS 447, 2322 (2015MNRAS.447.2322R)
  and the catalogue's own per-row references (spectral types, distances).
- Name = "WR NN" with the catalogue's suffixes (WR 20a, WR 2-1 …). Type = `WR*`
  (a star — planning consumers skip it; it exists to be searchable and to overlay).
- V-Mag = Johnson V where present, else the WR narrow-band v (290 of 717 rows have
  one); B/J/H/K as given. Hubble column carries the spectral type. Identifiers =
  HD + aliases (Gaia DR3 …). Common names = the catalogue's Nebula note (e.g.
  WR 136 → "NGC 6888") so the ring nebula resolves in search. Const is empty.
