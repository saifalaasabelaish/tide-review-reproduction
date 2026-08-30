# TiDE Review

A seminar report reviewing and reproducing **"Long-term Forecasting with TiDE: Time-series Dense Encoder"** (Das et al., ICLR 2024), written for the *Data Analytics Seminar I* at ISMLL, Universität Hildesheim.

📄 [Read the full report](report/tide_report.pdf) · 📓 [Reproduction notebook](notebook/tide_reproduction.ipynb)

## What's in this repo

- **`report/`** — the full written report (PDF): paper summary, architecture walkthrough, experimental results, critical discussion, and the reproduction section below.
- **`notebook/`** — a Jupyter notebook reproducing TiDE's multivariate forecasting results on the ETTh1 dataset.

## About the paper

TiDE (Time-series Dense Encoder) is an MLP-based encoder-decoder architecture for long-term time-series forecasting. Instead of self-attention, it uses dense residual blocks to encode historical observations and covariates, then decodes them with a temporal decoder — while a global linear residual connection ensures a purely linear forecasting model is always a special case of the architecture. The paper shows TiDE matches or beats strong Transformer-based baselines (PatchTST, FEDformer, Autoformer, Informer) and DLinear across seven long-term forecasting benchmarks, while training 5–10× faster.

- Paper: [arXiv:2304.08424](https://arxiv.org/abs/2304.08424) (ICLR 2024)
- Official code: [google-research/tide](https://github.com/google-research/google-research/tree/master/tide)

## Reproduction

Using the authors' official implementation and `scripts/etth1.sh` configuration, I reproduced TiDE's multivariate forecasting results on **ETTh1** across all four horizons (96, 192, 336, 720), with one deliberate deviation: training was capped at a fixed **5 epochs** instead of the original early-stopping schedule (minimum 5 epochs, run until convergence), due to compute constraints.

| Horizon | Paper MSE | Paper MAE | Reproduced MSE (5 epochs) | Reproduced MAE (5 epochs) |
|---|---|---|---|---|
| 96  | 0.375 | 0.398 | 1.955 | 0.979 |
| 192 | 0.412 | 0.422 | 1.795 | 0.941 |
| 336 | 0.435 | 0.433 | 1.339 | 0.803 |
| 720 | 0.454 | 0.465 | 1.157 | 0.755 |

**Result:** the reproduction does **not** match the paper — errors are 2–5× higher across all horizons, and the trend across horizons is inverted (decreasing rather than increasing with horizon length). This is attributed to the truncated training budget preventing convergence, rather than any flaw in the architecture itself. Full discussion is in the report.

## Citation

```bibtex
@inproceedings{das2024tide,
  author    = {Das, Abhimanyu and Kong, Weihao and Leach, Andrew and Mathur, Shaan and Sen, Rajat and Yu, Rose},
  title     = {Long-term Forecasting with {TiDE}: Time-series Dense Encoder},
  booktitle = {International Conference on Learning Representations (ICLR)},
  year      = {2024}
}
```

## Author

Safieddin Sabealeish — MSc student, Universität Hildesheim