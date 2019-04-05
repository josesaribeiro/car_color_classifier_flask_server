# Flask server demo for car color classification

[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE)

## Introduction

A Python example for using [Spectrico's car color classifier](http://spectrico.com/car-color-recognition.html). The Flask server exposes REST API for car color recognition. It takes as input a cropped single car image. The API is simple: make a HTTP POST request to local host on port 6002. The cropped car image must be send using multipart/form-data encoding.
It has to be jpg or png. The size doesn't matter. It is resized by the server to 224x224 pixels. Because the car image is not square, it is center-cropped by the server to become square and then it is resized to 224x224 pixels, so the aspect ratio is keeped. The server returns the first 3 color probabilities. The classifier is based on Mobilenet v2 (TensorFlow backend). It takes 35 milliseconds on Intel Core i5-7600 CPU for single classification. It can be accelerated more by running on GPU. This classifier is not accurate enough yet and serves as a proof-of-concept demo.

---

#### Usage
The server is started using:
```
$ python cars_colors_classifier_server.py
```
The request format using curl is:
```
curl "http://127.0.0.1:6002" -H "Content-Type: multipart/form-data" --form "image=@car.jpg"
```
The response is in JSON format:
```
{
  "colors" : [
    {
      "color" : "orange",
      "prob" : 0.9554577
    },
    {
      "color" : "yellow",
      "prob" : 0.019680426
    },
    {
      "color" : "white",
      "prob" : 0.009691614
    }
  ]
}
```

![image](https://github.com/spectrico/car_color_classifier_flask_server/blob/master/car-color.png?raw=true)

---
## Requirements
  - python
  - numpy
  - tensorflow
  - opencv
  - PIL

---
## Configuration

The settings are stored in python file named config.py:
```
model_file = "model-weights-spectrico-car-colors-mobilenet-224x224-052EAC82.pb"
label_file = "labels.txt"
input_layer = "input_1"
output_layer = "softmax/Softmax"
classifier_input_size = (224, 224)
```
***model_file*** is the path to the car color classifier
***classifier_input_size*** is the input size of the classifier
***label_file*** is the path to the text file, containing a list with the supported colors

---
## Credits
The car color classifier is based on MobileNetV2 mobile architecture: [MobileNetV2: Inverted Residuals and Linear Bottlenecks](https://arxiv.org/abs/1801.04381)
