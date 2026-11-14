# Track A Proposal: Noise Robustness of NeuroAssist EEG Event Localisation

**Student:** Zimal Fatima, Roll No. 536854, MS AI, SEECS (NUST) · **Repository:** <https://github.com/Zimal2/neuroassist-noise-robustness>

## 1. Benchmark paper

M. A. Alqarni *et al.*, "NeuroAssist: Open-Source Automatic Event Detection in Scalp EEG," *IEEE Access*, vol. 12, pp. 170321–170334, 2024, doi: 10.1109/ACCESS.2024.3492673.

- **Link:** <https://doi.org/10.1109/ACCESS.2024.3492673>
- **Venue rank:** IEEE Access, SCImago **Q1** (SJR 2025 = 0.884). [Evidence](docs/venue_scimago.png)
- **Code:** <https://github.com/dll-ncai/Localization-of-Abnormalities-in-EEG-Waveforms>
- **Dataset:** NMT-Events, 1,075 scalp EEG recordings: <https://dll.seecs.nust.edu.pk/downloads/>

## 2. Result to reproduce

**Table 3, Random Forest with the Mexican Hat (Mexh) wavelet**: 3-class classification (Normal, Slow Waves, Spike and Sharp Waves) of 2 s EEG windows using CWT features, on a subject-wise test set.

| Metric (macro avg.) | Precision | Recall | F1 | Accuracy |
|---|---|---|---|---|
| Paper's number | 0.675 | 0.678 | 0.667 | **0.732** |

The authors' `machinelearning_models.ipynb` reproduces these values, and my reimplementation follows it.

## 3. Research question

> How much do the accuracy and macro F1 of the reproduced Random Forest (Mexh) model drop, relative to my clean result, when white Gaussian noise at SNRs of 20, 10, 5 and 0 dB is added to the test EEG?

## 4. Data and compute

- **Data:** NMT-Events (already downloaded, ~10 GB) with the authors' subject-wise split.
- **Tools:** Python, MNE, PyWavelets, scikit-learn.
- **Runs on Colab: yes.** Classical ML needs no GPU. It also runs on my laptop (i5, 16 GB RAM), processing one recording at a time.

## 5. Plan

- **By 4 December:** own pipeline working, and the Table 3 RF (Mexh) result reproduced or the difference explained.
- **By 18 December:** noise results at all 4 SNR levels compared with the clean reproduction, plus the IEEE paper.
- **Fallback:** use a smaller subset of training subjects (same test set) and report it as a partial reproduction.

*LLM use: Anara helped with reading the paper and claude for code, and with drafting. The research question, experiments and analysis are my own.*
