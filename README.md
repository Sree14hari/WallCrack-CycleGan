# CycleGAN for Crack Image Restoration and Feature Preservation Analysis

This project demonstrates the use of a CycleGAN model to restore degraded images of concrete cracks by removing blur and shadow artifacts. A key component of this work is a detailed quantitative analysis to verify that the restoration process preserves the physical dimensions (width) of the cracks, making the model suitable as a pre-processing step for automated inspection systems.

## Visual Demonstration

The model is effective at removing both blur and shadow artifacts while maintaining the structural integrity of the original crack.


## Methodology

The project follows a comprehensive end-to-end pipeline:

1.  **Data Preparation**: A custom dataset was created by applying blur and shadow augmentations to a set of clean crack images.
2.  **Model Training**: A CycleGAN model was trained on this unpaired dataset to learn the translation from degraded images (Domain A) to clean images (Domain B).
3.  **Performance Evaluation**: The trained model was evaluated using standard image quality metrics (PSNR and SSIM).
4.  **Feature Preservation Analysis**: A detailed analysis, using a custom script based on image skeletonization and distance transforms, was conducted to measure and compare the crack widths before and after restoration.

## Dataset

The model was trained and evaluated on a custom-built dataset derived from a set of 1000 clean, cracked wall images.

  * **Training Set**:
      * `trainB` (Clean): 800 images.
      * `trainA` (Augmented): 2400 images (800 blurred, 800 shadowed, 800 with both).
  * **Testing Set**:
      * `testB` (Clean Ground Truth): 200 images.
      * `testA` (Augmented): 600 images (200 for each augmentation type).

## Setup and Installation

This project is based on the official `pytorch-CycleGAN-and-pix2pix` repository.

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix.git
    cd pytorch-CycleGAN-and-pix2pix
    ```

2.  **Install base dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

3.  **Install additional libraries for analysis:**

    ```bash
    pip install pandas scikit-image opencv-python seaborn scipy matplotlib
    ```

## Usage

### Training

The model was trained for 50 epochs using the following command. The best-performing checkpoint was selected for final evaluation.

```bash
python train.py --dataroot "path/to/your/Data" --name crack_wall_cleanup_model --model cycle_gan --gpu_ids 0 --batch_size 1 --load_size 256
```

### Testing and Evaluation

To generate restored images from a specific checkpoint (e.g., epoch 50) for the test set, the following commands are used:

1.  **Rename the model file**: In the `./checkpoints/crack_wall_cleanup_model/` directory, rename `50_net_G_A.pth` to `50_net_G.pth`.
2.  **Run the test script**:
    ```bash
    python test.py --dataroot "path/to/Data/testA" --name crack_wall_cleanup_model --model test --epoch 50 --num_test 1000
    ```

### Analysis

The final analysis, including statistical calculations and plot generation, is performed using the scripts provided in the `output.ipynb` Jupyter Notebook.

## Results and Analysis

The final model from **Epoch 50** was selected as the best-performing model based on quantitative metrics.

### Quantitative Model Performance

| Metric | Score |
| :--- | :--- |
| **Average PSNR** | 25.11 dB |
| **Average SSIM** | 0.8903 |

These scores indicate a high level of image quality and structural similarity between the restored images and the original clean images.

### Crack Width Preservation Analysis

A detailed analysis was performed to measure the impact of restoration on the physical dimensions of the cracks.

#### Blur Dataset Analysis

**Descriptive Statistics:**
The blur artifact increased the mean measured crack width by over 1mm, while the CycleGAN restored it to near the original value. The model reduced the mean error by \~70% and the error variance by over 98%.

| Metric | Clean Width (mm) | Blurred Width (mm) | Restored Width (mm) | Error (Blurred) | Error (CycleGAN) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Mean** | 2.763 | 3.798 | 2.965 | 1.091 | 0.344 |
| **Variance** | 1.375 | 7.235 | 1.240 | 3.652 | 0.052 |

**Correlation:**
The heatmap shows a very strong positive correlation (**0.95**) between the clean and restored crack widths, while the correlation with the blurred width was much lower (**0.77**).

#### Shadow Dataset Analysis

**Descriptive Statistics:**
The shadow artifact caused a massive distortion, increasing the mean measured width from \~2.7mm to \~14mm. The CycleGAN model successfully corrected this, reducing the mean error by **\~97%**.

| Metric | Clean Width (mm) | Shadowed Width (mm) | Restored Width (mm) | Error (Shadowed) | Error (CycleGAN) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Mean** | 2.733 | 14.023 | 2.901 | 11.291 | 0.377 |
| **Variance** | 0.991 | 6.129 | 0.904 | 6.468 | 0.053 |

**Correlation and Error Distribution:**
The visualizations from the report, such as the scatter plot and box plot, provide clear evidence of the model's success. The scatter plot shows a tight clustering of restored widths around the ideal 1:1 line. The box plot demonstrates a dramatic reduction in both the median error and error variance when comparing the shadowed images to the CycleGAN-restored images.

## Conclusion

This project successfully demonstrates that a CycleGAN model can be effectively trained to restore images of concrete cracks degraded by blur and shadow artifacts. The quantitative analysis proves that the model not only improves the visual quality but also preserves the dimensional integrity of the cracks, making it a viable and valuable pre-processing tool for automated structural health monitoring and inspection systems.

## Acknowledgments

This project utilizes code from the `pytorch-CycleGAN-and-pix2pix` repository by Jun-Yan Zhu and Taesung Park.
