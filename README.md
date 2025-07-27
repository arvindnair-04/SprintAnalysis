
# 100m Sprint Analysis – Paris 2024 Olympics

This project studies elite sprinters from the **Paris 2024 Olympic 100m Final** using advanced video-based motion analysis.
With just a race video—no sensors or special equipment—we extract 3D skeleton data and measure how each athlete moves.
We use AI pose estimation tools to track key body joints (like shoulders, hips, knees, and ankles) in every frame. This creates a stick-figure version of the athlete’s motion, which we then analyze to calculate detailed performance metrics.

---

## Overview

This project analyzes how Olympic sprinters move by using 3D skeleton data from race videos. It turns visual motion into measurable performance insights:

- Tracks Leg Movement: Measures ankle-to-hip distance to identify leg extension during each stride.

- Detects Ground Contact & Air Time: Identifies when each foot is on the ground or in the air using a rule-based method.

- Calculates Key Metrics: Computes step length, step rate, air/contact time, and speed per stride.

- Compares Multiple Athletes: Runs full analysis on 8 Olympic finalists to study and compare their biomechanics.


This approach transforms video into data-driven insights—making elite sprint mechanics understandable and measurable.

---

## Methodology

### 1. Pose Data Preprocessing
- Input: A .csv file containing 3D joint positions (X, Y, Z) for 33 landmarks per video frame (from tools like MediaPipe or OpenPose).

- Handling Missing Data: Gaps from undetected joints are filled using linear interpolation.

- Outlier Correction: Sudden, unrealistic joint jumps are smoothed using filtering techniques.

- Result: A clean, continuous dataset that accurately reflects each athlete’s movement.

### 2. Smoothing & Peak Detection
#### Leg Extension Tracking:
We calculate:

diff1 = |midhip_z - right_ankle_z|

diff2 = |midhip_z - left_ankle_z|
These measure vertical distance between hips and ankles to reflect leg motion.


#### Signal Smoothing:
The data is cleaned with a Savitzky–Golay filter to reduce noise while preserving movement shape.

#### Peak Detection (Foot Contact):
Peaks above a threshold indicate leg extension moments, marking the start of ground contact for each leg.

This lets us track stride timing and detect foot-ground interaction using only joint position data.


### 3. Analysis of Contact vs. Air
Using the detected leg extension peaks, we classify each frame into one of three movement phases:

- Contact Phase: From the peak until the leg’s motion drops below a threshold.

- Air Phase: When both feet are off the ground (no leg in contact).

- Double Contact: When both legs touch the ground—usually seen only at race start.


We assign binary labels (1 = contact, 0 = air) to each leg per frame, allowing detailed frame-by-frame stride analysis.

This breakdown reveals how much time athletes spend in the air versus on the ground during each step.


### 4. Metric Computation
Based on the classified stride phases, we compute key sprint performance metrics:

- Step Length: Measured as the distance between ankles during maximum leg extension in air.

- Step Rate (Frequency): Time between left and right foot contacts, converted to steps per second.

- Air Time & Contact Time: Total number of frames spent in each phase, scaled using the video’s FPS.

- Speed: Calculated using Speed = Step Length × Step Rate to estimate running velocity in meters per second.


These metrics help compare stride efficiency, timing, and speed across different athletes.

---

## Athletes Analyzed

1. Noah Lyles  
2. Kishane Thompson  
3. Fred Kerley  
4. Akani Simbine  
5. Marcell Jacobs  
6. Letsile Tebogo  
7. Kenny Bednarek  
8. Oblique Seville

---

## Evaluation Metrics (Threshold Analysis Accuracy)

| Athlete             | Accuracy L | Accuracy R | Precision L | Precision R | Recall L | Recall R |
|---------------------|------------|------------|-------------|-------------|----------|----------|
| Noah Lyles          | 94.0       | 95.2       | 93.70       | 95.97       | 94.44    | 94.44    |
| Kishane Thompson    | 94.8       | 93.6       | 95.20       | 92.13       | 94.44    | 95.12    |
| Fred Kerley         | 94.8       | 94.0       | 93.65       | 94.21       | 95.93    | 93.44    |
| Akani Simbine       | 94.0       | 96.0       | 91.60       | 96.75       | 96.77    | 95.20    |
| Marcell Jacobs      | 92.4       | 94.0       | 92.97       | 91.47       | 92.25    | 96.72    |
| Letsile Tebogo      | 94.0       | 96.8       | 96.67       | 95.08       | 91.34    | 98.31    |
| Kenny Bednarek      | 96.0       | 92.8       | 96.85       | 92.13       | 95.35    | 93.60    |
| Oblique Seville     | 91.2       | 94.0       | 91.41       | 92.25       | 91.41    | 95.97    |

---

## Athlete Performance Summary

| Athlete           | Step Length (m) | Contact Time (s) | Air Time (s) | Step Frequency (Hz) |
|-------------------|------------------|-------------------|---------------|----------------------|
| Noah Lyles        | 2.083           | 0.130             | 0.088         | 4.810                |
| Kishane Thompson  | 2.034           | 0.126             | 0.081         | 4.851                |
| Fred Kerley       | 2.521           | 0.133             | 0.094         | 4.815                |
| Akani Simbine     | 2.161           | 0.133             | 0.079         | 5.936                |
| Marcell Jacobs    | 2.126           | 0.120             | 0.092         | 4.790                |
| Letsile Tebogo    | 2.184           | 0.120             | 0.098         | 4.654                |
| Kenny Bednarek    | 2.273           | 0.130             | 0.080         | 4.825                |
| Oblique Seville   | 1.825           | 0.130             | 0.074         | 5.291                |

---

## Visual Outputs

## Classified Thresholds Visualization
![Classify Thresholds](img/ClassifyThresholds.png)

## Frame-by-Frame Threshold Classification
![Frame-by-Frame Threshold](img/FrameByFrameThreshold.png)

## Air vs Contact Duration - Actual
![Actual Air vs Contact](img/ActualAirVSContact.png)

## Air vs Contact Duration - Predicted
![Predicted Air vs Contact](img/PredictedAirVSContact.png)

## Step Rate - Actual
![Actual Step Rate](img/ActualStepRate.png)

## Step Rate - Predicted
![Predicted Step Rate](img/PredictedStepRate.png)

## Step Length Over Time
![Step Length](img/StepLength.png)

## Step Difference per Frame
![Step Difference](img/StepDiffPerFrame.png)

## Speed Over Time
![Speed](img/Speed.png)

---

## Dependencies

- `numpy`, `pandas`, `scipy`, `matplotlib`, `plotly`, `cv2`
- `scipy.signal.savgol_filter`
- Custom analysis class: `SkeletonAnalyzerFineTune1`

---

