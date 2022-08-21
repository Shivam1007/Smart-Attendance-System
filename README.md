# Smart-Attendance-System

Manual attendance systems in place are prone to proxies by students. Biometric attendance is out of the picture as well due to the threat posed by COVID.
Thus in this project we aim to design a non-contact way of marking the student's attendance. We also aim to handle False Positives and duplicate entries as well.

The various phases of the project are described below:

### Face Detection
Converting the whole image area into embeddings creates unnecessary features which are not related to person’s identity/face and adds to the complexty. Thus, we only
detect the face regions from images and keep only them for further embeddings calculations.

Dual Shot Face Detector(DSFD) is presently the state-of-the-art face-detector. Here, we use the pre - trained DSFD Model to locate the face regions and 
return the respective bounding boxes.

### Extracting Embeddings
The above detected face images are provided as input to the Feature Extractor to calculate the face embeddings. In present case, we use the LightCNN-29V2 model which is the state-of-the-art Face Recognition 
model and is able to handle noisy images as well. These embeddings are then stored along with their corresponding IDs. Redundancy is also checked during this step.

### Face Verification
We use the laptop’s camera to continuously gather frames and detect images. If an image contains a face, it’s respective bounding box is calculated and the image is cropped according to it. The cropped image is then passed as input 
to the face embeddings calculator which return an embedding, whose length is 256.

We use the cosine similarity metric to calculate the similarity between the detected face image and the one’s present in the database. The metric ranges from -1 to +1. The higher the score, the higher is the similarity.

### Result Calculation
Finally, the similarity scores calculated above are compared and the highest value among them is cho- sen. If this value lies below a certain threshold, the matching is rejected. 

Otherwise, the attendance of the person is recorded in the Attendance.csv file, along with the corresponding date and time.

## Pre-Trained Models
Download the pre-trained models from the links below and place them in the appropriate folders.

| Model    | Link                                                                                                                          |
|----------|-------------------------------------------------------------------------------------------------------------------------------|
| DSFD     | [WIDERFace_DSFD_RES152.pth](https://drive.google.com/file/d/1X8j_EhI17LJ6ryoNzUVQP2GFWLA8ZsGv/view?usp=sharing)               |
| LightCNN | [LightCNN_29Layers_V2_checkpoint.pth.tar](https://drive.google.com/file/d/1sjkb4jXFmP7BiUvsqezeV6IxFUouEVe4/view?usp=sharing) |


