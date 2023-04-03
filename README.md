# Face-Image-Occlusions
Code implementation for the termpaper Face Image Occlusions in the course IMT4126 Biometrics

The [add_occlusions_on_images.ipynb](../main/add_occlusions_on_images.ipynb) requires a pretrained model from dlib called `shape_predictor_68_face_landmarks.dat` that can be downloaded from [http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2).

## Setup
1. Download `shape_predictor_68_face_landmarks.dat` and put it in the same folder as `add_occlusions_on_images.ipynb`
2. Put face images in the `samples/` folder
3. Open the script `add_occlusions_on_images.ipynb` in Jupyter Notebook / Lab
4. Ensure that the parameters in the first cell are correct in regards to the following:
   - `model_path` is pointing to location of `shape_predictor_68_face_landmarks.dat`
   - `mask`, `cap`, and `glass` variables are updated with the desired occlusions that are going to be applied on the face images that are located in `samples/`
     - The choices of occlusions that are supported are the files that are in `occ/`
5. Run the script and the face images with the generated face occlusions are outputed in `results/`.

### Examples of adding mask and glasses occlusions to face images
````
mask = 'mask.png'
cap = ''
glass = 'glass.png'
````
| Original image | Output image |
| -------------- | ------------ |
|![org_img](example/org_img.png) | ![output_img](example/output_img.png)
