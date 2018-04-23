# Moth_Recognition_using_TinyYOLO
This repository contains Tensorflow- TinyYOLO model code to identify moth species at Real-time, photos as well as videos. 

It walk-through how to train a TensorFlow TinyYOLO model using image-retraining. It shows how to take our own images organize into folders by category, and use them to quickly retrain the top layer of the YOLO neural network to recognize those image categories. 

The training process outputs the retrained graph as tiny-yolo-voc-5c.pb, which can be used to validate the model. This deep learning classifier can be used to train any object of our interest.

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

> cd PatternRecog_code/using_TensorFlow/TinyYOLO_version

> python3 setup.py build_ext --inplace

Step-4 Train the neural network:

- Note that the cfg file under cfg/tiny-yolo-voc-5c.cfg folder and weights under bin/ tiny-yolo-voc.weights are already trained for 3 days up to 2000 epochs - ckpt/tiny-yolo-voc-5c-3000.meta. 
- Hence, we may just need to validate them using some demo images/videos. Also note, below commands uses TensorFlow GPU version. So, to run in CPU, just remove --gpu 1.0. 
- To understand all supported arguments used by this program, issue --h command.

> ./flow –h

Step-5 Validate the network:

- via Real-Time camera
> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --demo camera

- via video files
> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --demo data/videos/MothVideo.mp4

- via photos/images
> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --imgdir data/images/

# Expected Output for Image validation:
The below screenshot shows the expected output of model validation from photos or images.

Input:
> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --imgdir data/images/

Output:



# Expected Output for video validation:
The network validation from video files can be done by using --saveVideo argument, which will save the video file in main folder

Input:
> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --demo data/videos/MothVideo.mp4 --saveVideo

Output:

The output file is saved as video.avi under main folder

# Expected Output for Real-Time validation:
The network validation from desktop/laptop camera at real time can be done by using same --saveVideo argument, which will save the video file in main folder

Input:
> ./flow --model cfg/tiny-yolo-voc-5c.cfg --load 3000 --threshold 0.03 --gpu 1.0 --demo camera --saveVideo

Output:

The output file will be saved as video.avi under main folder (replacing the above video.avi)

Step-6 Deactivate virtual environment:

> source deactivate

# Mobile App in iOS devices:

Step-1 Basic Requirements

- You’ll need an iOS device like iPhone or iPad.
- You’ll need to download and launch latest Xcode.
- To execute the code in iOS device, you’ll need to enroll in the Apple iOS developer program which is $99/year and register your iPhone/iPad under ‘Accounts’ tab.

Step-2 Virtual Environment Setup:

> virtualenv -p /usr/bin/python2.7 env

> source env/bin/activate

> pip install tensorflow

> pip install keras==1.2.2

> pip install coremltools

Step-3 Move to code repository:

> cd PatternRecog_code/using_TensorFlow/TinyYOLO_version/iOS/YOLO-CoreML/YOLO-CoreML

Step-4 Open YOLO-CoreML.xcodeproj in Xcode

Step-5 Build the Project:

- Run and compile the code using iPhone/iPad device. 
- You will receive the message of ‘Build Successful’ and app will get launched in iPhone/iPad accessing your device camera.

Step-6 App Validation:

- Point the iPhone/iPad camera to any trained moth picture/image, you will see the moth labels and its confidence level.

Step-7 Cleanup:

- Close the app and disconnect the device from Xcode.
