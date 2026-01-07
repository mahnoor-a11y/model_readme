# Sign-IDD.yaml

- **dats**
  max_sent_length: Maximum sentence and frames length allowed
  rawdata: If we are giving raw human videos as input keep this flag true to run media pipe.
  jsons : if we are giving direct json files keep this variable true to skip mediapipe
  
- **plot**
 predicted: Plot predicted poses only
 predicted_vs_groundtruth: Plot side-by-side comparison

- **Training**
  epochs: How many epochs to run for
  validation_freq: After how many steps to run a validation on the model
  learning_rate: Initial model learning rate

- **classification**
  define best, avg , worst case bins ranges to classify frames on the basisi of dtw
