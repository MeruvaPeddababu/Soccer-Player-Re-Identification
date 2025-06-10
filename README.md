# Soccer Player Re-Identification in Sports Footage – Report

## 🎯 Objective

The goal of this assignment was to implement **player re-identification (Re-ID)** techniques in soccer footage. Two options were addressed:

1. **Option 1: Cross-Camera Player Mapping** – Match player identities across two different camera feeds (`broadcast.mp4` and `tacticam.mp4`)
2. **Option 2: Re-Identification in a Single Feed** – Ensure consistent player IDs over time, even when players go out of frame and reappear

Both tasks simulate real-world sports analytics scenarios where maintaining persistent identity is crucial for performance tracking, tactical analysis, and automated commentary.

---

## 🧩 Approach and Methodology

### General Setup

We used a pre-trained YOLOv11 model fine-tuned for soccer player detection and applied built-in tracking algorithms like **BoT-SORT** for ID persistence.

To ensure robustness beyond basic tracking, we implemented custom logic using visual features and similarity matching for both tasks.

---

### Option 1: Cross-Camera Player Mapping

#### Steps:
1. Run object detection + tracking independently on both videos
2. Extract first appearance crops of each player
3. Use **HSV histogram comparison** and **cosine similarity** to match appearances
4. Save mapping between Broadcast ID and Tacticam ID in JSON format

#### Outcome:
- Successfully matched several players across cameras
- Mappings saved in `cross_camera_mapping.json`
- Accuracy limited by lighting differences and pose variation

---

### Option 2: Re-Identification in a Single Feed

#### Steps:
1. Run detection and tracking on `15sec_input_720p.mp4`
2. Build a **Re-ID memory bank** that stores appearance features of first-seen players
3. When a player reappears after being out of view, compare its features with known ones
4. Assign consistent IDs based on best match
5. Output an annotated video showing persistent IDs

#### Outcome:
- Consistent IDs maintained even after temporary disappearance
- Annotated output video saved as `output_reid_video.mp4`
- Simple but effective logic using color histograms

---

## 🔬 Techniques Tried and Outcomes

| Technique | Description | Outcome |
|----------|-------------|---------|
| **YOLOv11 + BoT-SORT Tracker** | Used for initial detection and tracking | Provided good baseline ID assignment and motion tracking |
| **Color Histogram Matching** | HSV histogram of player crops used for visual similarity | Worked well under similar lighting; failed with large viewpoint change |
| **Cosine Similarity** | Measured similarity between feature vectors | Improved matching accuracy over raw histogram comparison |
| **Custom Re-ID Memory Bank** | Stored and compared player appearances | Enabled robust re-identification beyond tracker limits |

---

## ⚠️ Challenges Encountered

1. **Lighting Variation**
   - Players appeared differently in various frames or camera angles
   - Affected histogram-based matching accuracy

2. **Pose and Viewpoint Changes**
   - Players seen from different angles looked visually different
   - Caused mismatches in cross-camera mapping

3. **Occlusions and Fast Movement**
   - Trackers sometimes lost players during heavy occlusion
   - Led to ID switches or new ID assignments on reappearance

4. **Model Limitations**
   - The provided `best.pt` model was basic and not trained on diverse soccer data
   - Missed some players or generated false positives in complex scenes

---

## 🛠️ What Remains / How to Proceed with More Time/Resources

While both options were successfully implemented, there are several improvements that could be made:

### 1. **Use Deep Feature Embeddings**
   - Replace histogram-based matching with deep Re-ID models like **OSNet**, **FastReID**, or **StrongSORT**
   - These models learn robust features invariant to pose, lighting, and scale changes

### 2. **Add Spatial Consistency Checks**
   - For cross-camera mapping, use field alignment or spatial priors to improve matching
   - Map player positions onto a common coordinate system (e.g., top-down pitch map)

### 3. **Improve Long-Term Tracking**
   - Add trajectory prediction to handle long-term occlusion
   - Integrate Kalman Filters or LSTM-based motion models

### 4. **Optimize for Real-Time Performance**
   - Reduce inference latency using TensorRT or ONNX optimizations
   - Enable live feed processing and real-time visualization

### 5. **Enhance Model Accuracy**
   - Fine-tune the YOLO model on additional soccer datasets
   - Improve detection accuracy and reduce false negatives

---

## ✅ Final Summary

This submission demonstrates functional implementations of **both options** of the assignment:

- ✅ **Cross-camera player mapping** with visual matching
- ✅ **Persistent ID assignment** in a single feed with Re-ID logic

Although the current solutions rely on basic visual features, they provide a solid foundation for more advanced approaches.

All code is self-contained, documented, and ready to run without modification.
