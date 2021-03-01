# Chapter 10
# Sequence Modeling: Recurrent and Recursive Nets

* Family of neural networks for processing sequential data.

* RNNS take advantage of sharing parameters across different parts of a model.

* Using separate parameters for different time indices would not allow generalization to sequence lengths not seen during training.

* Parameter sharing also allows capture necessary information, not necessarily based on the structure and location of the information in the sequence.

* In RNNs, each output is a function of the previous members of the output and is produced using the same update rule as applied to previous outputs. This leads to parameter sharing.
 
* Notation and problem formulation adopted:
	* RNNs operating on a sequence that contains vectors $x^(t)$ where $t$ refers to the time step index and ranges from $1$ to $tau$.

* RNNs may also be applied in two dimensions across spatial data like images and can also have backward connections when applied to data involving time provided the entire sequence is observed before feeding back.

## Unfolding Computational Graphs

* Consider the classical form of a dynamical system:

	$s^(t) = f(s^(t-1), theta)$
	
 where $s^(t)$ is the state at time $t$. Since it depends on the state at time $t-1$, it defines a recurrent network.

* For a finite number of time steps $tau$, the dynamical system can be expanded till no such recurrence exists as:

	$s^(t) = f(f(f(f(....f(s^(1), theta), theta), theta), ... ,theta)$
	
 The repeated application of the definition yields an expression that does not involve recurrence and can now be represented by a traditional directed acyclic computational graph.
 
* Another example of such a dynamical system can consider an external signal $x^(t)$ to give the form:

	$s^(t) = f(s^(t-1), x^(t), theta)$
	
 The inclusion of the state $s^(t-1)$ serves the purpose of providing the present state information about the whole past sequence and thus the state encodes information about the whole past sequence.
 
* The above form is the most common form of expression for an RNN where the state $s$ replaced by $h$ to denote that the state is a hidden state/unit of the network as:
	
	$h^(t) = f(h^(t-1), x^(t), theta)$
	
* When an RNN is trained to perform a task that requires predicting the future from the past, the network typically uses $h^(t)$ as a kind of lossy summary of the task-relevant aspects of the past sequence of inputs up to $t$. 

* The summary is essentially lossy as the arbitrary length sequence of inputs $(x^(t), x^(t-1), ... ,x^(1)) is mapped to a fixed length vector.

* Depending on the training criterion, the summary may retain certain aspects of the sequence with more precision than the rest. 
  
## Recurrent Neural Networks

* Important design patterns for RNNS:
	
	* RNNs that produce an output at each time step and have recurrent connections between hidden units.
	
	* RNNS that produce an output at each time step and have recurrent connections but only from the output at one time step to the hidden unit at the next time step.
	
	* RNNS with recurrent connections between hidden units that read an entire sequence and then produce a single output.
	
### Forward propagation equations

* Starting at the initial state $h^(0)$, the update equations at each time step from $t=1$ to $t=tau$ for a **recurrent network that maps an input sequence to an output sequence of the same length** is given by:
	
	$a^(t) = b + Wh^(t-1) + Ux^(t)$
	$h^(t) = tanh(a^(t))$
	$o^(t) = c + Vh^(t)$
	$y_pred^(t) = softmax(o^(t))$

 where:
	$b, c$: bias vectors
	$U$: weight matrix for input-to-hidden connections
	$V$: weight matrix for hidden-to-output connections
	$W$: weight matrix for hidden-to-hidden connections 
	
* The total loss for a given sequence of $x$ values paired with a sequence of $y$ values is the sum of the losses over all the time steps. If the loss is the negative log-likelihood of $y(t)$ given $x^(1), x^(2), ... , x^(t)$ then the loss is given as:
	
	L({x^(1), x^(2), ... , x^(tau)}, {y^(1), y^(2), ... y^(tau)})
	   __
	=  \  L^(t)
	   /_ 
	    t
	   __
	=  \  log p_(model)(y^(t)|{x^(1), ... , x^(t)})
	   /_
	    t
where $log p_model(y^(t) | x^(1), ... , x^(t)})$ is given by reading the entry for $y^(t)$ from the output vector $y_pred^(t)$.

* The **gradient computation** of the loss function with respect to the parameters is an **expensive operation**:
	
	* it involves an entire forward pass through the unrolled graph followed by a back propagation pass. 
	  
	* The **runtime** is $O(tau)$ and cannot be reduced by parallelization due to the sequential nature.
	
	* The **memory cost** is also $O(tau)$ as tall the forward pass states need to be stored for the back propagation step.
	 
	* The back-propagation algorithm applied to the unrolled graph is called **back-propagation through time**.

### Teacher Forcing and Networks with Output Recurrence

* Networks with recurrent connections only from the output at one time step to the hidden units at the next time step are strictly less powerful due to the lack of hidden-to-hidden connections. This is because it is unlikely the output will capture relevant past information that can be used by the network to predict the future as the outputs are trained to match the target outputs.

* The **advantage** od eliminating hiiden-to-hidden recurrence is that for any loss function based on comparison of the output at time $t$ with the training target at time $t$, **all the time steps are decoupled**.
	* Training can be parallelized with the gradient at each step $t$ computed in isolation.
	
	* There is **no need to compute the output for the previous time step first** as the **training set provides the ideal value of the output**.

* Models with **output-to-hidden** connections can be trained with **teacher forcing**.

	* Teacher training arises from the maximum likelihood criterion in which the model receives the ground truth output $y^(t)$ as input at time $t+1$ during training.
	
	* During training, rather than feeding the model's own output back into itself, the connections should befed with the target values that specify what the correct output should be.
	
	* Originally motivated for avoiding back-propagation through time in models that lack hidden-to-hidden connections.
	
	* The **disadvantage** of teacher training arises if the network is used later in a closed loop mode where the network outputs are fed back as input.
	
		* The difference in the nature of the inputs that the network sees at training time vs. testing time could create problems.
		
		* This is mitigated by training the system with both free-training as well as teacher forcing so that it learns to account for the input conditions not seen during training.
		
		* Randomly using target inputs or the network outputs as inputs is another way to deal with the problem.

### Computing the Gradient in a Recurrent Network

* Computed in exactly the same manner as the gradients for a regular neural network. See the textbook pages 379-380 for the equations.

### Recurrent Networks as Directed Graphical Models

* The effect of the previous outputs on the future can be reduced from the entire sequence of outputs to only a certain length of look-back by assuming that the chain is Markovian.

* Inclusion of the entire past in the training objective(such as maximizing the log likelihood) is based on the belief that the dependance of an output $y^t$ on $y^i$ is not captured by the dependance of $y^t-1$ on $y^i$. 

* The inclusion of hidden values allows reduction of parameters but makes optimizing the parameters difficult. 
  
* Parameter sharing in RNNs rely on the assumption of **stationarity** i.e. the dependence between outputs at time $t+1$ and $t$ does not depend on the value $t$ itself. The time parameter $t$ can be used as an extra parameter to allow the network to discover time-dependance while sharing but this would require the model to extrapolate for new values of $t$.

#### Drawing samples from a model

* The main operation to be performed is to **sample from the conditional distribution** at each time step. However, the length of the sequence needs to be determined. This can be done in several ways:
	
	* When the output is a **symbol taken from a vocabulary**, a **special symbol corresponding to the end of the sequence** can be added. The sampling process is stopped when the stop symbol is generated.

	* An **extra Bernoulli output** can be introduced to the model representing the decision to either continue generation or halt generation. It is more general than the previous method as it can be applied to any RNN model. The extra output unit is **usually trained with cross-entropy loss**. The sigmoid is trained to maximize the log-probability of the correct prediction as the whether the sequence ends or continues at each time step.

	* The sequence length $tau$ can be determined by **adding an extra ouput to the model that predicts the integet $tau$ itself**. An extra input is added to the recurrent update at each time step so that the recurrent update is aware of the distance from the end of the sequence. It is based on the decomposition:
		
		$P(x^(1), x^(2), ... , x^(tau)) = P(tau) P(x^(1), x^(2), ... , x^(tau) | tau)$
		
### Modeling Sequences Conditioned on Context with RNNs

* Instead of taking a sequence of vectors $x^(t)$ as input, RNNs can also be designed in a manner that they take a single fixed-size vector $x$ as input.
	
	* The fixed-size vector can be made as an extra input of the RNN that generates the $y$ sequence. This extra input can be provided in one of the following ways:
		
		* As an **extra input** at **each time step**.
		
		* As the initial state $h^(0)$
		
		* Both
	
	* **As an extra input at each time step**:
		
		* this is the most common approach. 
		  
		* The interaction between the input $x$ and each hidden unit vector $h^(t)$ is parameterized by a weight matrix $R$.
		
		* The same product $x^T R$ is added as an additional input to each hidden unit.
		
		* The value $x^T R$ can be thought of as effectively a new bias parameter for each hidden unit.
	
	* Instead of using a single vector, a sequence of values $x^(1), x^(2), ... x^(tau)$ can be used which corresponds to a conditional distribution $P(y^(1), y^(2), ... , y^(tau) | x^(1), x^(2), ... , x^(tau))$ that makes a conditional independence assumption that the distribution factorizes as:
		___
		| | P(y^(t) | x^(1), x^(2), ... , x^(t))
		 t
	 The conditional independence assumption can be removed by adding connection from the output at time $t$ to the hidden unit at time $t+1$.
		 
		 * This model can then represent arbitrary probability distributions over the $y$ sequence.
		 
		 * This model has the **restriction** that the length of the sequence $x$ and $y$ must be the same.
	 
### Bidirectional RNNs

* Bidirectional RNNS were developed for applications in which the prediction at a time point depends **on the whole sequence** and **not just the past values of the sequence**.

* Bidirectional RNNs combine an RNN that moves forward through time beginning from the start of the sequence, and another RNN that moves backward through time beginning at the end of the sequence.

* If $h^(t)$ represents the forward moving sub-RNN's hidden layer and $g^(t)$ represents the backward-moving sub-RNN's hidden layer,  the output $o^(t)$ is computed using a combination of the outputs from both $h^(t)$ and $g^(t)$ allowing the output unit to compute a representation that depends on both the past and the future and is **most sensitive to the closest neigbhours** without having to specify a fixed window.

* The bidirectional RNN idea can also be applied to two-dimensional/spatial input.

### Encoder-Decoder Sequence-to-Sequence Architectures

* Used for applications where the **input sequence** and the **output sequence** are **not of the same length**.

* The input to the RNN is known as the "context". The goal is to produce a representation of this context. The context might be a vector or a sequence of vectors that summarize the input sequence.

* Basic methodology:
	
	* An **encoder** or **reader** or **input** RNN processes the input sequence. It emits the context $C$ usually as a simple function of its final hidden state.
	
	* A **decoder** or **writer** or **output** RNN is conditioned on the fixed-length vector generated by the encoder to generate the output sequence Y.

* In a sequence-to-sequence architecture, the encoder and decoder RNNS are **trained jointly** to maximize the average log likelihood i.e. $log P(y^(1), ... y^n_y|x^1, ... , x^n_x)$ over **all pairs of x and y sequences in the training set**.

### Deep Recurrent Networks

* Each RNN consists of blocks that represent the following transformations:
	* input to the hidden state
	* one hidden state to the next hidden state
	* thidden state to the output
  These are represented by one weight matrix each in a standard RNN.
  
* Adding more depth to each of the blocks would be advantageous.

* **Decomposing the RNN structure** into multiple layers can be advantageous.
	* The lower layers in the hierarchy of the decomposed structure can be thought of as playing a role in transforming the raw input into a representation that is more appropriate at the higher levels of the hidden state. 
	
* Separate MLPs can be added for each of the block operations but this makes optimization difficult. This can be mitigated by introducing skip connections in the hidden-to-hidden path.

### The Challenge of Long-Term Dependencies

* The basic problem is that gradients propagated over many stages tend to either vanish(most of the time) or explode(rarely).

* The difficulty with long-term dependencies arises from the exponentially smaller weights given to long-tem interactions compared to short-term ones.

* As a simplification, the recurrence relation can be thought of as:
	
	$ h^(t) = W^T h^(t-1)$
  
  Thus, on simplification, this yields,
	
	 $h^(t) = (W^t)^T h^(0)$
	
  Assuming an eigendecomposition of W of the form:
  
	 $W = QDQ^T$
	 
  Simplifies the recurrence relation further to:
  
	 $ h^(t) = Q^T D^t Q h^(0)$
	 
  As the eigenvalues are raised to the power $t$, eigenvalues with magnitude less than one decay to zero and eigenvalues with magnitude greater than one explode. **Any component of h^(0) that does not align with the largest eigenvector will eventually be discarded.**
  
* This problem is particular to the case of recurrent networks where the same weight is multiplied at each time step causing explosion of vanishing. If each multiplicand at each time step were different, the situation would be different.

* Whenever a model is able to represent long-term dependencies, the gradient of a long-term interaction has exponentially smaller magnitude than the gradient of a short-term interaction. This means that it takes a long time to learn long-term dependencies because these dependencies will tend to be hidden by the smallest fluctuations arising from short term dependencies.

* Increasing the span of the dependencies that need to be captured makes gradient-based optimization increasingly difficult with the probability of successfull training of a traditional RNN via SGD rapidly reaching 0 for sequences of only length 10 or 20.

### Echo State Networks

Pending

### Leaky Units and Other Strategies for Multiple Time Scales

* One way to deal with long-term dependencies is to design a model that operates at multiple time scales so that some parts of the model operate at fine-grained time scales and can handle small details while other parts operate at coarse time scales and transfer information from the distant past to the present more efficiently.

#### Adding Skip Connections through Time

* Skip connections introduce recurrent connections with a time delay of $d$ to mitigate the problem of vanishing and exploding gradients.

* Skip connections with time delay $d$ cause the gradients to diminish exponentially as a function of $tau/d$ rather than $tau$.

* Since there are both delayed and single step connections, gradients may still explode exponentially in $tau$.

* This allows the learning algorithm to capture longer dependencies although not all long-term dependencies may be represented weill this way.
