# Noise Robustness of NeuroAssist EEG Event Localisation

Python Capstone (Track A): reproduce and extend
**"NeuroAssist: Open-Source Automatic Event Detection in Scalp EEG"** (IEEE Access, 2024, [doi:10.1109/ACCESS.2024.3492673](https://doi.org/10.1109/ACCESS.2024.3492673)).

- **Reproduce:** Table 3, Random Forest + Mexican Hat wavelet (accuracy 0.732, macro F1 0.667)
- **Extend:** test robustness to Gaussian noise at SNR 20 / 10 / 5 / 0 dB
- **Proposal:** [PROPOSAL.md](PROPOSAL.md)

## Repository layout

| Path | Contents |
|---|---|
| `PROPOSAL.md` | One-page project proposal |
| `docs/` | Benchmark paper PDF and venue-rank evidence |

## Data

The NMT-Events dataset is **not** included in this repository (about 10 GB). Download it from
<https://dll.seecs.nust.edu.pk/downloads/> and place it in a local `data/` folder, which is git-ignored.

## Original code

Authors' reference implementation: <https://github.com/dll-ncai/Localization-of-Abnormalities-in-EEG-Waveforms>

## Python skills profile

[link to be added]
