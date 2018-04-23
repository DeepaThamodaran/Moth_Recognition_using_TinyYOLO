# Moth_Recognition_using_TinyYOLO

This repository walks-through how to train a TensorFlow TinyYOLO model using image-retraining. It shows how to take our own images, organize by category, and use them to quickly retrain last layer of the convolution neural network and validate those object of interest. 

The training process outputs the retrained graph as tiny-yolo-voc-{no.of classes}c.pb, which can be used to validate the model in mobile devices. This deep learning classifier can be used to train any object of our interest.

# Operational Procedure in Terminal window

Step-1 Create Virtual Environment using Anaconda3:

> conda create -n MothReg_Desktop_TinyYOLO pip python=3.6

> source activate MothReg_Desktop_TinyYOLO

Step-2 Install Dependencies:

> pip3 install tensorflow-gpu

> pip3 install numpy

> pip install opencv-python

> pip install cython

Step-3 Move to code repository:

> cd Moth_Recognition_using_TinyYOLO

> python3 setup.py build_ext --inplace

Step-4 Download weights:

- Weights and config files can be downloaded form https://pjreddie.com/darknet/yolo/. For this exercise, I used Tiny-YOLO version 
- Now that you have downloaded, place your cfg file as cfg/tiny-yolo-voc.cfg and weights as bin/tiny-yolo-voc.weights

Step-5 Prepare for training:

- Download images of interested objects from Google and label them using rectlabel app. Here, I tried 5-moth species - Cabbage_white, Cecropia_silkmoth, Luna_moth, Bronze_copper and Silvery_Blue; downloaded those species images from google and labelled them using rectlabel app
- Save the images under /training/images folder and annotations under /training/annotations folder
- Generate label.txt file with class names and place it under main folder
- Generate new tiny-yolo-voc-{no.of classes}c.cfg file from tiny-yolo-voc.cfg. The naming convention of new cfg file is always tiny-yolo-voc-{no.of classes}c.cfg. 
- In this new tiny-yolo-voc-{no.of classes}c.cfg file, we have to modify filters and classes of the last convolution layer as below.
- filters=50
    - Reason:
    - Filter calculation: 5 * (x + 5) = y, 
    - where x = No.of classes (if in our case, its 5)
    - thus y = 5 * (5+ 5) = 50
- classes=5
    - Reason:
    - classes represents number of training objects (if in our case, its 5)
    
Step-6 Re-train the network:

- All below commands uses TensorFlow GPU version. So, to run in CPU, just remove --gpu 1.0. 
- To understand all supported arguments used by this program, issue --h command.

> ./flow –h

-- To initiate retraining from beginning

> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load bin/tiny-yolo-voc.weights --train --gpu 1.0 --annotation training/annotations --dataset training/images --epoch 2000

The above execution will produce tiny-yolo-voc-xc-yyyy.meta files under ckpt folder

-- To initiate retraining from last saved check point (if "yyyy" is the last saved check piont)

> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load yyyy --train --gpu 1.0 --annotation training/annotations --dataset training/images --epoch 2000


Step-5 Validate the model:

-- via Real-Time camera

> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load yyyy --threshold 0.03 --gpu 1.0 --demo camera

  The above execution will detect moth species using webcam at real-time
 
> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load yyyy --threshold 0.03 --gpu 1.0 --demo camera --saveVideo

  The above execution will detect moth species using webcam at real-time and save that as video.avi file under main folder 

-- via video files

> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load yyyy --threshold 0.03 --gpu 1.0 --demo data/videos/MothVideo.mp4 

  The above execution will detect moth species from MothVideo.mp4 and save the result as video.avi file under main folder 

-- via photos/images

> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load yyyy --threshold 0.03 --gpu 1.0 --imgdir data/images/

  The above execution will detect moth species from images under data/images/ folder and save the results under data/images/out folder
  
Step-6 Generate retrained graph:

The retrained graph have the naming cnvention of tiny-yolo-voc-xc.pb, to generate this file, use --savepb argument as shown below

> ./flow --model cfg/tiny-yolo-voc-xc.cfg --load -yyyy --savepb

   The above execution produce tiny-yolo-voc-xc.pb and tiny-yolo-voc-xc.meta files under built_graph folder

Step-7 Deactivate virtual environment:

> source deactivate

# Expected Output for Real-Time validation:

Real-time validation can be done interactivelly as well as by using --saveVideo argument, which will save the video file in main folder

Input:

> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --demo camera --saveVideo

Output:

The output file will be saved as video.avi under main folder.


# Expected Output for video validation:

Video validation from video files can also be done by using --saveVideo argument, which will save the video file in main folder, replacing video.avi if it already exist

Input:

> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --demo data/videos/MothVideo.mp4 --saveVideo

Output:

The output file is saved as video.avi under main folder (replacing the above video.avi)


# Expected Output for Image validation:

The below screenshot shows the expected output from photos or images.

Input:

> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --imgdir data/images/

Output:

![alt text](https://github.com/DeepaThamodaran/Moth_Recognition_using_TinyYOLO/blob/master/TinyYOLO_Photo_validation.png)

# Mobile App in iOS devices:

Very clear documentation of how to convert a retrained graph using coreml and make it compactible for mobile device are elaborated in these repositories. I just followed their step validated using iOS device. Below is the screenshot of real-time validation using iPhone

- https://github.com/tf-coreml/tf-coreml
- https://github.com/hollance/YOLO-CoreML-MPSNNGraph

![alt text](https://github.com/DeepaThamodaran/Moth_Recognition_using_TinyYOLO/blob/master/TinyYOLO_RT_validation.png)

# References:
