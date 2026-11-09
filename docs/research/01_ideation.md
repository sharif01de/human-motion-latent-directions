# Is Walking Speed a Linear Direction in a Human-Motion Latent Space?

**Research Concept Note**  
**Version:** 0.1  


---

## 1. Motivation and Research Question

Human walking sequences combine body configuration, timing, forward progression, and subject-specific style. We ask whether a learned sequence-level representation contains a **single linear direction** that corresponds to physical walking speed and can be used to change speed through a latent-space intervention.

**Research Question:**

For a latent representation $z$ of 3D walking motion, can we find a direction $d$ such that decoding $z + \alpha d$ changes measured walking speed monotonically while retaining plausible gait and much of the original subject/style characteristics?

A one-dimensional subspace is defined as:

$$
\mathrm{span}(d) = \{\alpha d \mid \alpha \in \mathbb{R}\}
$$

It does not need to correspond to one existing latent coordinate.

---

## 2. Literature and Provisional Research Gap

MotionCLIP explores semantic structure and editing in motion latents [1]. Semantic Latent Directions and Words in Motion show how linear or orthogonal directions can expose or control motion semantics [2, 3]. Related work learns low-dimensional locomotion representations linked to robot gait and speed [4].

MotionPyramid (2026) separates motion content, gait timing/frequency, and style, while GaitDynamics (2026) models biomechanical changes across walking/running conditions [5, 6].

### 2.1 Provisional Research Gap

We aim to test whether a **post-hoc one-dimensional linear direction** can control **measured physical forward walking speed** in our human-motion dataset, generalize to unseen subjects, and remain selective when explicit root-velocity channels are removed.

This is a focused empirical question, not a claim that latent gait or speed control has never been studied. The novelty must be checked against the full papers and the characteristics of our dataset.

---

## 3. Research Hypotheses

- **H1 — Accessibility:** A linear probe predicts walking speed from the latent vector on held-out subjects.
- **H2 — Controllability:** Moving along the probe-derived direction changes decoded speed monotonically within the latent data distribution.
- **H3 — Specificity:** The direction is stable across subject splits and random seeds, and preserves gait plausibility better than matched random directions.

---

## 4. Methodology and Evaluation

### 4.1 Step 1: Dataset Audit

Identify the following properties of the available motion dataset:

- Modality and skeleton representation.
- Timestamps and sampling frequency (FPS).
- Spatial units and coordinate system.
- Number of subjects and recording sessions.
- Walking conditions and available ground-truth speed labels.

Prefer calibrated pelvis/root path length divided by elapsed time as the walking-speed measurement. For treadmill trials, use treadmill metadata where available.

**Important:** Root-centred, time-normalized, or uncalibrated data may have already removed the information needed to determine absolute walking speed.

### 4.2 Step 2: Data Preparation and Splitting

- Select relevant walking clips.
- Preserve physical time and spatial scale.
- Record cadence and stride length.
- Split the data by subject rather than randomly by frames.
- Avoid normalization procedures that remove gait frequency.

The subject-wise split is important for evaluating whether the learned direction generalizes to previously unseen individuals.

### 4.3 Step 3: Learn a Baseline Latent Representation

Compare two possible approaches:

1. **PCA on gait features:** Establish a simple dimensionality-reduction baseline.
2. **Temporal autoencoder or VAE:** Learn a compact sequence-level representation that produces one latent vector $z$ for each walking clip.

Avoid starting with a large diffusion model. A compact baseline will make the initial experiments easier to interpret and evaluate.

### 4.4 Step 4: Find and Test a Latent Direction

Fit a ridge regression model:

$$
\hat{v} = w^T z + b
$$

Where:

- $z$ is the latent representation.
- $w$ is the learned regression weight vector.
- $b$ is the bias term.
- $\hat{v}$ is the predicted walking speed.

Standardize or whiten latent dimensions, derive a normalized direction $d$, and decode the edited latent representations:

$$
z' = z + \alpha d
$$

Vary $\alpha$ across values that remain within the latent data distribution.

Compare the results against:

- Random latent directions.
- A root-velocity-only baseline.
- An experiment in which root-velocity channels are removed (ablated).

### 4.5 Step 5: Evaluate the Intervention Effects

Evaluate the following aspects:

| Evaluation Aspect | Measurement |
|---|---|
| Speed prediction | MAE and $R^2$ on held-out subjects |
| Speed controllability | Monotonicity of speed versus $\alpha$ |
| Direction stability | Consistency across subject splits and random seeds |
| Reconstruction quality | Similarity between original and reconstructed motion |
| Foot-ground interaction | Foot sliding and contact consistency |
| Biomechanical plausibility | Joint limits and temporal smoothness |
| Gait characteristics | Changes in cadence and stride length |

Cadence and stride length should be tracked as outcomes because they are coupled to walking speed. They should not be assumed to remain constant during latent-space interventions.

---

## 5. Selected References

**[1]** Tevet et al., *MotionCLIP*, ECCV 2022.  
https://arxiv.org/abs/2203.08063

**[2]** Xu et al., *Learning Semantic Latent Directions for Accurate and Controllable Human Motion Prediction*, ECCV 2024.  
https://doi.org/10.1007/978-3-031-72664-4_4

**[3]** Tas & Wagner, *Words in Motion*, ICLR 2025.  
https://proceedings.iclr.cc/paper_files/paper/2025/hash/d8f1702373bc5f9a085b9f8226c589b2-Abstract-Conference.html

**[4]** Castillo et al., *Data-driven Latent Space Representation for Robust Bipedal Locomotion Learning*, ICRA 2024.  
https://cyberboticslab.com/publication/castillo-2024-data/

**[5]** Li et al., *MotionPyramid*, Computer Graphics Forum / SCA 2026.  
https://doi.org/10.1111/cgf.70563

**[6]** Tan et al., *GaitDynamics*, Nature Biomedical Engineering, 2026.  
https://doi.org/10.1038/s41551-025-01565-8

---

**End of Research Concept Note**