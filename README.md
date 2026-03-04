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

## How to run (local)
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

## Run on AWS
Below are common AWS paths depending on whether you want to **train** or only **run inference**.

### Option A — Train on Amazon SageMaker (recommended)
1. Upload the dataset to S3, keeping the folder-per-class structure:
   - `s3://<bucket>/data/Bird-drop/...`
   - `s3://<bucket>/data/Clean/...`
   - etc.
2. Start SageMaker Studio / Notebook instance (GPU instance recommended for faster training).
3. Sync data from S3 to the notebook instance:

```bash
aws s3 sync s3://<bucket>/data /home/ec2-user/SageMaker/data
```

4. In the notebook, set:

```python
DATASET_DIR = "/home/ec2-user/SageMaker/data"
```

5. Train and save the model:

```python
model.save("model.keras")
```

6. (Optional) Upload model artifact back to S3:

```bash
aws s3 cp model.keras s3://<bucket>/models/model.keras
```

### Option B — Deploy `app.py` on an EC2 instance
1. Launch an Ubuntu EC2 instance.
2. Install dependencies and clone the repo:

```bash
sudo apt-get update
sudo apt-get install -y python3-pip python3-venv git

git clone https://github.com/chatkausik/Classification-solar-panel-defects.git
cd Classification-solar-panel-defects
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

3. Download your saved model (example: from S3):

```bash
sudo apt-get install -y awscli
aws s3 cp s3://<bucket>/models/model.keras ./model.keras
```

4. Run the app:

```bash
python app.py
```

> If your app is a web server, ensure it binds to `0.0.0.0` and the Security Group allows inbound traffic on the app port.

## Notes / next improvements
- Add a train/val/test split and report a confusion matrix + per-class metrics.
- Save best model checkpoints and include reproducible training scripts.
- Consider fine-tuning MobileNetV2 (unfreeze top layers) once the head is stable.

## License
See `LICENSE`.