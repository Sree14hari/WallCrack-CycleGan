WALL CRACK CYCLE GAN

1. Introduction & Objective
The accurate visual inspection of structural cracks in concrete is crucial for safety and maintenance. However, real-world images are often degraded by artifacts such as motion blur and inconsistent lighting (shadows), which can interfere with both human and automated analysis.
The primary objective of this project was to develop and evaluate a deep learning model to remove these blur and shadow artifacts from images of cracked walls. We chose a Cycle-Consistent Generative Adversarial Network (CycleGAN), a model capable of learning transformations between two image domains without requiring paired data.
The success of the project was measured by:
1.	The visual quality of the restored images.
2.	Quantitative image quality metrics (PSNR and SSIM).
3.	The model's ability to preserve the physical dimensions of the crack after artifact removal

2. Methodology
2.1. Model Selection: CycleGAN
A CycleGAN was chosen because it excels at unpaired image-to-image translation. This means we did not need an exact "before and after" pair for every image (e.g., the same crack with and without a shadow). Instead, the model learns the general characteristics of an "artifact domain" (A) and a "clean domain" (B) and learns to translate between them. This is ideal for this project, as creating perfectly paired real-world data is impractical.
2.2. Dataset Preparation
A systematic approach was taken to prepare a high-quality dataset for training and testing.
1.	Domain A (Around 1500 images): This "problem" domain was created by applying augmentations to the clean images. To ensure the model was robust, three types of augmentations were generated for each clean image:
o	Gaussian Blur (500)
o	Randomized Synthetic Shadows (500)
o	A combination of both Blur and Shadow (500)
2.	Domain B (Clean Images): This "solution" domain contained the original, artifact-free cracked wall images.
2.3. Training Environment
•	Hardware: NVIDIA GeForce RTX 4050 Laptop GPU (6 GB VRAM)
•	Software: Python, PyTorch
•	Framework: The official pytorch-CycleGAN-and-pix2pix repository.
2.4. Training Process
The model was trained locally with the following key parameters, optimized for the available hardware:
•	Epochs: 50
•	Batch Size: 1
•	Image Size: 256x256 pixels
Training progress was monitored both through the terminal output (loss values) and visually by inspecting the generated image samples in the automatically created index.html report.
Evaluation Results of 50th epoch Model:
	Average PSNR: 25.11 dB
	Average SSIM: 0.8903

3. Model Training
•	Framework and Architecture: The project utilized the official PyTorch implementation of CycleGAN. The generator network used was the default ResnetGenerator with 9 residual blocks, which is known for producing high-quality results.
•	Training Environment: The model was trained locally on a machine equipped with an NVIDIA RTX 4050 Laptop GPU.
•	Training Parameters: The model was trained with a batch size of 1 and an image size of 256x256 pixels, which are standard settings for this architecture, especially when limited by VRAM.
•	Training Duration: The model was trained for 50 epochs. Checkpoints were saved and evaluated every 5 epochs to track performance and select the best-performing model.


  
			            a) Blured			      b) Unblured

  
				a) Shadowed			    b) Unshadowed
4. Evaluation and Results
The model's performance was evaluated using two distinct methods: standard quantitative metrics and the professor-specified crack width analysis.
4.1. Quantitative Performance (PSNR/SSIM)
The Peak Signal-to-Noise Ratio (PSNR) and Structural Similarity Index (SSIM) were calculated at various checkpoints to measure image quality. Higher values are better.

Epoch	Average PSNR (dB)	Average SSIM
20	24.66	0.8731
25	24.34	0.8602
30	24.96	0.8819
35	24.67	0.8854
40	24.66	0.8868
45	25.03	0.8891
50	25.11	0.8903

The model achieved its peak performance at Epoch 50, which was selected as the final model for the project.
4.2. Crack Width Preservation Analysis
A custom pyscript, was used to measure the maximum crack width. This script used grayscale conversion, binary thresholding, skeletonization, and a distance transform to calculate the width in millimeters. This analysis was performed on the large 1000-image test set.
The results were visualized in a scatter plot comparing the original crack width to the restored crack width.
4.2.1 Blur width Comparison Result
	clean_width_mm       3.375491
	blurred_width_mm     5.423630
	restored_width_mm    3.059506
Accuracy : 90.638%
 

4.2.2 Shadow width Comparison
	clean_width_mm        3.375491
	shadowed_width_mm    14.519615
	restored_width_mm     3.171094
            Accuracy: 93.94%
 

5. Conclusion
The project successfully demonstrated that a CycleGAN model can be trained to effectively remove blur and shadow artifacts from images of cracked walls. The final model (epoch 50) achieved strong quantitative scores (PSNR: 25.11 dB, SSIM: 0.8903).
Furthermore, the detailed crack width analysis proved that the restoration process maintains the structural integrity of the image features. The model's ability to restore the correct crack width after it was distorted by augmentations highlights a practical application for this technology in automated inspection and analysis.

