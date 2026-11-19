# Smart Vehicle Security Monitoring System (Computer Vision)

An **academic video-analysis prototype** for identifying people and cars near parked vehicles and displaying alerts for potentially suspicious proximity or abnormal motion.

**Course:** AI385 – Computer Vision, University of Prince Mugrin (Spring 2026).  
**Team:** Shog Aloufi, Rahaf Altir, Shahad Nasser.

## What it does

- **Vehicle and person detection:** YOLOv8s finds cars and people in each video frame; OpenCV draws bounding boxes, confidence scores, and per-frame counts.
- **Suspicious proximity alert:** A simple spatial heuristic estimates whether a detected person is near the primary vehicle. An on-screen alert appears after **12 seconds** of continuous proximity at the video's frame rate.
- **Possible accident alert:** Frame differencing flags significant motion near a detected car over several frames. This is a **motion-based warning**, **not reliable collision identification**.
- Outputs processed `.mp4` files for review.

## Example pipeline

`Video → OpenCV frames → YOLOv8s car/person detections → Proximity and motion rules → Video overlays and warning messages`

## My contribution (Shog Aloufi)

Implemented **car and person detection using YOLOv8s and OpenCV**. Participated with the team in testing, reporting, and final demo preparation. Proximity alert logic and accident alert logic were developed by other team members.

## Run locally

1. Install Python 3.10+ and dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Place the following **authorized** sample videos in the same directory as the notebook:
   - `normal_case1.mp4`
   - `suspicious_case7.mp4`
   - `accident_case3.mp4`
3. Open `AI385_CV_Project.ipynb` in Jupyter and run cells in order. The notebook loads `YOLO("yolov8s.pt")` (Ultralytics may download the model weights on first run). Internet may be required for the initial download.
4. Output videos are saved next to the notebook. Press `q` to stop playback.

**Environment note:** The notebook uses `cv2.imshow`, which requires a local graphical desktop; it may fail in Colab, cloud notebooks, and other headless systems.

## Limitations and responsible use

- It **does not prove theft, criminal intent, or an actual vehicle collision**. The alerts are heuristic signals for possible review.
- The suspicious-behavior timer is frame-count-based; it does not track individual identities across frames, so a changing nearby person may still increment the timer.
- The approximate "driver-side" region is based on image-box coordinates, not verified vehicle orientation.
- The accident detector uses basic frame differences; camera movement, lighting, and annotated-frame changes may trigger false motion alerts. It has **not** been validated as a real-world accident detector.
- Small/distant objects, crowding, occlusion, and camera angles affect predictions.
- Only three demonstration videos were reported; **no benchmark accuracy, precision, recall, or F1 score is claimed**.
- The code processes prerecorded test videos; live-camera use is an example option in the notebook but was not documented as experimentally validated.
- Keep CCTV footage private unless you have permission and the right to redistribute it. **Do not use this prototype for consequential safety or law-enforcement decisions.**

## Repository contents

- `AI385_CV_Project.ipynb` — original student project code, with notebook execution outputs cleared for publication
- `requirements.txt` — Python dependencies
- `.gitignore` — excludes weights, video files and notebook checkpoints

**Not included:** source CCTV videos, generated output videos, and `yolov8s.pt` weights. They were intentionally omitted for repository size, redistribution/privacy considerations, and reproducibility hygiene. The original course report refers to `AI385_CV_Project_Final_Simple.ipynb`; the uploaded project archive instead contained a notebook named `AI385_CV_Project.ipynb`.

## Notes for future improvement

Use a clean original frame for motion comparison, apply object tracking to maintain person identities, add scene-specific calibration and systematic evaluation, and handle video writers/GUI backends robustly.
