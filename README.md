<div align="center">

  <h1>Face Image Occlusions</h1>
  
  <p>
    The code files for the project Face Image Occlusions Generation in the course [IMT4126 Biometrics](https://www.ntnu.edu/studies/courses/IMT4126) at NTNU.
  </p>
</div>
When referring to real samples, it referes to the biometric samples with biometric characteristic of face that are used as input to add occlusions script. When referring to synthetic samples, it referes to the biometric samples with biometric characteristic of face that are synthetically occluded output by the add occlusion script.
<br />

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents
- [:ballot_box_with_check: Project Requirements](#project-requirements)
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
- [Distribution plot of similarity scores](#distribution-plot-of-similarity-scores) 
   * [:ballot_box_with_check: Requirements](#requirements-2)
   * [:toolbox: Setup](#setup-2)
- [DET Calculation and Plot](#det-calculation-and-plot) 
   * [:ballot_box_with_check: Requirements](#requirements-3)
   * [:toolbox: Setup](#setup-3)
   
## Project Requirements
This project was runned on Ubuntu 22.04, CUDA supported GPU, and `Python 3.10.6`.

The [add_occlusions_on_images.ipynb](../main/add_occlusions_on_images.ipynb) requires a pretrained model from dlib called `shape_predictor_68_face_landmarks.dat` that can be downloaded from [http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2).


All filenames of the biometric samples **MUST** start with a digit id followed by a d, where the id is unique of each different person. Example:
````
Person A:
00001d001.jpg
00001d002.jpeg
00001d003.png
00001d004.jpg
Person B:
00002d001.jpg
00002d002.png
00002d003.jpg
00002d004.jpeg
````

All biometric samples must be either `*.png`, `*.jpg`, or `*.jpeg` and be RBG or RGBA.

# Face Image Occlusion Generation

## Requirements

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| dlib | 19.24.0 |
| tqdm | 4.65.0 |
| opencv-python| 4.7.0.68 |

## Setup and run
- If generating all combinations of occlusions, use the script `add_occlusions_on_images_all_combinations.ipynb`.
- If generating one type of combination of occlusions, use the script `add_occlusions_on_images.ipynb`
1. Download `shape_predictor_68_face_landmarks.dat` and put it in the project root folder.
2. Put face images in the `samples/` folder.
3. Open one of the two scripts in Jupyterlab.
4. Ensure that the parameters in the first cell are correct in regards to the following:
   - `model_path` is pointing to location of `shape_predictor_68_face_landmarks.dat`.
   - If `add_occlusions_on_images.ipynb`:
     - `mask`, `cap`, and `glass` variables are updated with the desired occlusions that are going to be applied on the samples that are located in `samples/`. Glass can either be `sunglass.png` or `glass.png`, this will change the output filename accordingly.
     -  NB! The choices of occlusions that are supported are the files that are in `occ/`.
   - If `add_occlusions_on_images_all_combinations`:
     - `cap.png`, `glass.png`, `mask.png`, and `sunglass.png` must be precent in `occ/`
5. Run the script and the synthetic samples are saved in `results/` with addition of characters based on what occlusions are added to the image.
   - Example: `00001d001.jpg` with selected mask and glass as occlusions are saved as `00001d001_mask_glass.jpg`
   - The script prints a list over samples that were rejected as a result of the following:
      * Multiple faces were detected in sample
      * Face not detected in sample
      * Extreme pose conditions detected in sample
      * Sample is grayscale
      * If one of the following landsmarks are out of image bounds:
        - left ear
        - right ear
        - chin
        - eyes
 

## Example
This example is adding mask and glasses occlusions to biometric samples, with showing the occlusion variables and real and synthetic sample.
````
mask = 'mask.png'
cap = ''
glass = 'glass.png'
````
| Real Sample | Synthetic Sample |
| -------------- | ------------ |
|![00001d001.png](example/00001d001.png) | ![00001d001_mask_glass.png](example/00001d001_mask_glass.png)

# Biometric Features Extraction

## Requirements

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| tqdm | 4.65.0 |
| opencv-python| 4.7.0.68 |
| numpy | 1.24.2 |
| onnxruntime-gpu [^1] | 1.4.1 |
| insightface | 0.7.3 |

[^1]: If running with only cpu mode, install onnxruntime instead. NB! The feature extraction will take longer time.

## Setup
1. Open the script `feature_extraction.ipynb` in Jupyterlab.
   - Second cell extracts features only from real samples (from `samples/`).
   - Third cell extracts features only from synthetic samples (from `results/`).
2. Run the script!
   - The script prints a list over biometric samples where the insightface cannot detect a face.
   - NB! The script skips extracting features from a file if the biometric feature file already exist in `features/real/` or `features/synthetic/`
4. Features are saved in seperate files for each biometric sample in `features/`.
   - Features from real samples are saved in the folder `features/real/`.
   - Features from synthetic samples are saved in the folder `features/synthetic/`.

# Similarity Comparision

## Requirements

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| tqdm | 4.65.0 |
| numpy | 1.24.2 |

Extracted features from real samples (`features/real/`) and sythetic samples (`features/synthetic/`)


## Setup
1. Open the script `similarity_comparison.ipynb` in JupyterLab:
2. Run the first cell for imports, paths, function definition and creating feature lists
3. The next cells are independent comparisons:
  - **Second cell**: runs similarity comparison (mated and non-mated) on all features of real samples
  - **Third cell**: runs similarity comparison (mated and non-mated) on real vs synthetic features with all combinations of occlusions. It selects one feature from real image for each subject, and compares them towards all  synthetic features with the one set of combinations of occlusions. And loops through this until all combinations are done.
  - **Fourth cell**: runs similarity comparison (mated and non-mated) on all features of synthetic samples
  - **Fifth cell**: runs similarity comparison (mated and non-mated) on real vs all synthetic features
3. Similarity comparisons are saved in `similarity_scores/, with one file for each of the different categories of comparisons.

# Distribution plot of similarity scores

## Requirements

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| pandas | 2.0.0 |
| numpy | 1.24.2 |
| seaborn | 0.12.2 |
| matplotlib | 3.7.1 |

## Setup


# DET Calculation and Plot

## Requirements

| pip package | version used |
| -------------- | ------------ |
| tikzplotlib | 3.6.3 |
| pandas | 2.0.0 |
| numpy | 1.24.2 |
| seaborn | 0.12.2 |
| matplotlib | 3.7.1 |

## Setup

