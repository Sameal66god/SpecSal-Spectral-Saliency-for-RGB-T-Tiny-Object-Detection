# SpecSal: Spectral Saliency for RGB-T Tiny Object Detection

**Authors:** Xuanling Feng, Yi Chen, Zhuqi Chu, Bin Zhu

> **Status:** This is a placeholder repository. The paper is under review; code, configs, and trained models will be released here once the paper is accepted.

---

## Updates

- [2026-11] Placeholder repository created. Full release is planned upon paper acceptance.

---

## Introduction

SpecSal is an RGB-T tiny object detector that treats fusion as a spectral saliency problem. Rather than mixing the two modalities indiscriminately in the spatial domain, it separates target-related frequency components from background clutter before fusion. The framework consists of two modules:

- **MFPA** (Multi-scale Frequency-Phase Attention) decomposes paired features into low, mid, and high frequency bands, gates each band by cross-modal agreement, and restores local detail through a lightweight residual branch.
- **CFPE** (Cross-modal Foreground-Protective Enhancement) derives a foreground mask from high-pass energy, suppresses background wavelet coefficients while keeping target coefficients intact, and upsamples features without erasing small objects. The foreground-protection mechanism adds only 28 learnable scalars.

One training recipe is used for all three benchmarks, with no per-dataset tuning.

---

## Main Results

All numbers are produced with the same recipe (20 epochs, effective batch size 8, multi-scale 480-800, EMA, ResNet-50 x 2 backbone, 640 x 640 input) and the official evaluator of each dataset. Full comparisons with prior methods are reported in the paper.

### RGBT-Tiny

[[Dataset](https://github.com/XinyiYing/RGBT-Tiny)] &middot; SAFit evaluation protocol

| Method | AP | AP50 | AP75 | AP_t | AP_et | AP_s | AP_m | AR |
|--------|----|------|------|------|-------|------|------|----|
| **SpecSal (Ours)** | **42.85** | **62.10** | **50.54** | **33.16** | **46.80** | **59.98** | **58.49** | **57.19** |

### RGBTDronePerson

[[Project page](https://nnnnerd.github.io/RGBTDronePerson/)] &middot; tiny-object IoU protocol

| Method | mAP50 | mAP25 | mAP50 (tiny) |
|--------|-------|-------|--------------|
| **SpecSal (Ours)** | **57.52** | **67.24** | **58.85** |

### VTUAV-det

[[Project page](https://nnnnerd.github.io/RGBTDronePerson/)] &middot; COCO protocol, person class

| Method | mAP | mAP50 | mAP75 | mAP_s | mAP_m | mAP_l |
|--------|-----|-------|-------|-------|-------|-------|
| **SpecSal (Ours)** | **36.02** | **78.13** | **28.08** | **20.49** | **34.97** | **59.68** |

### Efficiency

Measured on a single RTX 4090, fp32, batch size 1, 640 x 640 input.

| Params | GMACs | Latency | FPS |
|--------|-------|---------|-----|
| 85.71 M | 184.29 | 65.22 ms | 15.33 |

---

## Getting Started

### Installation

Requirements: Python >= 3.9, PyTorch >= 2.1 (tested with CUDA 12.1).

```bash
git clone https://github.com/PLACEHOLDER/SpecSal.git
cd SpecSal
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
pip install -r requirements.txt
```

### Dataset Preparation

| Dataset | Split | Link |
|---------|-------|------|
| RGBT-Tiny | 34,111 train / 12,590 test pairs | [GitHub](https://github.com/XinyiYing/RGBT-Tiny) |
| RGBTDronePerson | 4,900 train / 1,225 val pairs | [Project page](https://nnnnerd.github.io/RGBTDronePerson/) |
| VTUAV-det | 11,392 train / 5,378 test pairs | [Project page](https://nnnnerd.github.io/RGBTDronePerson/) |

Organize the data under `data/` as follows:

```text
data/
├── rgbt_tiny/
│   ├── train/
│   └── test/
├── droneperson/
│   ├── train/
│   └── val/
└── vtuav_det/
    ├── train/
    └── test/
```

### Training

```bash
python tools/train.py --config configs/rgbt_tiny/specsal_r50x2_20e.yaml
```

The same config template applies to the other two datasets; only the dataset paths change.

### Evaluation

```bash
python tools/test.py --config configs/rgbt_tiny/specsal_r50x2_20e.yaml --checkpoint /path/to/model.pth
```

Each dataset is evaluated with its official protocol (SAFit for RGBT-Tiny, tiny-object IoU for RGBTDronePerson, COCO for VTUAV-det).

### Model Zoo

| Dataset | Config | Checkpoint |
|---------|--------|------------|
| RGBT-Tiny | coming soon | coming soon |
| RGBTDronePerson | coming soon | coming soon |
| VTUAV-det | coming soon | coming soon |

---

## Citation

If you find this work useful, please cite:

```bibtex
@article{feng2026specsal,
  title   = {SpecSal: Spectral Saliency for RGB-T Tiny Object Detection},
  author  = {Feng, Xuanling and Chen, Yi and Chu, Zhuqi and Zhu, Bin},
  journal = {Pattern Recognition (to appear)},
  year    = {2026}
}
```

---

## Acknowledgement

The training pipeline builds on our in-house reproduction of the DyFCLT framework; the reproducibility discussion is given in the paper. We thank the maintainers of RGBT-Tiny, RGBTDronePerson, and VTUAV-det for the public benchmarks.

---

## License

To be determined upon release.

---

## Contact

Xuanling Feng &mdash; fengxuanling22@nudt.edu.cn
