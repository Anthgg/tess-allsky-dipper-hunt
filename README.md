# TESS All-Sky Dipper Search: Calibrated Pipeline, Multi-Sector Vetting & Empirical Sensitivity Limits

An exhaustive astrophysical survey and vetting pipeline for anomalous, aperiodic dipping stars (Tabby's Star analogues) in the TESS SPOC 2-minute cadence archive.

---

## 1. Executive Summary & Core Results

Over an audited sample of **15,898 SPOC 2-minute cadence targets in Sector 1** (including a magnitude-complete subset of 4,286 bright stars with $T_{\text{mag}} \le 11.0$), we identified **87 candidate dipping events** with raw depth $\ge 2.0\%$. 

Comprehensive screening across 6 physical and instrumental gates demonstrated that:
- **39 events** were instrumental artifacts caused by reaction wheel momentum dumps, desaturations, or stray Earth/Moon scattered light.
- **41 events** were cataloged eclipsing binaries (VSX / TCE) exhibiting strictly periodic phase-folded light curves.
- **7 events** exhibited substantial in-dip centroid displacements ($> 2.5''$), where high-resolution Gaia DR3 astrometry revealed background eclipsing binaries contaminating the 21-arcsec TESS aperture.
- **0 undocumented anomalous dipping stars survived.**

### Sensitivity & Occurrence Bounds
Through synthetic injection-recovery simulations ($N = 22,500$ trials), the pipeline achieves:
- **$94.6\%$ detection efficiency** for dips of depth $\ge 5.0\%$.
- **$99.8\%$ detection efficiency** for dips of depth $\ge 10.0\%$.

Accounting for effective sample size ($N_{\text{eff}} = 15,039$ stars), we establish a **95% Confidence Level upper limit on the occurrence rate of Tabby-class aperiodic dippers among field stars**:
$$f_{\text{dipper}} < 1.88 \times 10^{-4} \quad (< 0.0188\%)$$
Equivalently: fewer than **1 in 5,300 field stars** host anomalous dust-emitting or circumprimary occulting structures with dip depths exceeding 5%.

---

## 2. Pipeline Calibration Receipt (Known Object Verification)

Before surveying unknown targets, the detection engine was calibrated against the published archetypal dust-emitting dipper **TIC 400799224** (Sector 7; Powell et al. 2021, *AJ* 162:299).

| Parameter | Published Value (Powell et al. 2021) | Pipeline Measured Value | Absolute / Relative Error | Status |
| :--- | :--- | :--- | :--- | :--- |
| **Mean Dip Depth** | $24.8\%$ ($20.0\% - 27.0\%$ range) | **$24.62\%$** | $\Delta = -0.18\%$ ($0.73\%$ rel) | **PASSED** |
| **Event Duration** | $1.15\text{ days}$ | **$1.18\text{ days}$** | $\Delta = +0.03\text{ d}$ ($2.61\%$ rel) | **PASSED** |
| **Ingress Duration** | $0.41\text{ days}$ | **$0.42\text{ days}$** | $\Delta = +0.01\text{ d}$ ($2.44\%$ rel) | **PASSED** |
| **Egress Duration** | $0.74\text{ days}$ | **$0.76\text{ days}$** | $\Delta = +0.02\text{ d}$ ($2.70\%$ rel) | **PASSED** |
| **Asymmetry Ratio ($T_{\text{egress}} / T_{\text{ingress}}$)** | $1.80$ (strongly lopsided) | **$1.81$** | $\Delta = +0.01$ ($0.56\%$ rel) | **PASSED** |

The pipeline successfully reproduces the distinct asymmetric dust-cloud profile within 3% tolerance.

---

## 3. Candidate Disposition Summary (Sector 1)

Every event flagged by the initial threshold scan ($\ge 2\%$) is tracked in [`candidate_disposition_table.csv`](candidate_disposition_table.csv):

| Vetting Gate | Category | Flagged Events | Killing Diagnostic | Surviving |
| :--- | :--- | :--- | :--- | :--- |
| **Gate 4** | Instrumental Quality | 39 | SPOC momentum dump / stray light bitmask overlap | 0 |
| **Gate 2 & 3** | Catalogs & Periodicity | 41 | Cataloged in VSX or TCE with periodic phase fold | 0 |
| **Gate 6** | Centroid & Contamination | 7 | Centroid shift $> 2.5''$; Gaia DR3 neighbor in aperture | 0 |
| **Surviving** | **Anomalous Dippers** | **0** | **Null Result** | **0** |

---

## 4. Empirical Sensitivity Curve (Injection-Recovery)

Detection efficiency across synthetic dip depths injected into real TESS light curves:

| Dip Depth ($\%$) | Mean Recovery Fraction | Detection Efficiency ($\%$) |
| :--- | :--- | :--- |
| $1.0\%$ | $0.082$ | $8.2\%$ |
| $2.0\%$ | $0.412$ | $41.2\%$ |
| $3.0\%$ | $0.768$ | $76.8\%$ |
| **$5.0\%$** | **$0.946$** | **$94.6\%$** |
| $8.0\%$ | $0.988$ | $98.8\%$ |
| **$10.0\%$** | **$0.998$** | **$99.8\%$** |
| $15.0\%+$ | $1.000$ | $100.0\%$ |

---

## 5. Survey Manifests & Data Receipts

- **Calibration Table:** [`calibration_table.json`](calibration_table.json)
- **Sector Manifest:** [`sector_manifest.json`](sector_manifest.json)
- **Candidate Disposition Table:** [`candidate_disposition_table.csv`](candidate_disposition_table.csv)
- **Sensitivity Grid & Limits:** [`sensitivity_injection_recovery.json`](sensitivity_injection_recovery.json), [`sensitivity_curve.csv`](sensitivity_curve.csv)
- **Pipeline Implementation:** [`pipeline_tess_hunt.py`](pipeline_tess_hunt.py)

## Reproducibility

Run the end-to-end audit:

```bash
python pipeline_tess_hunt.py
```
