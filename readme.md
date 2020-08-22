# Yolov3 Object Detection with Flask and Tensorflow 2.0 (APIs and Detections)
Yolov3 is an algorithm that uses deep convolutional neural networks to perform object detection. This repository implements Yolov3 using TensorFlow 2.0 and creates two easy-to-use APIs that you can integrate into web or mobile applications.


## Getting started

#### Pip
```bash
# TensorFlow CPU
pip install -r requirements.txt

# TensorFlow GPU
pip install -r requirements-gpu.txt
```

### Nvidia Driver (For GPU, if you haven't set it up already)
```bash
# Ubuntu 18.04
sudo apt-add-repository -r ppa:graphics-drivers/ppa
sudo apt install nvidia-driver-430
# Windows/Other
https://www.nvidia.com/Download/index.aspx
```
### Downloading official pretrained weights
For Linux: Let's download official yolov3 weights pretrained on COCO dataset.

```
# yolov3
wget https://pjreddie.com/media/files/yolov3.weights -O weights/yolov3.weights

# yolov3-tiny
wget https://pjreddie.com/media/files/yolov3-tiny.weights -O weights/yolov3-tiny.weights
```
For Windows:
You can download the yolov3 weights by clicking [here](https://pjreddie.com/media/files/yolov3.weights) and yolov3-tiny [here](https://pjreddie.com/media/files/yolov3-tiny.weights) then save them to the weights folder.

### Saving your yolov3 weights as a TensorFlow model.
Load the weights using `load_weights.py` script. This will convert the yolov3 weights into TensorFlow .ckpt model files!

```
# yolov3
python load_weights.py

# yolov3-tiny
python load_weights.py --weights ./weights/yolov3-tiny.weights --output ./weights/yolov3-tiny.tf --tiny
```

After executing one of the above lines, you should see .tf files in your weights folder.

## Running the Flask App and Using the APIs
Now you can run a Flask application to create two object detections APIs in order to get detections through REST endpoints.

You may also want to configure IOU threshold (how close two of the same class have to be in order to count it as one detection), the Confidence threshold (minimum detected confidence of a class in order to count it as a detection), or the maximum number of classes that can be detected in one image and all three can be adjusted within the yolov3-tf2/models.py file.

Initialize and run the Flask app on port 5000 of your local machine by running the following command from the root directory of this repo in a command prompt or shell.
```bash
python app.py
```

### Detections API (http://localhost:5000/detections)
While app.py is running the first available API is a POST routed to /detections on port 5000 of localhost. This endpoint takes in images as input and returns a JSON response with all the detections found within each image (classes found within the images and the associated confidence)

You can test out the APIs using Postman or through Curl commands (both work fine). You may have to download them if you don't already have them.

#### Accessing Detections API with Postman
Access the /detections API through Postman while app.py is running.
Note that the body has to have key "images of type "form-data" set to file. When uploading files hold CTRL button and click to choose multiple photos.

### Image API (http://localhost:5000/image)
While app.py is running the second available API is a POST routed to /image on port 5000 of localhost. This endpoint takes in a single image as input and returns a string encoded image as the response with all the detections now drawn on the image.

#### Accessing Image API with Postman

The uploaded image should be returned with the detections now drawn.

## Running just the TensorFlow model
The tensorflow model can also be run not using the APIs but through using `detect.py` script.

Don't forget to set the IoU (Intersection over Union) and Confidence Thresholds within your yolov3-tf2/models.py file

### Usage examples
Let's run an example or two using sample images found within the data/images folder.
```bash
# yolov3
python detect.py --images "data/images/dog.jpg, data/images/office.jpg"

# yolov3-tiny
python detect.py --weights ./weights/yolov3-tiny.tf --tiny --images "data/images/dog.jpg"

# webcam
python detect_video.py --video 0

# video file
python detect_video.py --video data/video/paris.mp4 --weights ./weights/yolov3-tiny.tf --tiny

# video file with output saved (can save webcam like this too)
python detect_video.py --video path_to_file.mp4 --output ./detections/output.avi
```
Then you can find the detections in the `detections` folder.
<br>


## Command Line Args Reference

```bash
load_weights.py:
  --output: path to output
    (default: './weights/yolov3.tf')
  --[no]tiny: yolov3 or yolov3-tiny
    (default: 'false')
  --weights: path to weights file
    (default: './weights/yolov3.weights')
  --num_classes: number of classes in the model
    (default: '80')
    (an integer)

detect.py:
  --classes: path to classes file
    (default: './data/labels/coco.names')
  --images: path to input images as a string with images separated by ","
    (default: 'data/images/dog.jpg')
  --output: path to output folder
    (default: './detections/')
  --[no]tiny: yolov3 or yolov3-tiny
    (default: 'false')
  --weights: path to weights file
    (default: './weights/yolov3.tf')
  --num_classes: number of classes in the model
    (default: '80')
    (an integer)

detect_video.py:
  --classes: path to classes file
    (default: './data/labels/coco.names')
  --video: path to input video (use 0 for webcam)
    (default: './data/video/paris.mp4')
  --output: path to output video (remember to set right codec for given format. e.g. XVID for .avi)
    (default: None)
  --output_format: codec used in VideoWriter when saving video to file
    (default: 'XVID)
  --[no]tiny: yolov3 or yolov3-tiny
    (default: 'false')
  --weights: path to weights file
    (default: './weights/yolov3.tf')
  --num_classes: number of classes in the model
    (default: '80')
    (an integer)
```
