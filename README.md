# Feel / Frame — Emotion, made visible

A responsive, interactive mini-project website that predicts four emotion labels from text and face images.

## Run it

This site reads its local CSV and model files with `fetch`, so serve the folder instead of double-clicking `index.html`. From this folder, run:

```text
python -m http.server 8000
```

Then visit `http://localhost:8000` in a modern browser. Stop the server with `Ctrl+C` in its terminal.

## The two trained models

- **Text model:** the site builds a Multinomial Naive Bayes model from the included 48 labelled sentences when the page loads.
- **Image model:** a HOG feature extractor and softmax classifier trained from the supplied FER2013 ZIP. It has 20,137 training images and 5,003 held-out test images across four mapped labels: `happy → joy`, `sad → sadness`, `angry → anger`, and `fear → fear`. Other FER2013 labels (`disgust`, `neutral`, `surprise`) are left out to match the four-class project.
- **Held-out image accuracy:** 48.4% across those four classes. Per-class accuracy on the held-out set was joy 66.2%, sadness 22.6%, anger 36.4%, and fear 60.0%. This is a small educational baseline, not a production facial-expression system.

The website includes the trained weights in `image_model.json`. Image predictions run locally in the browser with no large model download and no image upload to a model service. Because training images are small grayscale face crops, predictions are most meaningful with one centered, front-facing face crop. The model estimates visible-expression labels; it cannot determine a person's actual feelings or mental health.

## Retrain the image model

The ZIP should contain FER2013-style folders such as `Fer2013/train/happy/` and `Fer2013/test/angry/`. The project trainer reads it directly:

```text
python train_image_model.py "path/to/NLP.zip"
```

Requirements: Python 3, NumPy, and Pillow. The command writes a new `image_model.json` and reports the held-out test accuracy and confusion matrix.

## Website features

- Text/image tabs, example sentences, image upload and drag-and-drop preview
- Animated prediction readout with four class scores
- Responsive layout, keyboard shortcut (`Ctrl+Enter` / `Cmd+Enter`) for text analysis, and reduced-motion support

## Files

- `index.html` — page content and structure
- `styles.css` — visual design, responsive layout, and animations
- `app.js` — browser-side text training, HOG extraction, and model inference
- `emotion_dataset.csv` — text training data
- `image_model.json` — trained image-model weights and dataset metrics
- `train_image_model.py` — reproducible trainer for the supplied FER2013 ZIP
