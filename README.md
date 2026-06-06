# Bank Tail Risk Forecasting in Emerging Markets

**Replication code for:**  
Huillca, J. & Yi, L. (2026). *Forecasting Bank Credit Tail Risk in Emerging Markets: A Forward-Looking Structural Framework.* Master's Thesis, Barcelona School of Economics.

---

## Repository Structure

```
bank-tail-risk-peru/
├── 01_CETI_Pipeline.ipynb        # Bond panel construction + CETI estimation
├── 02_CETI_Analysis.ipynb        # CETI validation, hypothesis tests, figures
├── 03_TailRisk_HorseRace.ipynb   # LSTM-AL + CAViaR forecasting horse race
├── data/
│   ├── clri_lstm_ready.csv       # Processed input for notebook 03 (CETI + equity returns)
│   ├── df_master_export.csv      # Processed input for notebook 02 (full panel)
│   ├── Consolidado_SBS_Final.csv # SBS Peru: bank-level NPL data (public)
│   └── financial_system_index.csv # SBS Peru: system-level financial index (public)
├── mcmc_chains/
│   ├── chain_LSTM_BASE_BBVA_a01.npy
│   ├── chain_LSTM_BASE_BBVA_a05.npy
│   ├── chain_LSTM_BASE_BCP_a01.npy
│   ├── chain_LSTM_BASE_BCP_a05.npy
│   ├── chain_LSTM_CETI_BBVA_a01.npy
│   ├── chain_LSTM_CETI_BBVA_a05.npy
│   ├── chain_LSTM_CETI_BCP_a01.npy
│   └── chain_LSTM_CETI_BCP_a05.npy
└── outputs/                      # Created automatically on first run
```

---

## Notebooks

| # | Notebook | Description | Key Output |
|---|----------|-------------|------------|
| 1 | `01_CETI_Pipeline.ipynb` | Constructs the 146-bond panel, estimates the system spread signal (ΔCDSᴴ), performs market factor orthogonalisation, and computes the rolling CETI via WLS for BBVA Perú and BCP | `data/clri_lstm_ready.csv`, `data/df_master_export.csv` |
| 2 | `02_CETI_Analysis.ipynb` | Validates the CETI: distributional tests (H1), regime tests (H2), Granger causality vs NPL (H3), structural break diagnostics, all paper figures | Figures 3.1–3.7, Tables 3.2–3.3, 4.1–4.3 |
| 3 | `03_TailRisk_HorseRace.ipynb` | Estimates CAViaR-AS and LSTM-AL models; runs the two-stage CETI adjustment; computes VaR/ES forecasts, backtests (Kupiec, Christoffersen, Acerbi-Székely), and DM tests | Tables 4.4–4.8, Figures 4.5–4.7 |

---

## Reproducing the Results

### Option A — Exact replication (recommended)

Notebooks 02 and 03 can be run immediately using the pre-processed data files in `data/` and the saved MCMC posterior samples in `mcmc_chains/`. No re-estimation is required.

```bash
# Install dependencies
pip install numpy pandas scipy statsmodels matplotlib

# Run in order
jupyter notebook 02_CETI_Analysis.ipynb
jupyter notebook 03_TailRisk_HorseRace.ipynb
```

### Option B — Full pipeline from raw data

Notebook 01 requires the raw Bloomberg terminal data files (bond YTMs, equity prices, market indices) which are **not included** in this repository due to data licensing restrictions. These were sourced from Bloomberg Professional and Peru's SBS regulatory database.

If you have access to the same data sources, place the raw files in a `DATA/` folder following the filenames referenced in `01_CETI_Pipeline.ipynb`, then run all three notebooks in order.

---

## Reproducibility Note

The LSTM-AL model (Notebook 03) is estimated via adaptive MCMC. The posterior samples used to produce the results reported in the thesis are archived in `mcmc_chains/` as `.npy` files. Loading these chains directly (the default behaviour when chain files are present) ensures exact replication of all reported figures and statistics.

Re-running MCMC from scratch will produce numerically equivalent but not bit-identical results due to the stochastic nature of the sampler. All qualitative conclusions are robust to this variation, as discussed in Section 3.2.3 of the thesis.

---

## Dependencies

| Package | Version used |
|---------|-------------|
| Python | 3.10+ |
| numpy | 1.24+ |
| pandas | 2.0+ |
| scipy | 1.10+ |
| statsmodels | 0.14+ |
| matplotlib | 3.7+ |

---

## Data Sources

| Dataset | Source | Availability |
|---------|--------|--------------|
| Corporate bond YTMs (146 bonds) | Bloomberg Professional | Licensed — not included |
| Bank equity prices (BBVA Perú, BCP) | Bloomberg Professional | Licensed — not included |
| MSCI EM index, sovereign yields | Bloomberg Professional | Licensed — not included |
| Bank NPL ratios | Superintendencia de Banca y Seguros (SBS) Peru | Public — included in `data/` |
| Financial system index | Superintendencia de Banca y Seguros (SBS) Peru | Public — included in `data/` |

---

## Authors

- **Jimena Huillca** — [Barcelona School of Economics](https://bse.eu)  
- **Liqi Yi** — [Barcelona School of Economics](https://bse.eu)

Supervised by Prof. Argimiro Arratia Quesada, Barcelona School of Economics, June 2026.
