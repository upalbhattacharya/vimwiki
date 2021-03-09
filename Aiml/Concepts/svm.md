# Support Vector Machines

* Also called maximum margin classifier.

* Used for classification problems.

* Creates a decision boundary.

* It finds the decision boundary based on maximizing the width between the points of one class and the points of another class.(positive and negative classes)

* It tries to find the margin which maximizes the width between the instances of two classes.

* Consider a vector that is perpendicular to this gutter from the origin.

* For an unknown point that requires classification, project that vector onto the perpendicular vector and decide where the point/vector lies.

* If $w$ is the perpendicular and $u$ is the unknown vector then the criterion can be written as:

	$w.c >= c$ where c is a constant
	
* $>=$ means that the point is a positive sample. Equivalent formulation:
	
	$w.c + b >=0 -> positive sample$
	
	the above is the **decision rule**.
	
* The perpendicular $w$ and constant $b$ are unknowns that need to be found.

* The goal is to put enough constraints on the system to be able to calculate $w$ and $c$ or $b$.

* The idea can be reformulated by expecting the decision rule applied on a positive sample $x^+$ to be greater than or equal to 1. i.e.

	$w.x^+ + b >= 1$ 

	here $x^+$ is **not an unknown**. It is an observed positive sample.
	
	Similarly for a negative sample, 
	
	$w.x^- + b <= -1$ 
	
* The target variable $y_i$ is defined as:
	
	$y_i = +1$ for positive sample($x^+$)
	   $i= -1$ for negative sample ($x^-$)

* Multiplying by the target variable to the updated decision rule, we get:
	
	$y_i(w.x_i + b) >= 1$
 $-> y_i(w.x_i + b) - 1 >= 0$
 
 * The points in the gutter:
	
	$y_i(w.x_i + b) - 1 = 0$
	
* The goal would be to maximize the width of the gutter.

* The width is:
	
			    w
  (x^+ - x^-) -----
			  ||w||
			  
	    w 
 -> 2 ----- 
      ||w||
	  
* The objective is to maximize the width i.e.

		  1						   1	
	max ----- -> min ||w|| -> min --- ||w||^2
		||w||					   2
		
* The extremum of a function with constraint can be found with Lagrange multipliers:
	
		 1			  ___	
	L = --- ||w||^2 - \   alpha_i[y_i(w.x_i + b) - 1]
	     2			  /__
		 
	Taking the gradient with respect to $w$,
	
	dou L		___
	----- = w - \  alpha_i y_i x_i = 0 (maxima-minima) 
	dou w       /__

		___
	w = \   alpha_i y_i x_i
		/__
	
		
	Taking the gradient with respect to $b$,
	
	dou L	___
	----- = \    alpha_i y_i = 0
	dou b	/__
	
	Plugging these back in to the original Lagrangian equation yields:

		___			   1  ___ ___
	L = \	alpha_i - --- \	  \	  alpha_i alpha_j y_i y_j x_i.x_j
		/__			   2  /__ /__
		
* The optimization only depends on the pairwise dot-products of the observed samples.
