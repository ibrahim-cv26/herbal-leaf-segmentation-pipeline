# herbal-leaf-segmentation-pipeline
An Automated dataset annotation for herbal plant leaves using Classical Image Processing and HSV Segmentations.
This repository contains the official, production-ready implementation of an unsupervised classical computer vision framework optimized for extracting leaf regions from unconstrained outdoor video assets. This system automates the dataset annotation pipeline, achieving an 85.2% computational matrix reduction data footprint.

## ☁️ Deployment Architecture
To ensure seamless accessibility and reproducible execution without heavy local hardware requirements, the pipeline is engineered to run on cloud infrastructure:
* **Compute Engine:** Developed and optimized for **Google Colab (hosted Linux runtimes)**, leveraging cloud-allocated virtual memory to handle high-throughput frame extraction batches.
* **Storage Ingestion Architecture:** Configured with secure **Google Drive API mounting integrations**, allowing the pipeline to stream large raw HD video assets (ranging between 30 MB to 75 MB) and safely write out isolated target assets directly to persistent cloud directories.

## 📊 Empirical Performance
* **Total Audited Scope:** 21 Independent Video Assets (504 distinct audited frames)
* **High-Fidelity Success Yield:** **66.7%** (14 Videos completely isolated)

## 🛠️ Pipeline Architecture
The pipeline ingests raw frames, transforms them into a decoupled illumination-invariant space, applies structural masking, and isolates foreground targets using the following exact constants:
* **Spatial Dimensions:** $640 \times 480$ via Bilinear Interpolation
* **Lower HSV Bound Vector:** $\mathbf{L} = [35, 40, 40]^T$
* **Upper HSV Bound Vector:** $\mathbf{U} = [85, 255, 255]^T$
* **Morphological Filter Kernel:** $5 \times 5$ Elliptical Structuring Matrix ($A \circ B$ followed by $A \bullet B$)

## 📦 Dataset Directory Structure
To replicate the results, ensure your input directory matches the following environment layout:
├── RawVideos/            # Contains original 1920x1080 .mp4 videos
├── ProcessedVideos/      # Contains raw 640x480 input frames
└── SegmentedOutput/      # Automatically created. Holds isolated color leaf assets

## 🚀 How to Reproduce
1. Open the included notebook script (`01_preprocessing.ipynb`) inside Google Colab or any local Jupyter environment.
2. Configure your `RAW_VIDEOS_DIR` and `PROCESSED_FRAMES_DIR` absolute paths at the bottom of the file.
3. Run all blocks at once from the top left corner. The pipeline will automatically build file safety directories and export pristine target assets matching original dataset filenames.
4. Then run the included notebook script (`02_segmentation.ipynb`) inside Google Colab to interact with our model. Try out the HSV interactive widget where you are not happy with the segmented image!
   <img width="1757" height="672" alt="image" src="https://github.com/user-attachments/assets/b9103b10-35ba-4189-a036-44ac75fe569e" />
6. Scripts (`02b_batchsegmentation.ipynb`) and (`02c_isolated_segments.ipynb`) are exploratory and not mandatory.

## 📦 Repository Structure

The codebase is modularized into sequential execution steps located in the `scripts/` directory:
1. **`01_preprocessing.ipynb`** Ingests raw HD video source files, handles temporal sampling at $1 \text{ fps}$, and executes spatial downsampling to $640 \times 480$ via bilinear interpolation.
2. **`02_segmentation.ipynb`** The interactive evaluation environment used to tune and lock in our primary HSV boundary vectors.
3. **`02b_batchsegmentation.ipynb`** Generates horizontal side-by-side split concatenations (`_verification.png`) for manual empirical quality auditing.
4. **`02c_isolated_segments.ipynb`** Applies the final binary mask back to the original frames via bitwise-AND matrix operations to export isolated, full-color herbal leaf targets against a absolute matte-black background.

