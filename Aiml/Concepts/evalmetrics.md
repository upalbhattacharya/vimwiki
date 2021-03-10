# Evaluation metrics

## Confusion Matrix

* It is a matrix representation of the prediction ability of a classification model. In a **binary classification model**, the confusion matrix is a 2x2 matrix given by:
	
					y_pred
			Postive		Negative
	Positive   TP          FN
  y	
	Negative   FP		   TN

where $TP$ is a **true positive** i.e. a correct positive classification.
	  $TN$ is a **true negative**
	  $FP$ is a **false positive**
	  $FN$ is a **false negative**
	 
* However, in a multi-class classification problem, the confusion takes on a different form with different areas of the matrix corresponding to different notions of classification error. The **multi-class confusion matrix is an nxn matrix and is given by**:
	
							y_pred
			1	2	3	4	5	.	.	.	n
		1	TP
		2		TP
		3			TP
		4				TP
		5					TP
		.						TP
		.							TP
		.								TP	
		n									TP
		
	* As with the 2x2 confusion matrix, the diagonal elements correspond to *true positives**. However, it is not easy to define general true negatives, false positives and false negatives and instead **per-class** values of these quantities are calculated:
		* **per-class true negative**: For a class $i$, the true negatives are given by **the sum of all elements in the confusion matrix except the values in the** $ith$ row and $ith$ column. i.e.:
		
					 n		 n
					___		___
			TN_i =  \		\	C_jk
					/__		/__
					j=1		k=1
					j!=i	k!=i
					
		* **per-class false positives**: The false positives for a class $i$ is given by the **sum of the values in the** $ith$ **column of the confusion matrix except the element** $C_ii$. i.e:
			
					 n
					___ 
			FP_i =  \	C_ji
					/__
					j=1
					j!=i
					
		* **per-class false negatives**: The false negatives for a class $i$ is given by the **sum of the values in the** $ith$ **row of the confusion matrix except the element** $C_ii$. i.e:
			
					 n
					___ 
			FN_i =  \	C_ij
					/__
					j=1
					j!=i
## Accuracy

* It is a metric used in classification problems that describes how well the model performs in classification in terms of the percentage of correct classifications. The Accuracy calculation for a **binary classification atsk** is given by:
	
						 TP + TN	
	Accuracy =    ---------------------
					TP + TN + FP + FN
					
* The case of true negatives appears only in per-class accuracy for a multiclassification problem or in a binary classification problem.

* In the context of **multi-class classification** the overall accuracy and per-class accuracies can be calculated.
	* **Overall accuracy**: The overall accuracy is simply the ratio of the number of correct classifications(sum of the diagonal elements) to the total number of observations(sum of all elements in the matrix):
	
	
							 n
							___ 
					TP	=	\	C_jj
							/__
							j=1
		Accuracy = ----------------------
						 n		 n
						___		___
						\		\	C_jk
						/__		/__
						j=1		k=1
	
	* **Per-class Accuracy**: The accuracy of a class is calculated in the same way as that in a binary classification task but by using that classes values of $TP_i$, $TN_i$, $FP_i$ and $FN_i$.

## Precision/Positive Predictive Value

* It is a metric used in classification problems that describes the precision of the positive labelling of the model as a ratio of the number of true positives to the total number of positive classifications.

* In a **binary classification task**, it is given by:
	
							TP	
			Precision = ----------
						 TP + FP
				 
* It gives an idea of the positive classification power of the model with it being 1 as a perfectly precise model with no false positives.

* In the case of a **multi-class classification** problem, there are different variants of the precision metric that can be used:

	* **micro**: The precision is counted globally based on all true positives against the sum of all true positives and false positives. Intuitively, all true positives is the trace of the confusion matrix while all false positives is the sum of the per-class false positives(column sums excluding the diagonal element). Thus, **the denominator is simply the sum of ALL elements in the confusion matrix**. Thus, the resultant value for **micro precision** is the same as the **overall accuracy**.
	
	* **macro**: The per-class precision values are computed using the respective per-class values of true positives and false negatives. The **unweighted mean of the per-class precision scores gives the macro precision**. It does not take label imbalance into account.
	
	* **weighted**:The per-class precision values are computed. A **weighted average of these values is found by weighting with respect to their support(number of true instances of each label)**. 

## Negative Predictive Value

* Follows exactly the same idea as Precision but by considering the ratio of true negatives to the sum of true negatives and false negatives.

## Recall/Sensitivity/True Positive Rate

* The recall is calculated as the ratio between the number of Positive samples correctly classified as Positive to the total number of actual Positive samples. It measures the model's ability to detect positive samples.

* In a **binary classification** setting, it is given by:
	
				   TP	
	Recall = --------------
				TP + FN
				
* The **multi-class classification** setting follows the same nature as that seen with precision, namely with variants:
	* **micro** that is identical to overall accuracy
	* **macro**
	* **weighted**

## Intuition about Precision and Recall

* When a model has **high recall** but **low precision** then the model classifies most of the positive samples correctly but has many false positives.
* when a model has **high precision** but **local recall** then the model the model is accurate when it classifies a sample as positive but it can only classify a few positive samples i.e. it has a lot of false negatives.

* In the case of the goal being to detect all the positive samples without caring whether negative samples would be misclassified as positive, recall is a good metric.

* In the event that the problem is sensitive to classifying a sample as positive, precision would be a good metric.

## F1 score

* It is defined as:
	
			 Precision * Recall
	F1 =  2 --------------------
			 Precision + Recall
			
* It is more useful than accuracy especially for an uneven class distribution. Accuracy works best in the situtaion that false positives and false negatives have the same cost. In the event that the cost of false positives and false negatives is different, Precision and Recall are more useful and subsequently so is F1.

* The F1 score gives an idea about the imbalance between precision and recall. A lower score implies higher imbalance.

* In the event of **multi-class classification**, the F1 score has three types following the three types of precision and recall as:
	* **micro**
	* **macro**
	* **weighted**

## Precision-Recall Curve

* Most classification tasks output a probability distribution vector of the same length as the number of classes to predict with probabilities of belonging to each class. In order to define whether a particular prediction was correct, it is required to set threshold values that determine whether the model was able to assign the correct label with fair confidence. Taking the maximum value in an ouput vector and checking whether it is above or below a threshold gives the output of the model for classification.

* Setting the threshold to a correct value helps in getting a good model  with good precision and recall. To gain an idea of what a good value of the threshold should be, the **precision-recall curve** is used.

* Given predictions, a set of thresholds are taken and for each, the precision and recall are calculated. Then, these values are plotted to give the precision-recall curve.

* The desired threshold is a value for which both precision and recall are high and balanced. This can be observed either from the graph or by computing the F1 scores for each threshold and taking the highest F1 score's corresponding threshold value.

* At low thresholds, the model will have a lot of positive predictions. Thus, while it would have a good number of true postives, it would also contain a lot of false positives. As a result, the precision of the model would be low. At the same time, the large number of true and false positives would reduce the number of false negatives driving up the recall value.

## Average Precision

* The average precision metric is a way to summarize the precision recall curve into a single value representing the average of all precisions.

* It is a weighted sum of precisions at each threshold weighted by the difference between the recall values for two consecutive threshold values.

			n-1
			___
	AP =	\	[Recall(k) - Recall(k+1)]*Precision(k)
			/__
			k=0
	
	Recall(n) = 0, Precision(n) = 1
	n = number of thresholds
	
* It is therefore the area under the precision-recall curve.

## Mean Average Precision

* It is simply the mean of the per-class Average Precision values, averaged in terms of the number of classes considered.

* Usually used in the context of object detection models where IOU thresholds are defined and corresponding recall, precision and average precision values are computed for each threshold and averaged to give the mAP.

## AUC ROC

* ROC stands for **Receiver Operating Characteristics**. 

* The Area under the ROC curve is one of the most important evaluation metrics for checking any classification model's performance.

* The ROC curve is the Recall-False Positive Rate curve where the False positive rate is given by:
	
			  FP
	FPR =  --------- = 1 - Specificity
			FP + TN
			
which gives a measure of how many of the negative labels in a classification problem were incorrectly mapped as false positives.

* The area under the ROC curve gives an idea about the ability of a model to distinguish the two classes.

## Jaccard Index

* It is an evaluation metric for classification models.

* It can be defined as the intersection over the union between the predictions and the targets. In a binary classification problem, it is given as:
						 
						  TP	
	Jaccard Index = -------------
					 TP + FP + FN
					 
* In the multi-class setting, it has the following subtypes:
	* **micro**
	* **macro**
	* **weighted**
