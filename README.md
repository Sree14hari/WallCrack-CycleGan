# Wall Crack CycleGAN: Artifact Removal for Structural Inspection

This project uses a Cycle-Consistent Generative Adversarial Network (CycleGAN) to remove common visual artifacts, such as motion blur and shadows, from images of cracks in concrete walls. The goal is to improve the accuracy of both human and automated structural analysis by restoring images to a "clean" state.

The model is evaluated on its ability to produce visually clear images, its quantitative performance using **PSNR** and **SSIM** metrics, and, most importantly, its capacity to preserve the physical dimensions of the crack after artifact removal.

-----

## 🖼️ Visual Results

Here are examples of the model's performance in removing blur and shadow artifacts.

| Artifact | Before (Input) | After (Restored) |
| :---: | :---: | :---: |
| **Blur** | +Blurred) | +Unblurred) |
| **Shadow** | +Shadowed) | +Unshadowed) |

-----

## ⚙️ Methodology

### Model Selection: CycleGAN

A **CycleGAN** was chosen for this task because it excels at unpaired image-to-image translation. This is ideal for our use case, as it's impractical to obtain perfectly-paired real-world images of the exact same crack with and without artifacts. The model learns the general characteristics of an "artifact domain" (blurry/shadowed images) and a "clean domain" and translates between them.

### Dataset Preparation

A dataset of \~1500 images was systematically prepared.

  * **Domain A (Artifact Images):** This domain was created by applying augmentations to clean images to simulate real-world problems.
      * Gaussian Blur (500 images)
      * Randomized Synthetic Shadows (500 images)
      * Combination of Blur & Shadow (500 images)
  * **Domain B (Clean Images):** This domain contained the original, artifact-free images of cracked walls.

### Training

The model was trained locally using the official [pytorch-CycleGAN-and-pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) repository.

  * **Hardware:** NVIDIA GeForce RTX 4050 Laptop GPU (6 GB VRAM)
  * **Software:** Python, PyTorch
  * **Generator Architecture:** `resnet_9blocks`
  * **Training Parameters:**
      * **Epochs:** 50
      * **Batch Size:** 1
      * **Image Size:** 256x256 pixels

-----

## 📊 Evaluation and Results

The model's performance was evaluated quantitatively and by analyzing its ability to preserve the physical integrity of the cracks.

### Quantitative Metrics (PSNR & SSIM)

The model was evaluated every 5 epochs. The **Peak Signal-to-Noise Ratio (PSNR)** and **Structural Similarity Index (SSIM)** were calculated, with higher values indicating better image quality. The model trained for **50 epochs** yielded the best results.

| Epoch | Average PSNR (dB) | Average SSIM |
| :---: | :---: | :---: |
| 20 | 24.66 | 0.8731 |
| 25 | 24.34 | 0.8602 |
| 30 | 24.96 | 0.8819 |
| 35 | 24.67 | 0.8854 |
| 40 | 24.66 | 0.8868 |
| 45 | 25.03 | 0.8891 |
| **50** | **25.11** | **0.8903** |

### Crack Width Preservation Analysis

A critical evaluation was performed to ensure the restoration process did not alter the physical measurements of the cracks. A custom Python script was used to measure the maximum crack width (in mm) from the clean, artifact, and restored images.

#### Blur Restoration

The model successfully reduced the exaggerated width caused by blur, restoring it close to the original dimension.

```
Clean Width:     3.375 mm
Blurred Width:   5.423 mm  (Distorted)
Restored Width:  3.059 mm
--------------------------
Accuracy: 90.64%
```

#### Shadow Restoration

Shadows caused a significant overestimation of crack width. The model effectively removed the shadow and restored the measurement with high accuracy.

```
Clean Width:     3.375 mm
Shadowed Width:  14.519 mm (Distorted)
Restored Width:  3.171 mm
--------------------------
Accuracy: 93.94%
```

-----

## ✅ Conclusion

This project successfully demonstrates that a CycleGAN can effectively remove blur and shadow artifacts from images of concrete cracks. The final model achieved strong quantitative scores (**PSNR: 25.11 dB**, **SSIM: 0.8903**) and, more importantly, proved its practical value by preserving the structural dimensions of the cracks with over **90% accuracy**. This highlights the technology's potential for enhancing automated structural inspection systems.
