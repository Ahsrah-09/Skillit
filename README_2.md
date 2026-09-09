# Alzheimer's Disease Classification from Brain MRI

A Capstone Project in Medical Image Processing.

## What This Project Does

Alzheimer's disease is often found late, after damage to the brain has
already happened. This project trains a Convolutional Neural Network (CNN)
to look at a brain MRI scan and sort it into one of four stages:

- **NonDemented** — no signs of Alzheimer's
- **VeryMildDemented** — very early signs
- **MildDemented** — mild signs
- **ModerateDemented** — moderate signs

The goal is to help doctors catch the disease earlier and more consistently.

## Dataset

- **Name:** Alzheimer's Dataset (4 class of Images)
- **Source:** Kaggle
- **Link:** https://www.kaggle.com/datasets/rajmohan096/alzheimer-mri-preprocessed-dataset

The dataset has 4 folders, one per class, with grayscale brain MRI images
already resized to a common size.

| Class | Number of Images |
|---|---|
| NonDemented | 3,200 |
| VeryMildDemented | 2,240 |
| MildDemented | 896 |
| ModerateDemented | 64 |

Notice the classes are not balanced — ModerateDemented has far fewer images
than the others. The code handles this using **class weights**, which tell
the model to pay more attention to the rare classes during training.

## Tools Used

- Python
- TensorFlow / Keras (to build and train the CNN)
- OpenCV (to read and resize images)
- NumPy (for number crunching)
- Matplotlib (for charts)
- scikit-learn (for splitting data and measuring results)

## How the Model Works

1. Load the dataset and split it into training and test folders (80% train, 20% test).
2. Resize every image to 150x150 pixels and scale pixel values to 0–1.
3. Slightly rotate, zoom, and flip training images (data augmentation) so the
   model does not just memorize the exact pictures.
4. Feed images through a CNN with 3 convolution layers, each followed by
   pooling, then a dense layer, then a final layer that outputs one of the
   4 classes.
5. Train for up to 20 rounds (epochs), stopping early if the model stops
   improving.
6. Test the model on images it has never seen and report accuracy.
7. Save the trained model to a file so it can be reused later.

## How to Run It

1. Download the dataset from the Kaggle link above.
2. Unzip it and note the folder path.
3. Open `train_model.py` and set `DATASET_PATH` to that folder.
4. Install the requirements:
   ```
   pip install tensorflow opencv-python numpy matplotlib scikit-learn
   ```
5. Run the script:
   ```
   python train_model.py
   ```
6. When it finishes, you will have:
   - `alzheimers_classification_model.keras` (the trained model)
   - `training_curves.png` (accuracy/loss graphs)
   - `confusion_matrix.png` (how well the model told classes apart)

## Known Issue in the Original Notebook (Fixed and Re-Tested)

In an earlier run of the notebook, the training/validation data loaders were
created **before** the code that actually built the train/test folders. This
meant the model was trained on empty or wrong data, and it only reached
**3.43% test accuracy** — no better than guessing.

In `train_model.py` (the cleaned-up version in this submission), the
folder-splitting step now runs first, which fixes this problem. A later,
corrected run of the notebook confirms the fix works:

- **Test Accuracy: 52.15%** (up from 3.43%)
- **Test Loss: 1.0026**

### Per-Class Results (corrected run)

| Class | Precision | Recall | F1-score | Support |
|---|---|---|---|---|
| MildDemented | 0.31 | 0.59 | 0.40 | 180 |
| ModerateDemented | 0.38 | 0.23 | 0.29 | 13 |
| NonDemented | 0.75 | 0.56 | 0.64 | 640 |
| VeryMildDemented | 0.45 | 0.45 | 0.45 | 448 |

**What this means in simple words:**
- The model is best at spotting **NonDemented** scans (75% precision) — when
  it says "no signs," it's usually right.
- It struggles most with **ModerateDemented**, mainly because there are only
  64 images for that class in total. With so few examples, the model does
  not get enough chances to learn what that stage looks like.
- **MildDemented** and **VeryMildDemented** sit in the middle — these two
  stages likely look visually similar, which makes them easy to confuse.

52.15% is a solid proof-of-concept for a from-scratch CNN, but it is not
yet accurate enough for real clinical use. See "Future Improvements" below
for ways to push this higher.

## Files in This Submission

| File | Purpose |
|---|---|
| `train_model.py` | Main source code — cleaned and fixed |
| `Alzheimer's_Disease_Classification.ipynb` | Original notebook |
| `Project_Report.docx` | Full write-up of the project |
| `Presentation.pptx` | Slide deck summary |
| `README.md` | This file |

## Future Improvements

- Collect more ModerateDemented images, or use targeted augmentation for
  just that class, since it currently has only 64 images total.
- Try transfer learning with a pre-trained model (e.g. InceptionV3) instead
  of training a CNN from scratch — this usually needs far fewer images to
  reach good accuracy.
- Add explainability tools (e.g. Grad-CAM) so doctors can see which part of
  the scan led to a prediction.
- Test the model on MRI scans from a different hospital or scanner to check
  it generalizes well.
- Train for more epochs with a learning-rate schedule, since the validation
  accuracy was still moving (not fully flattened) when early stopping
  kicked in.

## Limits of This Project

- It is a screening/support tool, not a medical diagnosis tool.
- The ModerateDemented class has very few images (64), so predictions for
  that class are less reliable — the model's recall for this class is only
  23%.
- Current accuracy (52.15%) is a good proof of concept, not yet at the
  level needed for real clinical decisions.
