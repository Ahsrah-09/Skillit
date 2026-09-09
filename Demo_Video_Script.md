# Demo Video Script (for you to record)

I can't record an actual screen video from here, but here's a ready-to-use
script. Record your screen while following these steps — it should give a
clean 3–4 minute demo.

## Setup Before Recording
- Have `train_model.py` open in an editor (or the notebook, if you prefer).
- Have a few sample MRI images ready to test predictions on.
- Have the training curve and confusion matrix images ready to show.

## Script

**1. Intro (20 seconds)**
"Hi, this is a demo of my capstone project — Alzheimer's Disease
Classification from Brain MRI. It uses a CNN to sort a brain scan into one
of four stages: NonDemented, VeryMildDemented, MildDemented, or
ModerateDemented."

**2. Show the Dataset (30 seconds)**
- Open the dataset folder, show the 4 class folders.
- Mention the image counts and point out the class imbalance
  (ModerateDemented has far fewer images).

**3. Walk Through the Code (60–90 seconds)**
- Scroll through `train_model.py`, briefly pointing at each section:
  - Data split (train/test)
  - Image resizing and augmentation
  - Class weights
  - The CNN model layers
  - Training call
- Keep this part quick — no need to read code line by line.

**4. Show Training Results (45 seconds)**
- Show the training curves image (`training_curves.png`) — accuracy going
  up, loss going down.
- Show the confusion matrix image (`confusion_matrix.png`).
- Say the test accuracy number out loud.

**5. Live Prediction (45 seconds)**
- Run `predict_single_image()` on 2–3 sample images.
- Show the predicted class and confidence for each.

**6. Wrap-up (15 seconds)**
"That's the full pipeline — from raw MRI scans to a trained model that can
classify Alzheimer's stages. Thanks for watching."

## Recording Tips
- Use your OS's built-in screen recorder (Windows: Xbox Game Bar / Win+G;
  Mac: Cmd+Shift+5) or a free tool like OBS Studio.
- Record in short takes if it's easier — you can trim clips together later.
- Keep total length under 5 minutes; reviewers usually prefer short and clear.
