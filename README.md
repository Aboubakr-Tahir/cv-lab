# cv-lab

Personal Computer Vision lab for experiments (Google Colab + GitHub).  
Small ideas live here; big mature projects can graduate to their own repos.

## Structure

```
cv-lab/
├── notebooks/        # Jupyter/Colab notebooks for experiments
├── data/             # dataset fetch scripts (no raw data in Git)
├── experiments/      # logs, plots, small results (NO large models)
├── .gitignore
└── README.md
```

## Usage (Colab workflow)

1. Open or create a notebook in Colab.
2. **File → Save a copy in GitHub** → choose this repo and `notebooks/` folder.
3. Keep datasets outside the repo (use Google Drive or Colab's `/content`). Use scripts in `data/` to fetch datasets when a notebook starts.
4. Save small artifacts (plots, CSV logs) under `experiments/<date-topic>/`. Store large model files in Drive/HF Hub.

## Tips

- Follow PyTorch tutorials to start fast: https://docs.pytorch.org/tutorials/
- Name notebooks like `01_cifar10_baseline.ipynb`, `02_augmentations.ipynb` to keep order.
- Each experiment folder should include a short `README.md` with a summary and results screenshots.

## Experiments so far

- `01_transfer_learning_bees_ants.ipynb` (notebooks/)  
  Fine-tunes ResNet18 and tests feature extractor vs. full fine-tuning on ants/bees dataset.  
  Dataset: `data/get_hymenoptera.ps1` (Windows) or `data/get_hymenoptera.sh` (macOS/Linux).

## Datasets

We don’t store raw data in Git. Use scripts in `data/` to fetch.

### Hymenoptera (ants & bees)

- **Windows / PowerShell**

  ```powershell
  cd data
  powershell -ExecutionPolicy Bypass -File .\get_hymenoptera.ps1

  ```

- **macOS/Linux (or Git Bash)**

  ```powershell
  cd data
  bash ./get_hymenoptera.sh


  ```
