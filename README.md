# NEXUS · HUD

**A self-contained, offline astronomical ephemeris and visualization engine in a single HTML file.**

NEXUS · HUD provides high-precision ephemerides for the Sun, the Moon, and a curated catalogue of 80 bright stars, with an interactive 3D interface, a Besselian eclipse-path renderer, a full GNSS-aware time-scale converter, and an embedded self-audit of 113 numerical checks executed live in the browser.

The software is distributed as a single HTML document with no external dependencies, no network access requirement, and no installation procedure. All ephemeris data, textures, and reference tables are embedded in the file.

---

## 1. Scope

The engine computes, at arbitrary epochs within the interval −4000 to +8000 (with reduced precision outside −4000…+8000 for the solar series and beyond ±30 cy for the lunar series):

- Apparent geocentric positions of the Sun and the Moon;
- Apparent places of 80 bright stars (proper motion, parallax, aberration, solar light deflection);
- Lunar libration, sub-Earth point, sub-solar point, and axis position angle;
- Earth orientation parameters: ERA, GMST, GAST, LMST, LAST, polar motion, LOD, CIP coordinates, celestial intermediate origin locator;
- Conversion between 12 time scales: UTC, TAI, TT, TDB, UT1, TCG, TCB, WIB, LMT, GPS, GST, BDT, GLONASS;
- Solar and lunar eclipse circumstances, Besselian elements, path geometry, and local circumstances with Watts limb correction;
- Phase and apsidal events (new/quarter/full Moon, perigee, apogee);
- Spherical-trigonometry quantities: great-circle distance (spherical and Vincenty on WGS-84), rhumb line, astronomical triangle PZX, triangle A–B–Pole, spherical excess, and area.

All results are exposed as numerical cards and are subject to the embedded audit.

---

## 2. Scientific basis

### 2.1 Dynamical models

| Quantity | Model | Reference |
|---|---|---|
| Lunar motion | ELP/MPP02 (LLR fit), 2 645 main + 33 256 perturbation terms (full series) | Chapront & Francou 2003, A&A 404, 735 |
| Solar and Earth-Moon barycentre motion | VSOP2013, 294 425 terms (full series) | Simon et al. 2013, A&A 557, A49 |
| Long-range lunar motion (beyond ±30 cy) | JANUS 98.12 | Chapront-Touzé & Chapront 1988, A&A 190, 342 |
| Precession-nutation | IAU 2006 (Fukushima–Williams) + IAU 2006/2000A_R06 | IERS Conventions 2010, TN 36 |
| Lunar libration | LLIB04 with LLIB245/403/405 and numerical complements | Chapront et al., polac.obspm.fr |
| Earth orientation | IERS Bulletin A, Bulletin C, EOP 20C04, FCN continuation of dX, dY | IERS |
| ΔT | HMNAO/IERS table, SMH2016 ensemble | Stephenson, Morrison & Hohenkerk 2016, Proc. R. Soc. A 472 |
| Solar and lunar eclipse circumstances | Besselian elements, NASA Five Millennium Canon | Espenak & Meeus 2006, NASA/TP-2006-214141 |
| Lunar limb profile | Watts 1963, VizieR VI/122, 0.2° grid, 91 × 81 × 1800 records | C. Watts |
| Reference frame | ICRS, IAU 2006/2000A, CIP/CIO | SOFA software collection |
| Star catalogue | 80 bright stars, apparent place | Source not stated; entries marked `*` rounded to 1′ |
| Time scales | SOFA t_cal2jd, t_dat, t_tttcg, t_era00, t_gmst06, t_gst06a; GNSS ICDs | IAU SOFA, IS-GPS-200, Galileo OS SIS ICD, BDS-SIS-ICD, GLONASS ICD |

### 2.2 Declared uncertainties (from the authors)

- **Lunar position:** 0.06″ in longitude, 0.003″ in latitude, 4 m in distance (vs DE405/406, 1950–2060).
- **Solar position:** ≤ 6.4 mas vs IPS2000 outside the 1890–2000 fit; ≤ 0.03″ over 900–3100.
- **Libration:** ≈ 0.1–0.3″ in P1, P2, τ vs DE-n solutions.
- **Nutation:** 0.06 µas vs SOFA iauNut06a test values.
- **CIP X, Y:** ≤ 2 µas vs IERS series.
- **UT1:** ≈ 0.1 ms or better (final C04), degrading for predictions.
- **Light-time and aberration:** Moon position at t − r/c; solar aberration with heliocentric Earth velocity, error ≤ 0.01″.

### 2.3 Quantities not modelled

The following effects are explicitly excluded from the model and stated in the in-page documentation:

- Topocentric parallax (geocentric pages);
- Atmospheric refraction;
- Polar motion (geocentric pages);
- Relativistic light bending by the Moon;
- Mean-Earth to principal-axis offset (≈ 0.02°) in the libration frame.

---

## 3. Embedded audit

The software includes a self-contained audit comprising **113 numerical checks** executed live in the browser. The audit validates internal consistency and agreement with independent references. Each check reports its residual, the declared tolerance, and a pass/fail flag.

### 3.1 Summary statistics (reference epoch 2026-10-13)

| Metric | Value |
|---|---|
| Checks within tolerance | 113 / 113 |
| Median use of tolerance | 8.2 % |
| Checks above 70 % of tolerance | 5 |
| Audit wall-clock time | ≈ 80 s |

### 3.2 Distribution by system

| System | Checks | Worst margin |
|---|---|---|
| VSOP2013 (EMB, ecliptic + ICRS, interpolation) | 3/3 | 55 % of ±1.0 × 10⁻¹⁰ au |
| IPS2000 (independent theory, cross-check) | 1/1 | 74 % of ±1.5 × 10⁻⁷ au |
| JANUS 98.12 | 3/3 | 80 % of ±0.2″ |
| Nutation (SOFA iauNut06a) | 2/2 | 57 % of ±0.1 µas |
| Earth (EMB − µ·Moon) | 1/1 | 21 % of ±5.0 × 10⁻⁸ au |
| Frame (IERS CIP X, Y) | 2/2 | 44 % of ±3 µas |
| LLIB04 | 2/2 | 83 % of ±6.0 × 10⁻⁴″ |
| Libration (Meeus Ex. 53.a) | 2/2 | 87 % of ±0.05° |
| Eclipse (NASA/Espenak 2026) | 12/12 | 6.2 % of ±0.003 |
| Season (equinoxes, solstices 2026 vs DE405) | 4/4 | 39 % of ±8 s |
| ELP/MPP02 (Table 8) | 2/2 | 19 % of ±1.0 × 10⁻⁴ km |
| TDB − TT (HF2002, FB2001) | 5/5 | 65 % of ±40 ns |
| ΔT (Timescales.py, IERS) | 3/3 | 23 % of ±5 ms |
| Stars (SOFA iauAtci13) | 2/2 | 12 % of ±0.02″ |
| Orbit angles (internal geometry) | 2/2 | 0.0 % |
| Moon face (selenodetic transform) | 1/1 | 0.0 % |
| Eclipse map (NASA canon, 20 sub-checks) | 20/20 | 49 % of tolerance |
| Besselian elements (12 eclipses + 454 full-set) | 7/7 | 41 % |
| Eclipse path (5 eclipses 1999–2026) | 10/10 | 46 % |
| Lunar limb (Watts 1963, 10 sub-checks) | 10/10 | 36 % |
| Time scales (SOFA, IERS, GNSS ICDs) | 19/19 | 26 % |

### 3.3 Tightest margins

The five checks with the highest tolerance utilisation are reported below. These reflect the precision limit of the reference file, not a deficiency of the model.

| Check | Margin |
|---|---|
| Libration: Meeus Ex. 53.a, l vs optical, 1992-04-12 0h TD | 87 % of ±0.05° |
| LLIB04: P1, P2, τ vs LLIB04.txt (30 dates), solution | 83 % of ±6.0 × 10⁻⁴″ |
| LLIB04: with numerical complements | 83 % of ±6.0 × 10⁻⁴″ |
| JANUS: Example 1 (−1562, ET) longitude, max \|Δ\| | 80 % of ±0.2″ |
| IPS2000: EMB vs notice.txt, 11 epochs 500–2498 | 74 % of ±1.5 × 10⁻⁷ au |

### 3.4 Tolerance derivation

Tolerances are not adopted by convention. Each is derived either from:

- The arithmetic resolution of the reference file (number of decimal digits printed);
- The documented accuracy of the model, taken from its source publication;
- The quadrature of declared errors when two independent sources are compared.

This procedure is documented inline for every row of the audit.

---

## 4. Interface

The application is organised into five functional tabs.

### 4.1 HUD — Celestial globe

Interactive 3D celestial sphere with the following layers: Earth, graticule, equator, CIP·CIO, ERA ring, stations, Sun·Moon, celestial, horizon, stars, 3D triangles, eclipse shadow, eclipse path. View modes: free, Jolotundo, Greenwich, CIO, inertial, Sun. Data cards are accessible by tapping markers or the globe.

### 4.2 ORBIT · ANGLES

Two 3D scenes:

- **Geocentric**: Sun–Earth–Moon geometry with the lunar trail over ±15 days, rendered with a sliding-window incremental update. Angles ψ (elongation), i (phase), δ (Sun angle) are drawn in-scene.
- **Heliocentric**: Earth orbit from VSOP2013 with perihelion/aphelion and equinox/solstice markers.

Additional panels: spherical-triangle solver (A–B–Pole), Vincenty ellipsoidal distance on WGS-84, rhumb-line, astronomical triangle PZX with the bright-star catalogue, Clairaut maximum latitude, antipode, spherical excess and area.

### 4.3 MOON FACE

Photographic 3D lunar globe rendered in WebGL from the LROC-WAC mosaic (near and far sides, 360° equirectangular). Topographic relief derived from the LRO GLD100 digital elevation model. Interaction: drag-to-rotate, double-tap to reset. Automatic libration follows the simulation epoch. Selenographic graticule, 20 maria, 16 craters, orientable frame (IAU selenographic vs celestial north). Watts limb profile ×20 is available as a topographic overlay of the limb.

### 4.4 TIME SCALES

Bidirectional converter between UTC, TAI, TT, TDB, UT1, TCG, TCB, WIB, LMT, GPS, GST, BDT, GLONASS. Input accepts calendar dates, JD, MJD, or week/TOW for GPS, GST, BDT. Round-trip inversion is performed and the residual is displayed. EOP values are taken from IERS Bulletin A, with FCN continuation beyond the tabulated range.

### 4.5 EPHEMERIS AUDIT

Executes the audit described in §3. Reference model and data tables are also reproduced in this tab, with full bibliographic entries.

---

## 5. Technical specification

- **Distribution format:** single HTML document.
- **Embedded data:** all textures and reference tables encoded as base64.
- **Rendering:** WebGL (lunar globe) and Canvas 2D (celestial globe, orbit scenes, eclipse path).
- **File size:** ≈ 17 MB (dominated by the LROC-WAC 4K texture and the GLD100 heightmap).
- **Runtime dependencies:** none.
- **Network:** not required at any point.
- **Target platform:** modern mobile browsers with WebGL support (Chrome, Firefox, Safari).

### 5.1 Lunar textures

- Mosaic: LROC-WAC 4K equirectangular, 360°, retrieved from the CDS HiPS2FITS service, survey `CDS/P/Moon/LROC-WAC-GLD100-118m`.
- Heightmap: GLD100 level-2 tiles (32 tiles at 1024 × 1024), assembled to 8192 × 4096, downsampled to 4096 × 2048 grayscale, with polar band correction by mirroring the outer 12 %.
- Relief rendering: perturbed surface normal from finite differences of the height field, applied in the fragment shader.

---

## 6. Usage

1. Open `NEXUS_ESO-18_GNSS.html` in a browser.
2. Use the tab bar to navigate between HUD, ORBIT · ANGLES, MOON FACE, TIME SCALES.
3. Time is controlled per-tab via the simulation buttons: `−1 d`, `+1 d`, `1 d/s`, `1 wk/s`, `LIVE ×1`, and others.
4. Tap a marker or the globe to display its data card.
5. In MOON FACE, drag to rotate and double-tap to reset.
6. In TIME SCALES, select the input scale at left; the conversion is displayed below.
7. To execute the audit, open the HUD tab, scroll to the bottom, and press **RUN AUDIT** (≈ 80 s).

No installation, no account, no network access required.

---

## 7. Limitations

- The star catalogue contains 80 entries. Rows marked `*` in the audit are rounded to 1′ (≈ 0.1°).
- Selenographic feature coordinates (20 maria, 16 craters) are hand-entered and accurate to approximately ±1° (±30 km).
- Libration is reported in the principal-axis frame; the mean-Earth frame differs by ≈ 0.02° and is not applied.
- Eclipse circumstances near the limits of the Five Millennium Canon may show reduced agreement; the catalogue itself is not exact.
- The reduced-precision solar series used outside −4000…+8000 has an estimated accuracy of ≈ 0.01°.

---

## 8. Citation

If this software contributes to published work, please cite the underlying sources listed in §2 rather than the software itself, unless the software is the primary subject of the citation. A citation block for NEXUS · HUD will be provided when the version is fixed.

---

## 9. Licence

This software is released under the MIT License. See the [LICENSE](LICENSE) file for the full text.

The ephemeris values produced by this software are derived from the scientific models listed in §2 and §10. Those underlying models are the property of their respective authors and institutions; the MIT License applies only to the source code and data contained in this repository.

---

## 10. Credits

Scientific data and models are derived from the work of:

- Institut de Mécanique Céleste et de Calcul des Éphémérides (IMCCE) — VSOP2013, IPS2000.
- Observatoire de Paris, SYRTE — ELP/MPP02, LLIB04/245/403/405, JANUS.
- IAU SOFA Board — reference implementations for precession-nutation, Earth rotation, and time scales.
- International Earth Rotation and Reference Systems Service (IERS) — EOP, leap seconds, Conventions 2010.
- NASA Goddard Space Flight Center — Five Millennium Canon of Solar Eclipses (Espenak & Meeus).
- C. B. Watts — lunar limb profile (1963).
- Centre de Données astronomiques de Strasbourg (CDS) — HiPS2FITS service.
- Gaia Sky project — GLD100 virtual texture.

---

*Developed entirely on a mobile device, within a Pydroid 3 terminal environment.*
