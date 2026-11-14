# Track A Proposal: Noise Robustness of NeuroAssist EEG Event Localisation

**Student:** Zimal Fatima, Roll No. 536854, MS AI, SEECS (NUST) · **Repository:** <https://github.com/Zimal2/neuroassist-noise-robustness> · **Track:** A (Research: reproduce and extend)

## 1. Benchmark paper

M. A. Alqarni, H. Masood, A. J. Qureshi, M. Alvi, H. Arbab, H. A. Khan, A. M. Kamboh, S. Shafait and F. Shafait, "NeuroAssist: Open-Source Automatic Event Detection in Scalp EEG," *IEEE Access*, vol. 12, pp. 170321–170334, Nov. 2024, doi: 10.1109/ACCESS.2024.3492673.

- **Link:** <https://doi.org/10.1109/ACCESS.2024.3492673>
- **Venue rank:** IEEE Access, peer-reviewed journal, **SCImago Q1** (SJR 2025 = 0.884, H-index 338; Computer Science misc. and Engineering misc.). Evidence: [docs/venue_scimago.png](docs/venue_scimago.png)
- **Code (public):** <https://github.com/dll-ncai/Localization-of-Abnormalities-in-EEG-Waveforms>
- **Dataset (public):** NMT-Events: 1,075 scalp EEG recordings (962 normal, 113 abnormal; 200 Hz, EDF; 10/20 montage with 21 electrodes, of which 19 EEG channels are used after removing the A1/A2 references) with neurologist-annotated event times and channels, available from <https://dll.seecs.nust.edu.pk/downloads/>

## 2. Result to reproduce

**Table 3, Random Forest with the Mexican Hat (Mexh) wavelet**, the best classical ML result in the paper. Each 2 s EEG window (50% overlap) of each channel is classified as Normal, Slow Waves or Spike and Sharp Waves, using CWT statistical features reduced with PCA (99% variance) and a subject-wise test split.

| Table 3, Mexh wavelet | Precision | Recall | F1 | **Accuracy** |
|---|---|---|---|---|
| Random Forest (main target) | 0.675 | 0.678 | 0.667 | **0.732** |
| Decision Tree (secondary) | 0.668 | 0.670 | 0.663 | 0.720 |

**Metrics:** test accuracy and macro-averaged precision, recall and F1, as reported in the paper. **Protocol check:** the saved output of the authors' `machinelearning_models.ipynb` gives 0.67592 / 0.67880 / 0.66778 / 0.73244, which matches Table 3 (the paper truncates to 3 decimals). That notebook's protocol is my **primary run**: 2–45 Hz bandpass, ICA muscle-artifact removal, CWT scales 1–23 with 5 statistics per scale, StandardScaler, PCA at 99% variance, and RF with 100 trees and depth 7. The paper's stated 1–45 Hz filter is run as a sensitivity check. The paper's values are reproduction **targets**: I will report my own clean numbers and explain any difference.

## 3. Research question

> How much do the accuracy and macro F1 of my reproduced Random Forest (Mexh) model from Table 3 (paper: 0.732 and 0.667) drop, relative to my own clean test-set result, when white Gaussian noise at SNRs of 20, 10, 5 and 0 dB is added to the test EEG signals?

**Noise procedure:** zero-mean white Gaussian noise is added to the raw signal of each channel of each test recording, with variance set so that SNR = 10·log10(P_signal / P_noise) matches the target, where P_signal is that channel's mean power. Noise is added before the same filtering and feature-extraction steps used for clean data. Each SNR level is repeated with 3 fixed random seeds (0, 1, 2), so every noise realisation can be regenerated exactly, and results are reported as mean ± std. The model is trained on clean data only and evaluated on the same held-out test subjects, with the clean baseline reported alongside, so noise is the only change.

## 4. Data and compute

- **Data:** NMT-Events (already downloaded, about 10 GB) with the authors' subject-wise train/validation/test split (`data_split.csv`). Following the paper, only patients aged 6 or over are used.
- **Tools:** Python, MNE (EEG loading, filtering, ICA), PyWavelets (CWT), scikit-learn (PCA, Random Forest, Decision Tree), NumPy, pandas, Matplotlib.
- **Runs on Colab: expected yes.** The classical ML pipeline needs no GPU, and the authors' feature matrix (~450k × 115) fits in under 1 GB of RAM. I will first run an end-to-end pilot on about 10 recordings to measure runtime and memory, then run the full set on my laptop (Intel i5, 16 GB RAM) one recording at a time with cached features, with Colab as a backup.
- **Own code:** I will write my own pipeline (EDF → windows → labels → CWT features → PCA → classifier), using the authors' notebooks only as a reference. Their image-generation step for the deep learning models is not needed.

## 5. Plan

- **By 4 December:** data loading and label parsing verified; 10-recording pilot run (runtime and memory measured); full pipeline written and tested; Table 3 Random Forest and Decision Tree (Mexh) results reproduced (primary 2–45 Hz run plus the 1–45 Hz check), or the difference explained.
- **By 18 December:** noise extension run at all 4 SNR levels; accuracy, macro F1, per-class F1 and confusion matrices compared with the clean reproduction; IEEE paper (4–6 pages) and README finished with run instructions.
- **Fallback:** if the full reproduction is too slow or fails, train on a smaller subset of training subjects with the full subject-wise test set, use the Decision Tree (Mexh) only, and report it clearly as a partial reproduction. The noise extension applies unchanged.

*LLM use: Claude helped with reading the paper and code, and with drafting. The research question, experiments and analysis are my own.*
