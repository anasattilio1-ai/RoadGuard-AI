# RoadGuard AI --- Pothole Detection

RoadGuard AI is a team hackathon project focused on identifying potholes
in road videos. This repository documents my contribution to the
**computer-vision detection pipeline**, rather than claiming ownership
of the full system.

## My contribution

-   Integrated a **pretrained YOLOv8 pothole-detection model**.
-   Used **ByteTrack** through Ultralytics to track detections across
    consecutive video frames.
-   Processed road video frame by frame with OpenCV.
-   Generated an annotated output video.
-   Saved a full annotated frame when a track produced a
    higher-confidence detection than the previously saved frame for that
    track ID.
-   Exported detection metadata to JSON, including confidence, frame
    number, timestamp, track ID, and image path.

**Model-training note:** I did not train or fine-tune the model. I
downloaded and integrated the existing model weights into the
video-processing workflow.

## Model source

-   Hugging Face model repository:
    https://huggingface.co/Samdutse/pothole-yolov8
-   Weights filename: `best.pt`
-   The script downloads the weights with `huggingface_hub` at runtime.

Please review the model repository's license and usage terms before
redistributing its weights. This repository does not include `best.pt`.

## Technologies

-   Python
-   YOLOv8 / Ultralytics
-   ByteTrack
-   OpenCV
-   Hugging Face Hub
-   Google Colab and Google Drive

## How it works

1.  Mount Google Drive in Colab.
2.  Download the pretrained `best.pt` model from Hugging Face.
3.  Upload a road video.
4.  Process frames with YOLOv8 and ByteTrack using a confidence
    threshold of `0.35`.
5.  Write an annotated video and save selected full-frame images for
    tracked detections.
6.  Copy images to Google Drive and export event metadata to
    `results.json`.

## Run in Google Colab

This script depends on Google Colab-specific features
(`google.colab.drive` and `google.colab.files`), so it is not intended
to run unchanged as a normal local Python script.

1.  Open a Google Colab notebook.

2.  Run this install command in a separate cell:

    ``` python
    %pip install -q ultralytics huggingface_hub opencv-python
    ```

3.  Upload `pothole_yolov8.py` to the Colab session or paste its
    contents into a code cell.

4.  Run the script and authorize Google Drive access when prompted.

5.  Upload a road video when the file-upload prompt appears.

The script creates a Google Drive folder at:

`/content/drive/MyDrive/hackthon 2026`

## Outputs

-   `pothole_tracking.mp4` --- annotated video.
-   `pothole_images/` --- selected full annotated frames, named by track
    ID.
-   `pothole_images.zip` --- ZIP archive of saved images.
-   `results.json` --- video metadata and one saved event record per
    track ID.

Each event includes fields such as `track_id`, `confidence`,
`frame_number`, `timestamp_seconds`, `image_name`, and
`image_drive_path`. **`image_drive_path` is a path inside Google Drive,
not a public shareable URL.**

## Important limitations

-   The pipeline does **not** determine GPS coordinates or the
    real-world location of a pothole.
-   It does **not** send driver alerts or automatically submit reports
    to a municipality.
-   A tracking ID is not guaranteed to represent one unique physical
    pothole; tracking can vary with video quality and scene conditions.
-   Saved images are full annotated frames, not cropped pothole-only
    images.
-   Results depend on the pretrained model, video conditions, and
    tracking behavior.

## Possible future improvements

-   Add GPS/geolocation support from a suitable data source.
-   Add duplicate-event handling and validation for detections across
    frames.
-   Generate structured reports and integrate a notification/reporting
    service.

## Acknowledgment

The pothole-detection weights are provided by the model repository
linked above. Please consult its model card for the original model
details, evaluation claims, and license information.
