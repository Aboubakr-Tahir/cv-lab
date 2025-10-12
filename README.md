# cv-lab

Personal Computer Vision lab for experiments (Google Colab + GitHub).
Small ideas live here; big mature projects can graduate to their own repos.

## Structure

---

```text
cv-lab/
├── notebooks/      # Jupyter/Colab notebooks for experiments
├── data/           # Dataset fetch scripts (no raw data in Git)
├── experiments/    # Logs, plots, small results (NO large models)
├── .gitignore
└── README.md
```

---

## Usage (Colab workflow)

1.  Open or create a notebook in **Colab**.
2.  Go to **File → Save a copy in GitHub** and choose this repo and the `notebooks/` folder.
3.  Keep datasets **outside** the repo (use Drive or Colab’s `/content` or `/kaggle/working`). Use scripts or notebook cells to fetch and prepare data at runtime.
4.  Save small artifacts (plots, CSV logs) under `experiments/<date-topic>/`.
5.  Store large model files in Google Drive, Hugging Face Hub, or **W&B Artifacts** (not in Git).

---

## Tips

- Start from the official **PyTorch Tutorials** (Transfer Learning, Finetuning).
- Name notebooks sequentially: `01_*`, `02_*` for easy browsing.
- Each experiment folder should include a short `README.md` with its goal, setup, results (including screenshots), and links to dashboards (e.g., W&B).

---

## Experiments

### 01 — Transfer Learning: Ants & Bees

- **Notebook:** `notebooks/01_transfer_learning_bees_ants.ipynb`
- **Summary:** Fine-tunes ResNet-18 and compares _feature extraction_ vs _full fine-tuning_ on the Hymenoptera dataset.
- **Dataset fetch:**
  - Windows PowerShell: `data/get_hymenoptera.ps1`
  - macOS/Linux (or Git Bash): `data/get_hymenoptera.sh`

### 02 — Transfer Learning: Dogs vs Cats (Kaggle)

- **Notebook:** `notebooks/02_transfer_learning_dogs_cats.ipynb`
- **Goal:** Fine-tune a pretrained **ResNet-18**, hit **>95%** validation accuracy, log to **Weights & Biases**

#### What this notebook does

- Loads the Kaggle **Microsoft Cats vs Dogs** dataset with `kagglehub`.
- Cleans corrupted images found in the dataset.
- Splits data **once** (80/20) using two `ImageFolder`s with separate transforms:
  - **train**: random resized crop + horizontal flip + normalization.
  - **val**: resize + center crop + normalization.
- Trains two runs:
  - **Full fine-tuning** of ResNet-18 (reaches ~99% val acc).
  - **Feature extraction** (freezes backbone, trains `fc` layer) (reaches ~98% val acc).
- Logs all metrics to **W&B** (loss/accuracy curves).

- **W&B project:** `cats-vs-dogs-resnet`

#### Notes / Gotchas

- Do **not** store checkpoints in `/tmp` on Colab; keep the best model weights in memory during training using `best_model_wts = deepcopy(model.state_dict())`, then save to a persistent path like `/content/resnet18_catsdogs.pt` or as a **W&B Artifact**.
- Set `num_workers=2` and use `pin_memory=True` on GPU for better performance.
- If you see a **“Truncated File Read”** warning, it’s a bad image; the notebook includes a cleaning step for this.

## Datasets (no raw data in Git)

### Hymenoptera (ants & bees)

- **Windows / PowerShell**
  ```powershell
  cd data
  powershell -ExecutionPolicy Bypass -File .\get_hymenoptera.ps1
  ```
- **macOS/Linux (or Git Bash)**
  ```bash
  cd data
  bash ./get_hymenoptera.sh
  ```

### Dogs vs Cats (Kaggle)

- Download via `kagglehub` inside the notebook.
- Work from `/kaggle/working/` (writable) rather than `/kaggle/input/` (read-only).
- Keep your Kaggle API token (`kaggle.json`) private.

---

## Results Snapshot

### Dogs vs Cats (ResNet-18)

- **Full fine-tuning:** ~99% validation accuracy
- **Feature extraction:** ~98% validation accuracy
- **W&B:** Loss/accuracy curves and run configurations successfully logged.

_(Place screenshots under `experiments/2025-10-dogs-cats/` if desired.)_

---

## Next Up (Planned)

### 03 — Real-world Detection on Your Own Data

- Collect 150–500 images (from a phone), label them on Roboflow (in YOLO format), and train a YOLOv8 model.
- Compare mAP vs. model sizes and note the trade-offs (speed vs. size vs. accuracy).
- Keep the same repository layout: `notebooks/03_*`, logs in `experiments/...`, and large files in Drive/HF/W&B.

---

## References

- PyTorch Tutorials — Transfer Learning / Finetuning
- torchvision models — ResNet-18 weights & transforms
- Weights & Biases quickstart (experiment tracking)
