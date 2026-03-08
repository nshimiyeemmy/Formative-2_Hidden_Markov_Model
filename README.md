# Hidden Markov Model for Human Activity Recognition

## Project Overview

This repository contains the implementation of a Hidden Markov Model (HMM) for recognizing human activities from smartphone sensor data. The project uses accelerometer and gyroscope readings to classify four distinct activities: **standing**, **walking**, **jumping**, and **still** (no movement).

## Team Members

| Name           | GitHub Username | Email                    | Responsibilities                                                             |
| -------------- | --------------- | ------------------------ | ---------------------------------------------------------------------------- |
| Emmy Nshimiye  | nshimiyeemmy    | nshimiyee311@gmail.com   | Jumping & Still activities data collection, HMM pipeline implementation      |
| Hirwa Blessing | hirwaalu        | blessinghirwa7@gmail.com | Standing & Walking activities data collection, Documentation & visualization |

## Repository Structure

```
Formative-2_Hidden_Markov_Model/
├── dataset/                          # Raw sensor data (ZIP files)
│   ├── jump_*.zip                   # Jumping activity recordings
│   ├── still_*.zip                  # Still activity recordings
│   ├── standing_*.zip               # Standing activity recordings
│   └── walking_*.zip                # Walking activity recordings
├── HMM_Human_Activity.ipynb         # Main Jupyter notebook with complete pipeline
├── HMM_Human_Activity_Report.pdf    # Detailed technical report
├── Team_Task_Sheet_*.pdf            # Task allocation and planning document
└── README.md                        # This file
```

## Project Features

### 1. Data Collection

- **50 sensor recordings** across 4 activities (13 jumping, 13 still, 12 standing, 12 walking)
- Each recording: 5-10 seconds duration
- Sensors: 3-axis accelerometer and gyroscope
- Harmonized sampling rate: **50 Hz**
- Data source: iOS Sensor Logger app (iPhone 15 Pro)

### 2. Feature Engineering

**Time-domain features (8):**

- Accelerometer magnitude: mean, std, variance, SMA
- Gyroscope magnitude: mean, std, variance, SMA
- Axis correlations: acc_xy, acc_xz, acc_yz

**Frequency-domain features (4):**

- FFT-derived dominant frequency (accelerometer & gyroscope)
- Spectral energy (accelerometer & gyroscope)

**Total: 15 features per 2-second window (50% overlap)**

### 3. Hidden Markov Model Implementation

- **From-scratch implementation** in NumPy (no pre-built HMM libraries)
- **Gaussian emission probabilities** with diagonal covariance
- **Baum-Welch algorithm** for training with log-likelihood convergence checking
- **Viterbi algorithm** for activity sequence decoding
- Supervised initialization from labeled training data

### 4. Model Evaluation

- Train/test split with **unseen test files** (held-out recordings)
- Metrics: Sensitivity, Specificity, Overall Accuracy, Confusion Matrix
- Visualizations: Transition matrix, Emission parameters, Decoded sequences
- Raw signal plots for each activity

## Installation

### Requirements

```bash
python >= 3.10
numpy
pandas
matplotlib
seaborn
scikit-learn
```

### Setup

```bash
# Clone the repository
git clone https://github.com/nshimiyeemmy/Formative-2_Hidden_Markov_Model.git
cd Formative-2_Hidden_Markov_Model

# Install dependencies
pip install numpy pandas matplotlib seaborn scikit-learn

# Launch Jupyter notebook
jupyter notebook HMM_Human_Activity.ipynb
```

## Usage

### Running the Complete Pipeline

Open `HMM_Human_Activity.ipynb` and run all cells sequentially:

1. **Data Loading**: Automatically discovers and loads all ZIP files from `dataset/`
2. **Preprocessing**: Harmonizes sampling rates to 50 Hz via linear interpolation
3. **Feature Extraction**: Computes time and frequency features over sliding windows
4. **Training**: Trains HMM using Baum-Welch on labeled training sequences
5. **Evaluation**: Tests on unseen files and generates performance metrics
6. **Visualization**: Produces transition matrices, confusion matrices, and decoded sequences

### Key Parameters

```python
TARGET_FS = 50.0      # Harmonized sampling rate (Hz)
WINDOW_SEC = 2.0      # Window size for feature extraction
OVERLAP = 0.5         # Window overlap (50%)
RANDOM_SEED = 42      # Reproducibility seed
```

## Results Summary

The trained HMM successfully distinguishes between activities with:

- **Overall Accuracy**: Reported in notebook evaluation section
- **Best Recognized**: Activities with distinct motion signatures (e.g., jumping)
- **Most Challenging**: Similar low-motion activities (e.g., standing vs. still)

Key findings:

- Transition probabilities reflect realistic activity patterns
- Frequency-domain features critical for distinguishing dynamic vs. static activities
- Harmonized sampling rate ensures consistent feature quality across devices

## Key Implementation Highlights

### Baum-Welch Training

- Log-space arithmetic prevents numerical underflow
- Convergence criterion: `|log-likelihood(t) - log-likelihood(t-1)| < 1e-3`
- Supervised initialization improves convergence speed

### Viterbi Decoding

- Dynamic programming for most likely state sequence
- Log-space implementation for numerical stability

### State-to-Label Mapping

- Greedy one-to-one assignment based on training data co-occurrence
- Ensures interpretable hidden states aligned with activity labels

## Data Collection Details

### Sampling Rates

- **Raw sampling rates**: ~100 Hz (accelerometer), ~100 Hz (gyroscope)
- **Harmonized rate**: 50 Hz (chosen to balance temporal resolution and computational cost)
- **Window size justification**: 2-second windows capture periodic motion patterns while maintaining sequence resolution

### Device Information

| Team Member  | Device Model  | Sampling Rate (Raw) | Activities        |
| ------------ | ------------- | ------------------- | ----------------- |
| nshimiyeemmy | iPhone 15 Pro | ~100 Hz             | Jumping, Still    |
| hirwaalu     | iPhone 15 Pro | ~100 Hz             | Standing, Walking |

## Contribution History

### Commit Distribution

- **nshimiyeemmy**: 5 commits (4 dataset + 1 code)
- **hirwaalu**: 6 commits (4 dataset + 2 code/docs)

### Task Allocation

See `Team_Task_Sheet_*.pdf` for detailed work breakdown and timeline.

## References & Resources

- **Sensor Logger App**: [iOS](https://apps.apple.com/app/sensor-logger/id1531582925) | [Android](https://play.google.com/store/apps/details?id=com.kelvin.sensorapp)
- **HMM Theory**: Rabiner, L. R. (1989). "A tutorial on hidden Markov models and selected applications in speech recognition"
- **Activity Recognition**: Lara, O. D., & Labrador, M. A. (2013). "A survey on human activity recognition using wearable sensors"

## License

This project is an academic assignment for Machine Learning Techniques II course. All rights reserved to the team members and course instructors.

## Contact

For questions or collaboration:

- Emmy Nshimiye: nshimiyee311@gmail.com
- Hirwa Blessing: blessinghirwa7@gmail.com

## Acknowledgments

- Course Instructor: Machine Learning Techniques II
- Sensor data collection: Team members
- Implementation: From-scratch HMM with NumPy (no external HMM libraries used)

---

**Last Updated**: March 8, 2026  
**Assignment**: Formative 2 - Hidden Markov Models