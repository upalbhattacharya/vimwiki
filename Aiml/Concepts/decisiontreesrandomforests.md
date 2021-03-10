# Decision Trees

* Also known as classification and regression trees(CART).
* Decision trees build classification or regression models in the form of a tree structure.
* It is greedy.
* It breask down a dataset into smaller and smaller subsets.
* It creates a tree with:
	* **decision nodes**
	* **leaf nodes**

* A **decision node** has two or more branches.
* A **leaf node** represents a classification or a decision.

* The ID3 algorithm for building decision trees uses **Entropy** and **Information Gain** to construct a decision tree.

* The decision tree is built top-down from the root node and involves partitioning the data into subsets that contain instances with similar values.

* If a sample is completely homogeneous, the entropy is zero while if the sample is equally divided, the entropy is one.

* **Entropy** for a split is given by:


		   ___
	E(S) = \	p_i log p_i
		   /__
		
	where the sum is taken over the number of attributes and $p_i$ is the probability of observing the classification $i$. The log is base 2.
	
* **Information gain** is based on the decrease in entropy after a dataset is split on an attribute. Constructing a decision tree is all about finding attributes that return the highest information gain.

* For a given decision $S$, the information gain is defined as:

						  ___
	IG(S) = E(S/parent) - \		w_i E(split_i/child_i)
						  /__
				
where the summation is taken over the number of children formed from the decision.
 and $w_i$ is the relative weight of the child $child_i$ created from the decision/split with respect to the parent. In simple terms, it is the ratio of the number of data points in the child to the number of data points in the parent.
 
* Thus, given a set of data points, at each node, a set of decision/attributes are considered and the one that gives the greatest information gain is used as the decision and the process continues in this manner till there are only leaf nodes that have entropy 0 indicating no impurity and that the data has been classified accurately.

* In theoretical applications, the model continues till it consists of only leaf nodes. However, in practice, this may lead to overfitting and model with very high variance but low bias. To avoid this, practical implementations allow specifying the depth of the tree as well as the minimum number of data points that must be in a node as a result of a decision.
