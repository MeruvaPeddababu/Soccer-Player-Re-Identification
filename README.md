# Soccer-Player-Re-Identification
This repository implements two tasks for player re-identification using computer vision techniques with YOLOv11 and custom logic.

## 📁 Folder Structure
├── README.md
├── best.pt # Pre-trained YOLOv11 model for player detection
├── broadcast.mp4 # Video input for Option 1
├── tacticam.mp4 # Video input for Option 1
├── 15sec_input_720p.mp4 # Video input for Option 2
├── option1_cross_camera.py # Code for Option 1: Cross-camera mapping
├── option2_reid_single_feed.py # Code for Option 2: Single feed Re-ID

## 🧰 Requirements

Make sure the following packages are installed before running:

```bash
pip install ultralytics opencv-python numpy matplotlib scikit-learn gdown torch torchvision
How to Run
# For Option 1: Cross-Camera Mapping
python option1_cross_camera.py
This script:

Detects and tracks players in both broadcast.mp4 and tacticam.mp4
Uses color histograms and similarity matching to map IDs across cameras
Saves mapping results to cross_camera_mapping.json

# python option2_reid_single_feed.py
This script:

Detects and tracks players in 15sec_input_720p.mp4
Maintains consistent IDs even when players go out of view
Outputs an annotated video output_reid_video.mp4 with persistent player IDs
📄 Summary of Approach
🎯 Objective
Ensure consistent player identification:

Across different camera views (Option 1 )
Over time in a single feed, including after temporary disappearance (Option 2 )
🔍 Techniques Used
YOLOv11 + BoT-SORT Tracker
Detects and assigns initial IDs based on motion and appearance
Color Histogram Matching
Compares HSV distributions of player crops for visual similarity
Cosine Similarity
Measures similarity between feature vectors for Re-ID
Memory Bank
Stores known appearances to reassign correct IDs

🧩 Code Explanation
📁 option1_cross_camera.py
Performs cross-camera ID mapping by:

Running tracking independently on both videos
Extracting first appearance of each player
Matching them using histogram comparison
Saving a JSON file with Broadcast ID → Tacticam ID mappings
📁 option2_reid_single_feed.py
Implements real-time Re-ID by:

Tracking players using built-in tracker
Storing appearance features in memory
Re-matching lost players when they reappear
Annotating output video with stable IDs
⚠️ Challenges Faced
Lighting variation affected histogram-based matching
Occlusions caused ID switches
Tracker drift over long sequences
Need for more robust deep Re-ID models like OSNet or FastReID
