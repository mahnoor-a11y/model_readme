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

# generate_pose_files.py
This file generate training files required by the model:

- `*.skels` — pose sequences  
- `*.gloss` — gloss tokens  
- `*.files` — video ids  
---

## What This Code Does

### 1. Converts MediaPipe keypoints into fixed-length pose vectors
- Extracts 2D **body, hand, and face** keypoints from JSON files  
- Normalizes coordinates relative to the **neck joint**  
- Outputs a **534D pose vector per frame**  
  (`178 joints × (x, y, z)`)  
- Appends the **frame index** as an additional value → **535D per frame**

---

### 2. Converts sentences into gloss format
- Lowercases text and removes punctuation  
- Lemmatizes words using **spaCy**  
- Removes predefined stopwords  
- Outputs **uppercase gloss tokens**  
- Produces **one gloss sequence per video**

---

