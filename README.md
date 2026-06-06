# Bank Tail Risk Forecasting in Emerging Markets

**Replication code for:**  
Huillca, J. & Yi, L. (2026). *Forecasting Bank Credit Tail Risk in Emerging Markets: A Forward-Looking Structural Framework.* Master's Thesis, Barcelona School of Economics.

🔗 Repository: https://github.com/liqi-yi/Credit-Signals-and-Bank-Tail-Risk

---

## Files

| File | Description |
|------|-------------|
| `01_CETI_Pipeline.ipynb` | Bond panel construction, system spread signal (ΔCDSᴴ), market factor orthogonalisation, rolling CETI estimation via WLS |
| `02_CETI_Analysis.ipynb` | CETI validation: distributional tests (H1), regime tests (H2), Granger causality vs NPL (H3), structural break diagnostics, all paper figures |
| `03_TailRisk_HorseRace.ipynb` | CAViaR-AS and LSTM-AL estimation, two-stage CETI adjustment, VaR/ES forecasts, backtests (Kupiec, Christoffersen, Acerbi-Székely), DM tests |
| `clri_lstm_ready.csv` | Processed input for notebook 03: weekly CETI series + bank equity returns |
| `df_master_export.csv` | Processed input for notebook 02: full bond-equity panel |
| `Consolidado_SBS_Final.csv` | SBS Peru: bank-level NPL data (public) |
| `financial_system_index.csv` | SBS Peru: system-level financial index (public) |
| `chain_LSTM_BASE_BBVA_a01.npy` | MCMC posterior samples — LSTM baseline, BBVA, τ=1% |
| `chain_LSTM_BASE_BBVA_a05.npy` | MCMC posterior samples — LSTM baseline, BBVA, τ=5% |
| `chain_LSTM_BASE_BCP_a01.npy` | MCMC posterior samples — LSTM baseline, BCP, τ=1% |
| `chain_LSTM_BASE_BCP_a05.npy` | MCMC posterior samples — LSTM baseline, BCP, τ=5% |
| `chain_LSTM_CETI_BBVA_a01.npy` | MCMC posterior samples — LSTM-CETI, BBVA, τ=1% |
| `chain_LSTM_CETI_BBVA_a05.npy` | MCMC posterior samples — LSTM-CETI, BBVA, τ=5% |
| `chain_LSTM_CETI_BCP_a01.npy` | MCMC posterior samples — LSTM-CETI, BCP, τ=1% |
| `chain_LSTM_CETI_BCP_a05.npy` | MCMC posterior samples — LSTM-CETI, BCP, τ=5% |

---

## How to Reproduce the Results

### Option A — Exact replication (recommended)

Notebooks 02 and 03 can be run directly using the pre-processed data files and the saved MCMC posterior samples included in this repository. No re-estimation is required.

```bash
pip install numpy pandas scipy statsmodels matplotlib
jupyter notebook 02_CETI_Analysis.ipynb
jupyter notebook 03_TailRisk_HorseRace.ipynb
```

### Option B — Full pipeline from raw data

Notebook 01 requires raw Economatica data (bond YTMs, equity prices, market indices) which are **not included** due to data licensing restrictions. These were sourced from Economatica and Peru's SBS regulatory database. If you have access, place the raw files in a `DATA/` folder following the filenames referenced in `01_CETI_Pipeline.ipynb`, then run all three notebooks in order.

---

## Reproducibility Note

The LSTM-AL model is estimated via adaptive MCMC (Roberts & Rosenthal, 2009). The posterior samples used to produce the reported results are archived as `.npy` files in this repository. When these chain files are present, Notebook 03 loads them directly, ensuring exact replication of all reported figures and statistics without re-running the sampler.

Re-running MCMC from scratch will produce numerically equivalent but not bit-identical results due to the stochastic nature of the sampler. All qualitative conclusions are robust to this variation (see Section 3.2.3 of the thesis).

---

## Dependencies

| Package | Version |
|---------|---------|
| Python | 3.10+ |
| numpy | 1.24+ |
| pandas | 2.0+ |
| scipy | 1.10+ |
| statsmodels | 0.14+ |
| matplotlib | 3.7+ |

---

## Data Sources

| Dataset | Source | Included |
|---------|--------|----------|
| Corporate bond YTMs (146 bonds) | Economatica | ✗ Licensed |
| Bank equity prices (BBVA Perú, BCP) | Economatica | ✗ Licensed |
| MSCI EM index, sovereign yields | Economatica | ✗ Licensed |
| Bank NPL ratios | Superintendencia de Banca y Seguros (SBS) Peru | ✓ |
| Financial system index | Superintendencia de Banca y Seguros (SBS) Peru | ✓ |

---

## Authors

- **Jimena Huillca** — Barcelona School of Economics
- **Liqi Yi** — Barcelona School of Economics

Supervised by Prof. Argimiro Arratia Quesada, Barcelona School of Economics, June 2026.
