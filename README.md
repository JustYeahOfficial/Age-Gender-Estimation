# Age & Gender Estimator

A single-file, offline-friendly web app that estimates **age** and **gender** from your webcam or an uploaded photo. Everything runs locally in your browser — no account, no server, no uploads.

## Quick start

Camera access needs a real server origin (not a plain double-clicked file). Easiest path:

```sh
python -m http.server
# then visit http://localhost:8000/age-gender.html
```

You *can* still open the file directly by double-clicking it — photo upload and drag-and-drop will work fine — but most browsers block camera access on a bare `file://` page, and the app will show a banner explaining that if it detects it.

## Features

- Live webcam detection with real-time on-screen results
- **Take picture & detect** — freezes a frame and analyzes it (no file is saved or uploaded)
- Upload a photo, or just **drag and drop** one onto the frame
- **Multiple faces at once** — group photos get a separate age/gender result per face, numbered to match the boxes drawn on the picture
- **Switch camera** button appears automatically on devices with more than one camera (e.g. front/back on phones)
- **Save annotated photo** — download the picture with the result boxes drawn on it, generated entirely on-device
- Large uploads are automatically downscaled for speed before analysis
- A banner warns you if the page was opened in a way that will block camera access, so you're not left guessing

## How it works

- Uses [face-api.js](https://github.com/justadudewhohacks/face-api.js) with the TinyFaceDetector + AgeGenderNet models, loaded from a CDN with automatic fallback to backup mirrors if one is slow or unreachable
- Live video uses a smaller/faster detector pass; uploaded photos and captured stills use a larger, more thorough pass, since they're not time-constrained
- Gender output is **male or female** only — the model is a binary classifier and can only guess from appearance, so results say nothing about anyone's identity or sexuality
- **Accuracy limits:** glasses/sunglasses, hats, beards, poor lighting, and tilted faces reduce accuracy; treat age as a rough estimate (shown as a range, not a single number)
- Nationality/ethnicity is deliberately **not** supported — appearance cannot reliably determine nationality, and any attempt to guess it from a face would just be reproducing stereotypes, not measuring anything real

## Privacy

- All face analysis happens in your browser with TensorFlow.js (WebGL)
- Your camera feed is never recorded, stored, or transmitted
- Uploaded photos are read into memory and analyzed locally
- The "save annotated photo" feature draws directly to a canvas and triggers a normal browser download — the image never touches a server
- Only the AI model weights (~2 MB) are downloaded, from a public CDN, once per browser (then cached)

## Disclaimer

This is a fun/demo app. Age estimates are approximations and gender output reflects a limited machine-learning model, not a person's identity. Do not use it for anything serious, screening, or profiling.
