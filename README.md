# Classification-solar-panel-defects

Classify solar panel defects using deep learning (TensorFlow/Keras).

## Project overview
This repository contains an end-to-end image classification pipeline to identify common solar panel surface conditions/defects from images.

## Classes
The model is trained to classify images into **6 classes**:
- Bird-drop
- Clean
- Dusty
- Electrical-damage
- Physical-Damage
- Snow-Covered

## Repository contents
- `Solar_Panel_Classification.ipynb` — Training experiments (baseline CNN, regularization/augmentation attempts, and transfer learning with MobileNetV2).
- `app.py` — Simple app / inference entry point.
- `requirements.txt` — Python dependencies.
- `Deployment.txt` — Deployment notes.

## Dataset
The notebook expects an image folder structured by class (one subfolder per class).

In the notebook, the dataset path is set to:

```python
DATASET_DIR = "/content/drive/MyDrive/Krish AI/Solar Panel Classification"
```

If you run locally, update `DATASET_DIR` to your dataset location.

## How to run
### 1) Create environment & install dependencies

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
pip install -r requirements.txt
```

### 2) Train (Notebook)
Open and run:
- `Solar_Panel_Classification.ipynb`

### 3) Inference / App
Run:

```bash
python app.py
```

> Note: You may need to export/save a trained model from the notebook (e.g., `model.save(...)`) and load it in `app.py`, depending on your current workflow.

## Notes / next improvements
- Add a train/val/test split and report a confusion matrix + per-class metrics.
- Save best model checkpoints and include reproducible training scripts.
- Consider fine-tuning MobileNetV2 (unfreeze top layers) once the head is stable.

## License
See `LICENSE`.