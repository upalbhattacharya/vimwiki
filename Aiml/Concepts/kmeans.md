# K-means clustering

* Clustering algorithm.

* Given a set of observations $(x_1, x_2, ... x_n)$ k-means aims to partition the observations into $k$ sets $S = {S_1, S_2, ... , S_k}$ so as to **minimize the within-cluster sum of squares**.

* The objective is defined as:

			 k
			___		___
	arg min \		\		||x - mu_i||^2
	   S    /__		/__
	       i=1     x in S_i
		   
 where $mu_i$ is the mean of the points $S_i$.
 
 
* The algorithm requires a specification of the number of clusters $k$.

* Initially, a random set of $k$ points are selected as the centroids/means of the clusters.

* It assigns each observation to the cluster with the nearest mean computed using Euclidean distance

	$S_i^(t) = { x_p: ||x_p - mu_i^(t)||^2 <= ||x_p - mu_j^(t)||^2; 1<=j<=k, j!=i}}$
	
	where $t$ is the iteration step.
	
* Having assigned the points to different clusters, recompute the cluster means by:
	
					  1			___
	m_i^(t+1) =  -----------	\	 x_j
				 | S_i^(t) |	/__
						      x_j in S_i^(t)
							  
* The process is repeated till convergence i.e. there is no change in the cluster means and assignments.

* The problem is NP-hard and may not convergence but good heuristics can be used as a stopping criteria.
