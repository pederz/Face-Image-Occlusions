<div align="center">

  <h1>Face Image Occlusions</h1>
  
  <p>
    The code files for the project Face Image Occlusions Generation in the course [IMT4126 Biometrics](https://www.ntnu.edu/studies/courses/IMT4126) at NTNU
  </p>
</div>

<br />

The list over the images that has been used from the FRGC dataset are in the file [images_list.txt](images_list.txt).
Both online and local hardware was used in the experiments in this paper. The local hardware that was used was an Ubuntu 20.04 machine, with an Intel 12900K (16 cores and 24 threads), 2 x NVIDIA GeForce 1080 Ti (11 GB GDDR5X), 32 GB DDR5 RAM and a PCIe x4.0 M.2 SSD. The online hardware was Google Colab. Google Colab was runned with a paid subscription (Google Colab Pro+), with GPU as hardware accelerator (NVIDIA Tesla P100 with 16 GB HBM2) and normal runtime shape.

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
- [Plot](#) 

The [add_occlusions_on_images.ipynb](../main/add_occlusions_on_images.ipynb) requires a pretrained model from dlib called `shape_predictor_68_face_landmarks.dat` that can be downloaded from [http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2).
# Face Image Occlusion Generation

## Requirements
This project was runned on Ubuntu 22.04, CUDA supported GPU, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab / notebook | >= 3.6.3 / >=6.5.3 |
| dlib | >= 19.24.0 |
| tqdm | >= 4.65.0 |
| opencv-python| >= 4.7.0.68 |

## Setup
1. Download `shape_predictor_68_face_landmarks.dat` and put it in the same folder as `add_occlusions_on_images.ipynb`
2. Put face images in the `samples/` folder
3. Open the script `add_occlusions_on_images.ipynb` in Jupyter Notebook / Lab
4. Ensure that the parameters in the first cell are correct in regards to the following:
   - `model_path` is pointing to location of `shape_predictor_68_face_landmarks.dat`
   - `mask`, `cap`, and `glass` variables are updated with the desired occlusions that are going to be applied on the face images that are located in `samples/`
     - The choices of occlusions that are supported are the files that are in `occ/`
5. Run the script and the face images with the generated face occlusions are outputed in `results/`.

## Example
This example is adding mask and glasses occlusions to face images, with showing the occlusion variables and org image and output image
````
mask = 'mask.png'
cap = ''
glass = 'glass.png'
````
| Original image | Output image |
| -------------- | ------------ |
|![org_img](example/org_img.png) | ![output_img](example/output_img.png)

# Biometric Features Extraction

## Requirements
This project was runned on machine with Ubuntu 22.04, GPU with CUDA, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab / notebook | >= 3.6.3 / >=6.5.3 |
| tqdm | >= 4.65.0 |
| opencv-python| >= 4.7.0.68 |
| numpy | >= 1.24.2 |
| onnxruntime-gpu*| >= 1.4.1 |
| insightface | >= 0.7.3 |

If running with only cpu mode, install onnxruntime instead. NB! The feature extraction will take longer time.

## Setup

# Similarity Comparision

## Requirements
This project was runned on machine with Ubuntu 22.04, GPU with CUDA, and `Python 3.10.6`.

| pip package | version used |
| -------------- | ------------ |
| jupyterlab / notebook | >= 3.6.3 / >=6.5.3 |
| tqdm | >= 4.65.0 |
| numpy | >= 1.24.2 |
| seaborn | >= 0.12.2 |
| matplotlib | >= 3.7.1 |

## Setup
