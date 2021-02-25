# Types of image segmentation

## Image Classification

Image classification is the task of categorizing an image according to a certain feature mapping. For example, for an image of a cat the goal is to classify the image as 'cat' and likewise for an image of a dog. No prediction about the location of the object of interest in the image is provided. The goal is to predict a class for the entire image. It suffers from the issue of being unable to classify images with several instances of the same or different classes properly.

## Image Localization and Object Detection

Image localization consists of predicting the location of a **single object** in a given image. Methods try to predict the location of an object of interest in the image and draw a bounding box around it. In the event of **multiple objects** in an image, object detection is used. Object detection predicts the location of multiple objects in an image along with the class of each predicted object. For example, in an image with a cat and a dog or, several cats, object detection algorithms draw bounding boxes around each cat/dog in the image and classifies each of them.


## Semantic Segmentation 

Semantic segmentation is the task of classifying every pixel in the image and assigning it to a particular class. This provides predictions of the shape of the objects in the image. All pixels belonging to a particular class are represented as the same. For example, in an image of a group of people standing together, semantic segmentation may classify all the people together and colour them together using the same colour while having the background as a separate colour. Therefore, it suffers from an inability of differentiating different instances of the same class in a image.

## Instance Segmentation

Like semantic segmentation, instance segmentation aims to classify each pixel in the image. In addition to this, instance segmenation also classifies different instances of the same class in an image. Therefore, in a picture of a crowd of people, instance segmentation would try to categorize each person separately.
