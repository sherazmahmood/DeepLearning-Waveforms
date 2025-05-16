# 🧠 Deep Learning for Seismic Waveform Inversion

This project explores a deep learning approach to **seismic waveform inversion**, transforming temporal waveform recordings into 2D subsurface velocity models. Traditional inversion methods rely on physics-based solvers. Here, we use data-driven modeling powered by **Transformers** and **CNN decoders**, trained on synthetic datasets representing a variety of geological structures.

## 📌 Project Goals

- Build models that learn seismic propagation patterns from waveform data
- Accurately reconstruct 70×70 velocity maps from (5 shots, 1000 receivers, 70 time steps)
- Preserve both **sharp features** (faults, interfaces) and **smooth regions** (layered media)
- Explore **curriculum learning**, starting from simple to complex subsurface models
- Design and test a **composite loss function** to balance fidelity, sharpness, and smoothness

## 📁 Dataset

- Synthetic seismic dataset sourced from the **Yale/UNC Geophysical Waveform Inversion competition**:  
  [https://www.kaggle.com/competitions/waveform-inversion](https://www.kaggle.com/competitions/waveform-inversion)
- Includes waveform recordings for various subsurface conditions:
  - Flat horizontal layers
  - Curved layers and styles
  - Faulted and irregular velocity maps

## Model Architecture

- **Input**: Waveform tensor (5, 1000, 70) → reshaped to (70, 1000)
- **Temporal Embedding**: Linear projection + learned positional encodings
- **Encoder**: Multi-layer Transformer to capture wavefront evolution
- **Decoder**: U-Net–style CNN with skip connections
- **Output**: Velocity map (70, 70)

## Loss Function Design

A custom **composite loss** was developed to improve training outcomes:

- RMSE Loss — baseline reconstruction loss
- Gradient Loss — preserves edges and boundaries
- Total Variation — enforces smooth, homogenous regions
- SSIM Loss — improves perceptual similarity
- Collapse Loss — biases predictions toward expected physical velocity bands

These losses are combined with adjustable weights to balance geological realism and numerical accuracy.

## Learning Strategy: Curriculum Learning

Training was conducted in **three phases**, progressively introducing complexity:

1. **Phase 1**: Simple, flat-layered structures
2. **Phase 2**: Curved velocity models
3. **Phase 3**: Faulted and irregular subsurface scenarios

Each phase accumulated data from previous ones, improving convergence and generalization.

## Exploratory Data Analysis (EDA)

To guide design choices, we analyzed:
- PCA of waveform evolution
- Correlation entropy between time steps
- Total variation and layer estimation in velocity maps

These metrics informed the model architecture and curriculum strategy.


## Citation and Credits

If you use this project, please cite:

> Lin, Y., Lu, L., Reade, W., Howard, A., Cruz, M., Chow, A. (2025).  
> *Yale/UNC Geophysical Waveform Inversion Dataset.*  
> [Kaggle](https://www.kaggle.com/competitions/waveform-inversion)

Model structure and loss function ideas were developed with guidance from OpenAI’s ChatGPT (May 2025 version), and adapted by the project author.
