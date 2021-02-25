# Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks

## Introduction

While Fast R-CNN helped to drastically reduce computational costs by sharing of convolutions across proposals to achieve almost real-time object detection, the primary bottleneck in detection systems is proposals generation.

While region proposal methods typically rely on inexpensive features and economical inference schemes, the region proposal step still consumes as much running time as the detection network.

Computing proposals with a deep convolutional neural network leads to a proposal network that is nearly cost-gree in comparison with the detection network.

Convolutional feature maps used by region-based detectors like Fast R-CNN can also be used for generating region proposals. A region proposal network(RPN) is constructed on top of the convolutional features by adding a few additional convolutional layers that simultaneously regress region bounds and objectness scores at each location on a regular grid. The RPN is a kind of a fully convolutional network.

RPNs are designed to efficiently predict region proposals with a wide range of scales and aspect ratios. RPNS use anchor boxes that serve as references at multiple scales and aspect ratios.

A training scheme that alternates between fine-tuning for the region proposal task and fine-tuning for object detection keeping the proposals fixed converges quickly. It produces a unified network with convolutional features that are shared between the RPN and the object detection network like Fast R-CNN.

## Related Work

Since R-CNN mainly plays as a classifer, it does not predict object bounds. Thus, its accuracy depends on the performance of the region proposal module. In most cases, object proposal methods were adopted as external modules independent of the detectors.

## Faster R-CNN

Faster R-CNN consists of two modules. 
* The first module is a deep fully convolutional network that proposes regions.
* The second module is the Fast R-CNN detector that uses the proposed regions.

The RPN module tells the Fast-RCNN module where to look.

### Region Proposal Networks

A RPN takes an of any size as an input and outputs a set of rectangular object proposals each with an objectness score(objectness measure the membership to a set of object classes vs. the background). This is modeled with a fully convolutional network. Both the detector network and the RPN share a common number of convolutional layers to gain sharing of computation.

To generate region proposals, 
* a small sliding window network is applied to the convolutional feature map output given by the last shared convolution layer.
* The sliding network takes as input an n x n spatial window of the input convolutional feature map
* Each window is mapped to a lower-dimensional feature vector.
* This feature vector is fed into two sibling fully-connected layers, one for the bounding-box regression and one for the box classification.
* The spatial window size should be kept small as, having passed through the layers of the common convolutional network, it has a large effective receptive field.
* The sliding window is implemented as a n x n convolutional layer which allows for the fully connected layers to be shared across all the windows. The fully connected layers are implemented as 1x1 convolutions.

#### Anchors

* For each sliding window location, k region proposals are simultaneously predicted. 
* The bounding-box regression layer has 4k outputs encoding the coordiantes of the k proposal boxes and the classification layer has 2k outputs that estimate the probability of an object or not for each proposal.
* The k proposals are parameterized relative to k reference boxes known as anchors. These reference boxes are centred at the sliding window in consideration and associated with a different scale and aspect ratio. Faster R-CNN uses 3 scales and 3 ratios for a total of k=9 region proposals for each sliding window.

##### Translation-Invariant Anchors

* The approach of the sliding window along with anchors ensures translation invariance for both the anchors and the functions that compute proposals relative to the anchors. 
* It also reduces the model size with a (4+2) x k dimensional convolutional output layer.
* It is expected to have less risk of overfitting on small datasets.

##### Multiple-Scale Anchors as Regression References

The anchor-based proposal network is built on a pyramid of anchors and is cost-efficient. The method classifies and regresses bounding boxes with reference to anchor boxes of multiple scales and aspect ratios. Thus, the various shapes and scales of the anchor boxes help to provide mutliple scales and ratios for creating proposals.

#### Loss Function
* A binary class label corresponding to the existence of an object or not is assigned to each anchor.
* A positive label is applied to:
	* anchors with the highest IoU with a ground-truth box.
	* anchors with an IoU overlap higher than 0.7 with a ground-truth box.
* Any anchor with an IoU lower than 0.3 for all ground-truth classes is given a negative label.
* Anchors that have IoUs in between are not used for the training objective.
* The loss function is the sum of the average classification loss(averaged over mini-batch size) and the average regression loss(averaged over the number of anchors~2400). The regression loss is the smooth L1 loss as used in Fast R-CNN. The regression loss is only considered over positive labels. The difference between the regression loss used in Faster R-CNN and Fast R-CNN is that the 4 tuple consists of the centre coordinates instead of the coordinates of the top-left corner.
* The bounding box regression is different from other methods in that:
	* The features used for regression are of the same spatial size (3 x 3).
	* The k different bounding boxes account for different sizes, aspect ratios and scales.
	* The k regressors do not share weights.

#### Training RPNs

* The sampling strategy follows the same as that observed in Fast-RCNN. Each mini-batch arises from a single image containing positive and negative samples.
* To avoid biasing towards negative samples, all anchor boxes are not used. Instead 256 anchors are sampled so that the ratio of positives to negatives is upto 1:1. For fewer than 128 positives, the rest are filled with negatives.

### Sharing Features for RPN and Fast-RCNN

The method used for training is a 4-step Alternating Training

* First, the RPN is trained. The network is initialized with an ImageNet pre-trainied model and fine-tuned end-to-end for the region proposal task.
* Second,  a separate detection network is trained by Fast R-CNN using the proposals generated in the first step. It is also initialized by the ImageNet pre-trained model.
* At this point, the two networks do not share convolutional layers.
* Third, the detector network is used to initialize RPN training. The shared convolutional layers are kept fixed and only the layers unique to RPN are fine-tuned. At this point the two networks share convolutional layers.
* Finally, the layers unique to Fast-RCNN are fine-tuned keeping the common layers fixed.
* Running for more such iterations provides negligible improvements.
