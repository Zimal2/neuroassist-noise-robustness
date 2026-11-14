# Track A Proposal: Noise Robustness of NeuroAssist EEG Event Localisation

**Student:** Zimal Fatima, [Roll No.] · **Repository:** <https://github.com/Zimal2/neuroassist-noise-robustness> · **Track:** A (Research: reproduce and extend)

## 1. Benchmark paper

M. A. Alqarni, H. Masood, A. J. Qureshi, M. Alvi, H. Arbab, H. A. Khan, A. M. Kamboh, S. Shafait and F. Shafait, "NeuroAssist: Open-Source Automatic Event Detection in Scalp EEG," *IEEE Access*, vol. 12, pp. 170321–170334, Nov. 2024, doi: 10.1109/ACCESS.2024.3492673.

- **Link:** <https://doi.org/10.1109/ACCESS.2024.3492673>
- **Venue rank:** IEEE Access, peer-reviewed journal, **SCImago Q1** (SJR 2025 = 0.884, H-index 338; Computer Science misc. and Engineering misc.). Evidence: [docs/venue_scimago.png](docs/venue_scimago.png)
- **Code (public):** <https://github.com/dll-ncai/Localization-of-Abnormalities-in-EEG-Waveforms>
- **Dataset (public):** NMT-Events: 1,075 scalp EEG recordings (19 channels, 200 Hz, EDF) with neurologist-annotated event times and channels, available from <https://dll.seecs.nust.edu.pk/downloads/>

## 2. Result to reproduce

**Table 3, Random Forest with the Mexican Hat (Mexh) wavelet**, the best classical ML result in the paper. Each 2 s EEG window (50% overlap) of each channel is classified as Normal, Slow Waves or Spike and Sharp Waves, using CWT statistical features reduced with PCA (99% variance) and a subject-wise test split.

| Table 3, Mexh wavelet | Precision | Recall | F1 | **Accuracy** |
|---|---|---|---|---|
| Random Forest (main target) | 0.675 | 0.678 | 0.667 | **0.732** |
| Decision Tree (secondary) | 0.668 | 0.670 | 0.663 | 0.720 |

**Metrics:** test accuracy and macro-averaged precision, recall and F1, as reported in the paper.

## 3. Research question

> How much do the accuracy (0.732) and macro F1 (0.667) of the Random Forest (Mexh) model in Table 3 drop when white Gaussian noise at SNRs of 20, 10, 5 and 0 dB is added to the test EEG signals?

The model is trained on clean data and tested on the same test subjects with and without noise, so noise is the only change.

## 4. Data and compute

- **Data:** NMT-Events (already downloaded, about 10 GB) with the authors' subject-wise train/validation/test split (`data_split.csv`). Following the paper, only patients aged 6 or over are used.
- **Tools:** Python, MNE (EEG loading, filtering, ICA), PyWavelets (CWT), scikit-learn (PCA, Random Forest, Decision Tree), NumPy, pandas, Matplotlib.
- **Runs on Colab: yes.** The classical ML pipeline needs no GPU. It also runs on my laptop (Intel i5, 16 GB RAM) by processing one recording at a time and caching features. Colab is a backup for long runs.
- **Own code:** I will write my own pipeline (EDF → windows → labels → CWT features → PCA → classifier), using the authors' notebooks only as a reference. Their image-generation step for the deep learning models is not needed.

## 5. Plan

- **By 4 December:** data loading and label parsing verified; full pipeline written and tested; Table 3 Random Forest and Decision Tree (Mexh) results reproduced, or the difference explained (for example, the paper reports a 1–45 Hz filter while the code uses 2–45 Hz).
- **By 18 December:** noise extension run at all 4 SNR levels; accuracy, macro F1, per-class F1 and confusion matrices compared with the clean reproduction; IEEE paper (4–6 pages) and README finished with run instructions.
- **Fallback:** if the full reproduction is too slow or fails, train on a smaller subset of training subjects with the full subject-wise test set, use the Decision Tree (Mexh) only, and report it clearly as a partial reproduction. The noise extension applies unchanged.

*LLM use: Claude helped with reading the paper and code, and with drafting. The research question, experiments and analysis are my own.*
