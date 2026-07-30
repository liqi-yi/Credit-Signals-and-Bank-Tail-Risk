# ICAIF'26 Replication Materials

   This folder contains the updated replication code and data for:
   Huillca, J. & Yi, L. (2026). Forecasting Bank Equity Tail Risk in
   Emerging Markets — ICAIF'26.

   Differences from the root-level (thesis) version:
   - Bank-issued bonds excluded from the panel (76 non-bank bonds vs.
     146 in the thesis version)
   - MCMC estimation uses a deterministic per-(model, asset, quantile)
     random seed; two independent full re-runs produce byte-identical
     posterior chains
   - See RUN_INSTRUCTIONS.md in this folder for exact run steps

   Root-level files are unchanged and correspond to the original BSE
   thesis version.
