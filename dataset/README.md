# Training Data

Per the AI Training Guide, keep **raw images organized in separate folders by class**
— do NOT mix them together. This makes it trivial to add a new class later
(e.g. `fire/`, `traffic_accident/`) without disturbing existing data.

```
data/
├── raw_data/
│   ├── pothole/         200–300 photos (day + night, near + far)
│   ├── garbage/         200–300 photos
│   └── streetlight/     200–300 photos
└── labeled/             output of Roboflow export (YOLOv8 format)
```

## Labeling workflow (Roboflow)

1. Create a free Object Detection project on roboflow.com
2. Upload each folder; tag classes: `Pothole`, `Garbage`, `Broken_Streetlight`
3. Draw tight bounding boxes around each instance
4. **Augmentation:** horizontal flip, ±15° rotation, ±25% brightness
5. Generate Version → Export as **YOLOv8**
6. Copy the download snippet into `notebooks/yolov8_training.ipynb`

## Class names

These must match `CLASS_NAMES` in `ml_service/app/main.py`:
- `Pothole`
- `Garbage`
- `Broken_Streetlight`

If you add a class later, update both the Roboflow project *and* `CLASS_NAMES`.
