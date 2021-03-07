# Chapter 8
# Optimization for Training Deep Models

* The objective is in finding the parameters $theta$ of a neural network that significantly reduces a cost function $J(theta)$

## Basic Algorithms

### Stochastic Gradient Descent

* SGD and its variants are the most used optimization algorithms for machine learning and deep learning.

* An unbiased estimate of the gradient is obtained by taking the average gradient on a minibatch of $m$ examples drawn i.i.d from the data-generating distribution.

* A crucial parameter for SGD is the learning rate $epsilon$. While in theory the learning parameter is kept fixed, in practice it is necessary to gradually decrease the learning rate over time.

* The decrease in the learning rate over time is due to the fact that the SGD gradient introduces a source of noise that does not vanish even when a minimum is reached.

* A sufficient condition to guarantee convergence of SGD is:

	inf	
	___
	\	epsilon_k = inf and,
	/__
	k=1
	
	inf
	___
	\	epsilon_k^2 < inf
	/__
	k=1
	
* In practice, **the learning rate is decayed linearly until iteration tau** by:
	$epsilon_k = (1 - alpha)epsilon_0 + alpha epsilon_tau$
	
with 
			 k
	alpha = ---
			tau

After iteration $tau$, it is common to leave $epsilon$ constant.

* Choice of $epsilon_0$, $epsilon_tau$ and $tau$ for the linear schedule is based on intuition. Usually, $epsilon_tau$ should be set to roughly 1 percent of the value of $epsilon_0$.

* The initial choice of $epsilon_0$ being too low can cause the learning to progress very slowly and may even get stuck with a high cost value.

* At the same time, having a large learning rate can cause violent oscillations causing the cost function to increase significantly.

* Typically, the optimal initial learning rate is higher than the learning rate that yields the best performance after the first 100 iterations or so.

* The most important property of SGD and related minibatch or online gradient based optimization is that **computation time per update does not grow with the number of training examples**. This allows convergence even when the number of training examples becomes very large.

* The study of convergence rate of an optimization algorithm can be done in terms of **excess error** $J(theta) - min_(theta)J(theta)$ which is the amount by which the current cost exceeds the minimum possible cost. When SGD is applied to a convex problem, the excess error is $O(1/sqrt(k))$ after $k$ iterations for a convex problem and $O(1/k)$ in the strongly convex case. **In theory, batch gradient descent enjoys better convergence rates than stochastic gradient descent**. The **Cramer-Rao Bound** states that the generalization error cannot decrease faster than $O(1/k)$. However, the asymptotic analysis obscures many advantages that stochastic gradient descent has after a small number of steps. SGD's ability to make rapid inital progress while evaluating the gradient for a few examples outwieghts its slow asymptotic convergence.

### Momentum

* SGD can be slow at times.

* The method of momentum is designed to accelerate learning especially in the face of high curvature, small but consistent gradients or noisy gradients.

* **It accumulates an exponentially decaying moving average of past gradieents and continues to move in their direction**.

* The momentum introduces a variable $v$ that plays the role of velocity. The velocity set to an exponentially decaying average of the negative gradient. The update equations are given by:

						   __          1  ___
	v <- alpha v - epsilon \/_theta ( --- \	 L(f(x^(i); theta),y^(i)))
									   m  /__
					
	theta <- theta + v
	
  $alpha$ is a hyperparameter in $[0,1)$ that determines how quickly the contributions of previous gradient exponentially decay.
													   __ 
* The velocity $v$ accumulates the gradient elements(the \/ term). The larger $alpha$ is relative to $epsilon$, the more previous gradients affect the current direction.

* While in SGD the size of each step is simply the norm of the gradient multiplied by the learning rate $epsilon$, in this case it also matters **how large and how aligned a sequence of gradients are**. The step size is largest when many successive gradients point in exactly the same direction.

* If the momentum algorithm always observes gradient $g$ then it accelerates in the direction of $-g$ till it reaches a terminal velocity where the size of each step is:

			epsilon ||g||
			-------------
			  1 - alpha
* Like $epsilon$, the parameter $alpha$ can be adapted over time but is less important than shrinking $epsilon$.

* An alternative to momentum is **Nesterov momentum** which follows the same procedure as momentum but with a difference in when the gradient is evaluated. In regular momentum, the gradient is evaluated **before** the current velocity is applied. In Nesterov momentum, the gradient is evaluated **after** the current velocity is applied. It can be thought of as adding a **correction factor** to the standard method of momentum. In SGD, it does not however improve the rate of convergence. The update equations are:


						   __          1  ___
	v <- alpha v - epsilon \/_theta ( --- \	 L(f(x^(i); theta + alpha v),y^(i)))
									   m  /__
					
	theta <- theta + v
	
## Algorithms with Adaptive Learning Rates

* The learning rate is reliably one of the most difficult to set hyperparameters because it significantly affects performance.

### AdaGrad

* The AdaGrad algorithm individually adapts the learning rates of all model parameters by scaling them inversely proportional to the square root of the sum of all the historical squared values of the gradient.

* The parameters with the largest partial derivative of the loss have correspondingly rapid decrease in their learning rate while parameters with small partial derivatives have a relatively small decrease in their learning rate. 
  
* The net effect is greater progress in the more gently sloped directions of parameter space.

* The update equations are given as:

		  1	 __		  ___
	g <- --- \/_theta \	  L(f(x^(i); theta), y^(i))
		  m			  /__
					   i
					   
	r <-r + g*g(squared norm)

				          epsilon
	 Delta theta <- - ---------------- *g
     			       delta + sqrt(r)
				  
	theta =  theta + Delta theta
	
	where g is the gradient and r is the gradient accumulation variable that is initially zero.
	
* The accumulation of squared gradients from the beginning of training can result in a premature and excessive decrease in the effective learning rate.

### RMSProp

* RMSProp modifies AdaGradd to perform better in the nonconvex setting by changing the gradient accumulation into an exponentially weighted moving average.

* AdaGrad is designed to converge rapidly in a convex setting. When a nonconvex function is used for training, the learning trajectory may end up stuck in a locally convex bowl.

* AdaGrad shrinks the learning rate according to the entire history of the squared gradient which makes the learning progress very slow.

* RMSProp uses an exponentially decaying average to discard history from the extreme past so that it can converge rapidly after finding a convex bowl.

* The update equations are given by:

		  1	 __		  ___
	g <- --- \/_theta \	  L(f(x^(i); theta), y^(i))
		  m			  /__
					   i
					   
	r <- rho r + (1 - rho) g*g

				          epsilon
	 Delta theta <- - ---------------- *g
     			       delta + sqrt(r)
				  
	theta =  theta + Delta theta

 where $rho$ is a decaying parameter.
 
### Adam

* The Adam optimization algorithm uses adaptive moments.

* It can be viewed as a combination of RMSProp and AdaGrad.

* In Adam, momentum is incorporated directly as an estimate of the first-order moment of the the gradient.

* The algorithm updates exponential moving averages of the gradient $m_t$ and the squared gradient $v_t$ where the hyperparameters $beta_1, beta_2 in [0, 1)$ control the exponential decay rates of these moving averages.

* The moving averages themselves are biased estimates of the 1st moment i.e. the mean and the 2nd moment i.e. the uncentered variance of the gradient.

* **The moving averages are initialized as vectors of 0s leading to moment estimates that are biased towards zero especially during the initial timesteps and especially when the decay rates are small**.

* The biases can easily be counteracted leading to bias-corrected estimates $m_t$ and $v_t$.

* The update equations are given by: 


		  1	 __		  ___
	g <- --- \/_theta \	  L(f(x^(i); theta), y^(i))
		  m			  /__
					   i
					   
	m_t <- beta_1 m_(t-1) + (1 - beta_1)g_t
	
	v_t <- beta_1 v_(t-1) + (1 - beta_1)g_t^2

			   m_t
	m_t <- ------------- (bias-corrected first moment estimate)
		   1 - beta_1^2
			   
			   v_t
	v_t <- ------------- (bias-corrected second moment estimate)
		   1 - beta_2^2
		  
											m_t 
	theta_t <- theta_(t-1) - epsilon -------------------
									  sqrt(v_t + delta)
									  
	where $delta$ is for stability.
	
* **Bias Correction**:
	* If $g_i$ is the gradient at step $i$ and $v_0$ is initialized as a vector of zeros, then the moving average can be re-written in the following way:
	
						 ___
		v_t = (1-beta_2) \	 beta_2^(t-i) g_i^2
						 /__
						 
	* Checking the relation between the expected value of $E(v_t)$ with $E(g_t^2)$,
	
	
						     ___
	   E(v_t) = E((1-beta_2) \	 beta_2^(t-i) g_i^2)
						     /__
			
			  = E(g_t^2)(1-beta_2)
			  
	Thus, the initialization of the initial parameters $v_t$ and $m_t$ as a vector of zeros introduces a biasing of $1-beta_1$ and $1-beta_2$ respectively which is then corrected.
	
## Optimization Strategies and Meta-Algorithms

### Batch Normalization

* It is a method of adaptive reparameterization motivated by the difficulty of training very deep models.

* Very deep models involved the composition of several functions or layers. The gradient defines the parameter updates based on the assumption that the other layers do not change. However, in reality, all layer parameters are updated simultaneously.

* The simultaneous updation of the parameters can lead to unexpected results.

* Batch normalization provides an elegant way of reparameterizing almost any deep network. It significantly reduces the problem of coordinating updates across many layers.

* Batch normalization can be applied to any input or hidden layer in a network.

* Batch normalization involves standardizing the values, column-wise by the column/feature mean and standard deviation.

* Back propagation through the standardization operation ensures that the gradient will never propose and operation that simply acts to increase the standard deviation or mean of the hidden output.
