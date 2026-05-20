# COS760 Group 57 — MGT Detection: Quickstart Guide

## What this project does
Fine-tunes two transformer models to classify text as human-written or machine-generated,
across isiZulu, English, and cross-lingual settings.

---

## What you need before starting

- All 9 CSV files (zul/eng/combined × train/val/test)
- A Google account (for Colab)

---

## Two models, both on Google Colab

| Model | Purpose | Notebook |
|---|---|---|
| **AfroXLMR-base** | Primary model (RQ1 & RQ2) | `AfroXLMR_Colab.ipynb` |
| **XLM-R base** | Baseline — run after AfroXLMR | `XLMR_Colab.ipynb` |

Run AfroXLMR first. Once you have its results, open a fresh session and run XLM-R.
You need results from both to compare in your report.

---

## First-time setup (same for both notebooks)

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Upload the notebook via **File → Upload notebook**
3. Go to **Runtime → Change runtime type → T4 GPU → Save**

---

## Running an experiment

The two notebooks are identical in structure — same cells, same steps, same outputs.
The only difference is the model loaded in Cell 4.

### Steps (Cells 1 → 11)

1. **Cell 1** — installs packages (~2 min)
2. **Cell 2** — confirms GPU is active. Should print `Tesla T4 | 15.8 GB`
3. **Cell 3** — uploads your CSV files (select all 9 at once)
4. **Cell 4** — change `EXPERIMENT` to your target run, then run it:
   - `'zul'` → isiZulu only
   - `'eng'` → English only
   - `'combined'` → both languages
   - `'cross'` → train on English, test on Zulu
5. **Cells 5 → 10** — run in order (~20 min per experiment)
6. **Cell 11** — automatically downloads your results CSV

### To run a different experiment
Change `EXPERIMENT` in Cell 4 and re-run from Cell 5 onward.
You do not need to restart the runtime, and you do not need to re-upload your CSV files.

### Running all 4 at once (Cell 12 — optional)
Only use Cell 12 if you have a stable session and ~2 hours.
It is safer to run one experiment at a time.

---

## Recommended order

Run AfroXLMR and XLM-R in **separate Colab sessions** — do not run both notebooks at the same time.

1. Open `AfroXLMR_Colab.ipynb` → run all 4 experiments → download results
2. Start a new session → open `XLMR_Colab.ipynb` → run all 4 experiments → download results

**Why separate sessions?** Each model uses ~1.1 GB of GPU memory on top of training.
Keeping them separate gives each model the full 15 GB T4 and avoids session crashes.

---

## Reading the training output

While training you will see output like this after each epoch:

```
{'eval_f1': 0.8821, 'eval_accuracy': 0.8893, 'eval_roc_auc': 0.9201, 'epoch': 2.0}
```

- **eval_f1** — the number that matters most. Should increase each epoch.
- **eval_roc_auc** — secondary metric. Should be above 0.85 for a good model.
- If F1 stops improving for 2 epochs, training stops early and restores the best checkpoint.

At the end you will see a full test set report:

```
              precision    recall  f1-score
       Human     0.976      0.975     0.975
     Machine     0.975      0.976     0.975
```

---

## What good results look like

| Experiment | Expected F1 (AfroXLMR) | Expected F1 (XLM-R) |
|---|---|---|
| English | 0.97 – 0.99 | 0.97 – 0.99 |
| Zulu | 0.95 – 0.98 | 0.90 – 0.95 |
| Combined | 0.95 – 0.98 | 0.92 – 0.96 |
| Cross-lingual | 0.70 – 0.80 | 0.65 – 0.75 |

AfroXLMR should outperform XLM-R on Zulu and cross-lingual — this gap is the core finding for your report.
Cross-lingual will be the lowest for both models — this is expected and is the key finding for RQ2.

---

## Output files produced per experiment

| File | Contents |
|---|---|
| `test_results_<lang>.csv` | Accuracy, F1, precision, recall, ROC-AUC for that experiment |
| `all_afroxlmr_results.csv` | All 4 AfroXLMR experiments in one table (Cell 12 only) |
| `all_xlmr_results.csv` | All 4 XLM-R experiments in one table (Cell 12 only) |
| `best_model/` | Saved model checkpoint |
