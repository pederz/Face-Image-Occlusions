<div align="center">

  <h1>Face Image Occlusions</h1>
  
  <p>
    The code files for the project Face Image Occlusions Generation in the course [IMT4126 Biometrics](https://www.ntnu.edu/studies/courses/IMT4126) at NTNU.
  </p>
</div>
When referring to real samples, it refers to the biometric samples with biometric characteristic of face that are used as input to add occlusions script. When referring to synthetic samples, it refers to the biometric samples with biometric characteristic of face that are synthetically occluded output by the add occlusion script.
<br />

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents
- [:ballot_box_with_check: Project Requirements](#project-requirements)
   * [:warning: Biometric Samples filename standards](#biometric-samples-filename-standards)
- [Face Image Occlusion Generation](#face-image-occlusion-generation)
   * [:ballot_box_with_check: Requirements](#requirements)
   * [:toolbox: Setup](#setup)
   * [:spiral_notepad: Example](#example)
- [Biometric Features Extraction](#biometric-features-extraction)
   * [:ballot_box_with_check: Requirements](#requirements-1)
   * [:toolbox: Setup](#setup-1)
- [Similarity Comparison](#similarity-comparison)
   * [:ballot_box_with_check: Requirements](#requirements-2)
   * [:toolbox: Setup](#setup-2)
- [Distribution plot of similarity scores](#distribution-plot-of-similarity-scores) 
   * [:ballot_box_with_check: Requirements](#requirements-2)
   * [:toolbox: Setup](#setup-2)
- [DET Calculation and Plot](#det-calculation-and-plot) 
   * [:ballot_box_with_check: Requirements](#requirements-3)
   * [:toolbox: Setup](#setup-3)
   
## Project Requirements
This project was run on Ubuntu 22.04, CUDA supported GPU, and `Python 3.10.6`.

The [add_occlusions_on_images.ipynb](../main/add_occlusions_on_images.ipynb) requires a pretrained model from dlib called `shape_predictor_68_face_landmarks.dat` that can be downloaded from [http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2).

All biometric samples must be either `*.png`, `*.jpg`, or `*.jpeg` and be RBG or RGBA.

### Biometric Samples filename standards
If the biometric features from the biometric samples are going to be used in similarity comparisons, then all filenames of the biometric samples must start with an id followed by a delimiter and then a picture number, where the id is unique for each subject and each picture of the subject has its own number. In this example, the delimiter is 'd'. Example:
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
:warning: If `delimiter = '_'`, this will cause problems for the similarity comparisons, and plotting functions

# Face Image Occlusion Generation

## Requirements

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| dlib | 19.24.0 |
| tqdm | 4.65.0 |
| opencv-python| 4.7.0.68 |

## Setup and run
- If generating all combinations of occlusions, use the script [add_occlusions_on_images_all_combinations.ipynb](../main/add_occlusions_on_images_all_combinations.ipynb). This will result in 11 synthetic images for each input image.
- If generating one type of combination of occlusions, use the script [add_occlusions_on_images.ipynb](../main/add_occlusions_on_images.ipynb)
1. Download `shape_predictor_68_f ace_landmarks.dat` and put it in the project root folder.
2. Put face images in the `samples/` folder.
3. Open one of the two scripts in Jupyterlab.
4. Ensure that the parameters in the first cell are correct in regards to the following:
   - `model_path` is pointing to location of the file `shape_predictor_68_face_landmarks.dat`.
   - If generating one type of combination of occlusions:
     - `mask`, `cap`, and `glass` variables are updated with the desired occlusions that are going to be applied on the samples that are in `samples/`. Glass can either be `sunglass.png` or `glass.png`, this will change the output filename accordingly.
     -  NB! The choices of occlusions that are supported are the files that are in `occ/`.
   - If generating all combinations of occlusions:
     - [mask.png](../main/occ/mask.png), [cap.png](../main/occ/cap.png), [glass.png](../main/occ/glass.png), and [sunglass.png](../main/occ/sunglass.png) must be precent in `occ/`
5. Run the script and the synthetic samples are saved in `results/` with addition of characters based on what occlusions are added to the image.
   - Example: [00001d001.png](example/00001d001.png) with selected mask and glass as occlusions are saved as [00001d001_mask_glass.png](example/00001d001_mask_glass.png)
   - The script prints a list over samples that were rejected because:
      * Multiple faces were detected in sample
      * Face not detected in sample
      * Extreme pose conditions detected in sample
      * Sample is grayscale
      * If one of the following landmarks are out of image bounds:
        - left ear
        - right ear
        - chin
        - eyes
 

## Example
This example is adding mask and glasses occlusions to biometric samples, showing the occlusion variables and real and synthetic sample.
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
| onnxruntime [^1] | 1.4.1 |
| insightface | 0.7.3 |

[^1]: Running cpu-only mode, if using CUDA compatible gpu, use onnxruntime-gpu instead. 1.4.1 requires CUDA 11.6 and cuDNN 8.2.4.

## Setup
1. Open the script [feature_extraction.ipynb](../main/feature_extraction.ipynb) in Jupyterlab.
   - **Second cell** extracts features only from real samples (from `samples/`).
   - **Third cell** extracts features only from synthetic samples (from `results/`).
2. Run the script!
   - The script prints a list over biometric samples where the insightface cannot detect a face.
   - NB! The script skips extracting features from a file if the biometric feature file already exist in `features/real/` or `features/synthetic/`. But this behaviour can be changed if `overwrite = True` in the first cell
4. Features are saved in separate files for each biometric sample in `features/`.
   - Features from real samples are saved in the folder `features/real/`.
   - Features from synthetic samples are saved in the folder `features/synthetic/`.

# Similarity Comparison

## Requirements

| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| tqdm | 4.65.0 |
| numpy | 1.24.2 |

Extracted features from real samples (`features/real/`) and synthetic samples (`features/synthetic/`)

:warning: The extracted features **MUST** start with an id followed by a delimiter and then a picture number, where the id is unique for each subject and each picture of the subject has its own number. Please see example of the biometric samples naming requirements in [Biometric Samples filename standards](#biometric-samples-filename-standards)

## Setup
1. Open the script [similarity_comparison.ipynb](../main/similarity_comparison.ipynb) in JupyterLab:
2. Run the **first cell** for imports, paths, function definition and creating feature lists. Update delimiter to reflect the file name
3. The next cells are independent comparisons:
  - **Second cell**: runs similarity comparison (mated and non-mated) on features of real samples only. It selects one feature as a biometric probe and compares against all other biometric references and this is repeated until all samples have been used as a probe.
  - **Third cell**: runs similarity comparison (mated and non-mated) on real vs synthetic features with all combinations of occlusions. It selects one combination of occlusions. Then one biometric feature from the synthetic occluded dataset with the selected combination of occlusions is selected as a biometric probe. The probe is compared against all biometric references from the original dataset and repeated until all features with the selected combination of occlusions have been used as a probe. When this is finished, another combination of occlusions is selected and a new round of comparisons is done. This is repeated until all combinations of occlusions have been selected, resulting in 11 different processes.
  - **Fourth cell**: runs similarity comparison (mated and non-mated) on real vs synthetic features with all combinations of occlusions. For each data subject, one sample is selected at random. Then all the features of the synthetic occluded samples generated from the selected sample are used as a biometric probe. Each probe is compared against all biometric references in the original dataset
4. Similarity comparisons are saved in `similarity_scores/`, with one file for each of the different categories of comparisons.
NB! This is a time-consuming process (third and fourth cell)! Also, increase the print length of the cells in Jupterlab to see all the print results from the third cell. Since it will loop through all combinations.

# Distribution plot of similarity scores

## Requirements

Latex are used for legends and labels. If not wanting to use latex, set `LaTeX = False` in the first cell


| pip package | version used |
| -------------- | ------------ |
| jupyterlab | 3.6.3 |
| pandas | 2.0.0 |
| numpy | 1.24.2 |
| seaborn | 0.12.2 |
| matplotlib | 3.7.1 |

## Setup
1. Open the script [plot_distributions.ipynb](../main/plot_distributions.ipynb) in JupyterLab:
2. Run the **first cell** for imports, paths, function definition and creating feature lists
3. The next cells are independent plotting of the different scores:
  - **Second cell**: Plots the score distribution of the mated and non-mated comparison on features from real samples.
  - **Third cell**: Plots the score distribution of the mated and non-mated comparison for each of the different real vs synthetic samples with different combinations of occlusions.
  - **Fourth cell** Plots the score distribution of the mated and non-mated comparison on the real vs all synthic samples.
  - **Fifth cell**: Plots one score distribution for mated comparisons and one score distribution for non-mated comparisons for real samples and real versus synthetic single occlusions in `similarity_scores`. It labels the real samples as "baseline", and the rest are labeled for the single occlusion
  - **Sixth cell**: Plots one score distribution for all mated comparisons and one score distribution for all non-mated comparisons in `similarity_scores`. It labels the real samples as "baseline", and the rest are labeled as what type of combination of occlusions that were in the real vs synthetic comparison.
  - **Seventh cell** Plots one score distribution of the mated real and mated real vs all synthetic samples, and one score distribution of the non-mated real and non-mated real vs all synthetic samples
5. Plots are saved in `plots/`, with files for each of the different categories of comparisons
  - `Latex = True`: Three files for each plot (`.pdf`,`.png`,`.pgf`), and plots are NOT shown in JupyterLab
  - `LaTeX = False`: Two files for each plot (`.pdf`,`.png`) and plots are shown in JupyterLab

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
1. Open the script [plot_DET_curves.ipynb](../main/plot_DET_curves.ipynb) in JupyterLab:
2. Run the **first cell** for imports & paths
3. The next cells are independent plotting of the different DET curves:
  - **Second cell**: Plots DET for the comparison of real samples and real vs synthetic with single occlusions. Saved as `baseline_single_occ.pdf`.
  - **Third cell**: Plots DET for the comparison of real samples and real vs synthetic of mask, mask&sunglass and mask&sunglass&cap. Saved as `baseline_selected_occ.pdf`.
  - **Fourth cell**: Plots DET for the comparison of real samples and real vs synthetic of mask, mask&sunglass, mask&sunglass&cap and real vs all synthetic. Saved as `baseline_selected_occluded-dataset.pdf`.
  - **Fifth cell** Plots DET for the comparison of real samples and real vs all synthetic. Saved as `dataset-occluded_dataset.pdf`.
4. Plots are saved in `plots_DET/` folder.
