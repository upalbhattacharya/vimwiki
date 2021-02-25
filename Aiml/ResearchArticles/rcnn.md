# Rich Feature Hierarchies for Accurate Object Detection and Semantic Segmentation

## Abstract and Introduction

**Observations made** by the authors through the course of the research work:

* One can apply high-capacity convolutional neural networks to bottom-up region proposals in order to localize and segment objects.
* When labelled training data is scarce, supervised pre-training for an auxiliary task, followed by a domain-specific fine-tuning, yields a significant performance boost.

**Achieves results** by:

* localizing objects with a deep network and,
* training a high-capacity model with only a small quantitiy of annotated detection data.

Sliding window based object localization does not provide precise localization. In deeper neural networks, neurons in the deeper layers, due to convolution, have very large receptive fields that make precise localization very difficult.

The goal of localization of objects in images was not done by the sliding window technique. Most work prior to it had used sliding windows with relatively shallow networks having only two convolution and pooling layers. In contrast, the CNN used by the RCNN consists of five convolution and pooling layers. Due to this distinction, units in the higher/later layers have large receptive fields and strides making precise localization difficult.

To overcome the issue of scarce labelled data, the work utilizes a supervised pre-training of the dataset on a large auxiliart dataset followed by domain-specific fine-tuning on a small dataset. It shows that this procedure acts as an effective paradigm for learning high-capacity CNNs when data is scarce. 

## Object detection with R-CNN

The object detection system consists of three modules:

- The first generates **category-independant** region proposals. These are the candidates that are provided to the detector portion of the system.
- The second is a large CNN that extracts a fixed-length feature vector from each of the regions passed from the previous module.
- The third is a set of class-specific linear SVMs.

### Module design

#### Region proposals

Utilizes selective search to generate region proposals.

### Test-time detection

At test time, the method runs selective search on the test image to extract around 2000 region propsals. Each proposal is warped to the correct shaped by a simple affine transformation and propagated through the CNN. 

Each warped region passes through the network to provide a 4096-dimensional feature vector. The dimension of the feature vector is **not** the number of classes. The feature vector gets fed to each of the linear class-trained SVMs to give a class score. For N classes to be classified, there would be N SVMs, one corresponding to each such class. A non-maximum suppression is applied to the vector of all the class scores for the region. This gives the classification of the region. A region is rejected if if has an intersection-over-union overlap greater than a learned threshold with another region proposal having a higher class score for the same class(after non-max supression).

#### Run-time analysis

Detection is made efficient by two properties of the model architecture.

- CNN parameters are shared across all categories. Sharing of parameters amortizes the time spent computing region proposals and features across all the classes. The only class-specific computations are the dot products of the feature vectors with the SVM weights for each class and non-maximum suppression which in practice can be carried out in a single matrix product. An example of such a multiplication would be:
	- The feature matrix consisting of the feature vectors of **all** the regions **after** passing through the CNN would be of shape $2000 \times 4096$
	- The SVM weight matrix would be of shape $4096 \times N$ where $N$ is the number of classes to be identified.
- The features computed by the CNN are low-dimensional when compared to other common approaches.

These properties allows RCNN to scale to thousands of object classes without resorting to approximate techniques.

### Training

The CNN was pre-trained on a large auxiliary dataset (ILSVRC 2012) with image-level annotations. 
To implement domain-specific fine-tuning, the CNN was continued to be trained by stochastic gradient descent  using only warped region proposals from VOC. Region proposals with an IoU greater than or equal to 0.5 overlap with a ground-truth box are treated as positives for that box's class.

To deal with a region that partial overlaps an object of interest, an IoU overlap threshold of 0.3 is used. This was done based on observation.

After feature extraction from the CNN, the per-class linear SVMs are optimized.

As training data is too large to fit in memory, standard hard negative mining is used.

## Visualization, ablation, and modes of error

### Visualizing learned features

To visualize learned features for filters other than the first layer, the research article uses the idea to treat each unit as if it were an object detector in its own right. To achieve this, they compute the units activations on a large set of proposals that were not used for training, sort the proposals from highest to lowest activation and perform non-maximum suppression to display the top-scoring regions. This visually shows what features are learnt by a unit.

### Ablation studies

On carrying out ablation studies for performance without **fine tuning**, the final fully connected layer $fc7$ generalizes **worse** than the full connected layer $fc6$ so removing about 16.8 million of the CNN parameters does not degrade the mAP. In addition, removing both $fc6$ and $fc7$ still produces fairly good results.

On fine-tuning the parameters using VOC 2007 after the initial training,  the mAP increases by 8.0 percentage points to 54.2%. There is a significant boost in the mAP from the layers $fc6$ and $fc7$ fue to fine tuning. 

The above improvement due to fine-tuning with VOC, highlights that the features learnt from ImageNet are general and most of the improvement is gained from learning the domain-specific non-linear classifiers on top of them. 

### Detection error analysis

On analysis of the error modes and types of errors a lot of the error rose due to issues of poor localization (bounding boxes or proposal regions with insufficient overlap with the object of interest) and not due to confusion with the background or other object classes. Therefore, application of bounding box regression generated significantly better performance and a higher mAP.

### Bounding box regression

In order to reduce localization errors a bounding box regression is used. A linear regression model is trained to predict a new detection window given the features of the $pool5$ layer for a selective search region proposal.

###### Tags

:computervision:cnn:objectdetection:semanticsegmentation:rcnn:
