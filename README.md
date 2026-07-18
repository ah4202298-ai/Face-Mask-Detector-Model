# Face-State Monitor

A browser-based image classifier that detects three face states in real time from a webcam feed:

- **Mask** — face wearing a mask
- **No Mask** — bare, unobstructed face
- **Occluded** — face blocked by a hand, paper, phone, or other object

The model was trained with [Google's Teachable Machine](https://teachablemachine.withgoogle.com/) and runs entirely client-side using TensorFlow.js — no frames are uploaded or sent to a server. Detections are rendered as a live event log, styled like a SOC/SIEM console (timestamp, severity level, message, confidence).

## Demo

Once deployed on GitHub Pages (see below), open the page, click **Start monitoring**, and grant camera access. The right-hand panel logs each state change as an event:

```
14:32:07  INFO   Face mask detected            97%
14:32:11  ALERT  No face mask detected         89%
14:32:15  WARN   Face obstructed (hand/paper)  91%
```

## Project structure

```
.
├── index.html          # single-file demo app (UI + inference logic)
├── model/              # your exported Teachable Machine model goes here
│   ├── model.json
│   ├── weights.bin
│   └── metadata.json
└── README.md
```

## How to use this repo

1. **Train your model** on [teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com/) with three classes named exactly:
   - `Mask`
   - `No Mask`
   - `Occluded`

   (If you use different class names, update the `CLASS_CONFIG` object near the top of the script in `index.html`.)

2. **Export** the model: `Export Model` → `Tensorflow.js` → `Download`. This gives you `model.json`, one or more `.bin` weight files, and `metadata.json`.

3. **Add the files** to a `model/` folder at the root of this repo, matching the structure above.

4. **Push to GitHub**, then enable GitHub Pages:
   - Go to **Settings → Pages**
   - Under **Source**, select the `main` branch and `/ (root)` folder
   - Save — GitHub will give you a live URL (e.g. `https://<username>.github.io/<repo>/`)

5. Open the live URL, allow camera access, and the log will start populating automatically.

## Dataset sources used for training

- [Face Mask Detection Dataset (andrewmvd)](https://www.kaggle.com/datasets/andrewmvd/face-mask-detection) — with_mask / without_mask / mask_worn_incorrectly, with bounding boxes
- [Face Mask 12K Images Dataset](https://www.kaggle.com/datasets/ashishjangra27/face-mask-12k-images-dataset) — ~12,000 pre-split images
- [Face Mask Dataset (shiekhburhan)](https://www.kaggle.com/datasets/shiekhburhan/face-mask-dataset) — includes occlusion variants (mask on chin, hand/hair covering face)
- Custom webcam captures (via Teachable Machine's built-in webcam capture) for the "Occluded" class — paper, hand, and phone held in front of the face at varying angles and distances

## Notes on accuracy

- The "Occluded" class benefits most from your own webcam captures, since public datasets rarely cover this case directly.
- Vary lighting, distance, and background across all classes so the model learns face state rather than background context.
- If a class is consistently misclassified, add more varied examples for it rather than increasing training epochs.

## Tech stack

- [Teachable Machine](https://teachablemachine.withgoogle.com/) — model training
- [TensorFlow.js](https://www.tensorflow.org/js) + `@teachablemachine/image` — in-browser inference
- Vanilla HTML/CSS/JS — no build step required
- GitHub Pages — hosting

## License

Add a license of your choice (e.g. MIT) if you plan to make this repo public.
