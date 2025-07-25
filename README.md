
# 🏃‍♂️ 100m Sprint Analysis – Paris 2024 Olympics

This project analyzes elite sprinters from the Paris 2024 Olympic 100m Final using 2D skeleton-based pose data. Using biomechanical features derived from pose estimation, it calculates per-athlete metrics such as **step length**, **step frequency**, **ground contact time**, **air time**, and **speed**.

---

## 🔍 Overview

The project:
- Uses midhip-to-ankle Z-axis distance to detect leg peak extension.
- Classifies air vs. contact phases per leg using threshold-based logic.
- Computes physical performance metrics and visualizes key events.
- Compares metrics across 8 elite athletes.

---

## 🧠 Methodology

### 1. Pose Data Preprocessing
- Input: CSV with 3D coordinates for 33 landmarks.
- Missing values are linearly interpolated.
- Outliers are corrected.

### 2. Smoothing & Peak Detection
```python
diff1 = |midhip_z - right_ankle_z|
diff2 = |midhip_z - left_ankle_z|
```
- Normalized and smoothed using Savitzky–Golay Filter.
- Peaks where smoothed difference exceeds a threshold are labeled as leg contact initiation.

### 3. Classification of Contact vs. Air
- Frames after peak until drop below threshold → **Contact phase**
- Intervals between opposite leg’s contact → **Air phase**
- Both legs 0 → **Air**
- One leg 1 → **Contact**
- Both legs 1 → **Double contact** (start of run)

### 4. Metric Computation
- **Step Length**: Distance between ankles during max air frame.
- **Step Rate**: Time between alternate leg contact events.
- **Air/Contact Times**: Counted in frames then scaled using FPS.
- **Speed**: `step length × step rate`

---

## 👟 Athletes Analyzed

1. Noah Lyles  
2. Kishane Thompson  
3. Fred Kerley  
4. Akani Simbine  
5. Marcell Jacobs  
6. Letsile Tebogo  
7. Kenny Bednarek  
8. Oblique Seville

---

## 📊 Evaluation Metrics (Threshold Classification Accuracy)

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

## 📈 Athlete Performance Summary

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

## 📊 Visual Outputs

- **Leg state plots** (red = contact, blue = air)
- **Air vs Contact time graphs** (Predicted and Actual)
- **Step rate graph** per athlete (Predicted and Actual)
- **Speed and Step Length graphs** per athlete

---

## 📦 Dependencies

- `numpy`, `pandas`, `scipy`, `matplotlib`, `plotly`, `cv2`
- `scipy.signal.savgol_filter`
- Custom analysis class: `SkeletonAnalyzerFineTune1`

---

