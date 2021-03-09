# K nearest neighbours algorithm

* Consists of points and their cluster target values.
* Given a new and undefined point, the k nearest neighbours of the cluster in terms of a distance metric are checked and the new points is assigned a class based on majority voting.

* Can have issues when the observed data is skewed heavily in favour of one class. This can be solved by weighting the classification. The class of each of the k nearest points is multiplied by a weight proportional to the inverse of the distance from that point to the new point.
