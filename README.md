<div align="center">

  <h1>Face Image Occlusions</h1>
  
  <p>
    The code files for the project Face Image Occlusions Generation in the course [IMT4126 Biometrics](https://www.ntnu.edu/studies/courses/IMT4126) at NTNU.
  </p>
</div>
When refering to real images, it means original face images. When refering to synthetic images, it means face images with face occlusions applied to the image.
<br />

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents
- [Face Image Occlusion Generation](#face-image-occlusion-generation)
   * [:ballot_box_with_check: Requirements](#requirements)
   * [:toolbox: Setup](#setup)
   * [:spiral_notepad: Example](#example)
- [Biometric Features Extraction](#biometric-features-extraction)
   * [:ballot_box_with_check: Requirements](#requirements-1)
   * [:toolbox: Setup](#setup-1)
- [Similarity Comparison](#similarity-comparision)
   * [:ballot_box_with_check: Requirements](#requirements-2)
   * [:toolbox: Setup](#setup-2)
- [Plot distribution](#plot-distribution) 
   * [:ballot_box_with_check: Requirements](#requirements-2)
   * [:toolbox: Setup](#setup-2)

The [add_occlusions_on_images.ipynb](../main/add_occlusions_on_images.ipynb) requires a pretrained model from dlib called `shape_predictor_68_face_landmarks.dat` that can be downloaded from [http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2).
# Face Image Occlusion Generation

## Requirements
This project was runned on Ubuntu 22.04, CUDA supported GPU, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| dlib | 19.24.0 |
| tqdm | 4.65.0 |
| opencv-python| 4.7.0.68 |

## Setup
1. Download `shape_predictor_68_face_landmarks.dat` and put it in the same folder as `add_occlusions_on_images.ipynb`.
2. Put face images in the `samples/` folder.
3. Open the script `add_occlusions_on_images.ipynb` in Jupyterlab.
4. Ensure that the parameters in the first cell are correct in regards to the following:
   - `model_path` is pointing to location of `shape_predictor_68_face_landmarks.dat`.
   - `mask`, `cap`, and `glass` variables are updated with the desired occlusions that are going to be applied on the face images that are located in `samples/`.
   - NB! The choices of occlusions that are supported are the files that are in `occ/`.
5. Run the script and the face images with the generated face occlusions are outputed in `results/`.

## Example
This example is adding mask and glasses occlusions to face images, with showing the occlusion variables and real and synthetic image.
````
mask = 'mask.png'
cap = ''
glass = 'glass.png'
````
| Real Image | Synthetic Image |
| -------------- | ------------ |
|![001_1.png](example/001_1.png) | ![001_1_m_g.png](example/001_1_m_g.png)

# Biometric Features Extraction

## Requirements
This project was runned on machine with Ubuntu 22.04, GPU with CUDA, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| tqdm | 4.65.0 |
| opencv-python| 4.7.0.68 |
| numpy | 1.24.2 |
| onnxruntime-gpu *| 1.4.1 |
| insightface | 0.7.3 |

*) If running with only cpu mode, install onnxruntime instead. NB! The feature extraction will take longer time.

## Setup
1. Open the script `create_embedding_insightface.ipynb` or `create_embedding_insightface_synthetic_only.ipynb` in Jupyterlab.
   - `create_embedding_real.ipynb` extracts embeddings only from real images (from `samples/`).
   - `create_embedding_synthetic.ipynb` extracts embeddings only from synthetic images (from `results/`).
2. Run the script!
   - The script prints a list over files where the insightface cannot detect a face.
   - NB! The script skips extracting embedding from a file if the embedding file already exist in `embeddings/real/` or `embeddings/synthetic/`
4. Embeddings are saved in `embeddings/`.
   - Embeddings from real images are saved in `embeddings/real/`.
   - Embeddings from synthetic images are saved in `embeddings/synthetic/`.

# Similarity Comparision

## Requirements
This project was runned on machine with Ubuntu 22.04, GPU with CUDA, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| tqdm | 4.65.0 |
| numpy | 1.24.2 |

All filenames of the images must start with a three digit id followed by a underscore, where the id is unique of each different person. Example:
````
Person A:
001_Image01.jpg
001_Image02.jpg
001_Image03.jpg
001_Image04.jpg
Person B:
002_Image01.jpg
002_Image02.jpg
002_Image03.jpg
002_Image04.jpg
````

## Setup
1. Open the script `similarity_comparison_real.ipynb`, `similarity_comparison_synthetic_seleted`, or `similairty_comparison_real_vs_synthetic_selected.ipynb` in Jupyterlab.
   - `similarity_comparison_real.ipynb` runs similarity comparison (mated and non-mated)  on all embeddings on real images found in `embeddings/real/`:
   - `similarity_comparison_real_vs_synthetic_selected.ipynb` runs similarity comparison (mated and non-mated) on only real vs synthetic embeddings with the selected occlusions. It selects one real embedding for each id, and compares them towards all synthetic embeddings with the selected occlusions.
   -  `similarity_comparison_real_vs_synthetic_selected.ipynb` runs similarity comparison  (mated and non-mated) on only synthetic embeddings with the selected occlusions.
2. If `similarity_comparison_real_vs_synthetic_selected` or `similarity_comparison_synthetic_selected.ipynb`:
   - Update the parameters of the selected occlusion that are going to be included in the similarity comparison:
   - Example of selecting embeddings with cap and mask to conduct comparison:
     ````
     mask = 1
     cap = 1
     glass = 0
     sunglass = 0
     ````
3. Similarity comparisons are stored are saved in `similarity_scores/`.
   - real images are saved in `embeddings/real/`.
   - generated images with occlusions are saved in `embeddings/synthetic/`.

# Plot distribution

## Requirements
This project was runned on machine with Ubuntu 22.04, GPU with CUDA, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| pandas | 2.0.0 |
| numpy | 1.24.2 |
| seaborn | 0.12.2 |
| matplotlib | 3.7.1 |

## Setup
