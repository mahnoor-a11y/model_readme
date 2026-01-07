# Sign-IDD.yaml

 **dats**
 * ` max_sent_length: Maximum sentence and frames length allowed`
 * ` rawdata: If we are giving raw human videos as input keep this flag true to run media pipe.`
 * ` jsons : if we are giving direct json files keep this variable true to skip mediapipe`
  
 **plot**
 * ` predicted: Plot predicted poses only
 * ` predicted_vs_groundtruth: Plot side-by-side comparison`

 **Training**
 * ` epochs: How many epochs to run for`
 * ` validation_freq: After how many steps to run a validation on the model`
 * `learning_rate: Initial model learning rate`

 **classification**
 * ` define best, avg , worst case bins ranges to classify frames on the basisi of dtw`

---

# generate_pose_files.py
This file generate training files required by the model:

- `*.skels` — pose sequences  
- `*.gloss` — gloss tokens  
- `*.files` — video ids  

## What This Code Does

### 1. Converts MediaPipe keypoints into fixed-length pose vectors
- Extracts 2D **body, hand, and face** keypoints from JSON files  
- Normalizes coordinates relative to the **neck joint**  
- Outputs a **534D pose vector per frame**  
  (`178 joints × (x, y, z)`)  
- Appends the **frame index** as an additional value → **535D per frame**


### 2. Converts sentences into gloss format
- Lowercases text and removes punctuation  
- Lemmatizes words using **spaCy**  
- Removes predefined stopwords  
- Outputs **uppercase gloss tokens**  
- Produces **one gloss sequence per video**

---

# data.py

This code file handles **dataset loading, preprocessing, and batching**.


## Purpose

- Loads **source sequences** (gloss/text)
- Loads **target pose sequences** (regression vectors)
- Builds vocabularies
- Creates training, validation, and test iterators

## Key Responsibilities

### 1. Dataset Loading (`load_data`)
- Reads `*.src`, `*.trg`, and `*.files` from train/dev/test folders
- Applies **sequence length filtering**
- Supports **frame skipping** for long pose sequences
- Builds source vocabulary
- Returns:
  - `train_data`, `dev_data`, `test_data`
  - `src_vocab`, `trg_vocab`


### 3. Vocabulary Management
- Builds source vocabulary from training data
- Supports:
  - Vocabulary size limits
  - Minimum frequency thresholds
  - Predefined vocabulary files

---

# plot_video.py 

This code file handles **2D skeleton visualization and DTW-based temporal alignment** for predicted and ground-truth pose sequences.


### 1. Full Skeleton Visualization
- Supports **178-joint skeletons** (body + hands + face)
- Draws:
  - Body joints and limbs
  - Left/right hands with different colors
  - Face landmarks (eyes, eyebrows, lips, oval)
- Consistent **global centering and scaling** across all frames


### 2. Video Generation
- Generates:
  - **Predicted-only videos**
  - **Predicted vs Ground Truth side-by-side videos**
- Output format: `1920×1080`

### 3. Frame Export for GAN Training
- Saves **skeleton-only frames** per sequence in pix2pix_GANs/data folder
- Organized by sequence ID
- Ready-to-use for **pix2pix models**



### 4. DTW-Based Temporal Alignment
- Applies **Dynamic Time Warping (DTW)** to align predictions with references
- Computes DTW scores for:
  - Full skeleton
  - Hands only
  - Face only
- Produces **time-aligned prediction sequences**


---

# predictions.py

This code runs **validation inference** for the model.

## What It Does
- Loads validation data in batches
- Runs the trained model in **evaluation mode**
- Generates predicted pose sequences
- Computes **DTW scores** between predicted and ground-truth poses
- Logs sample predictions for debugging



