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
	
* When an RNN is trained to perform a task that requires predicting the future from the past, the network typically uses $h^(t)$ as a kind of lossy summary of the task-relevant aspects of the past sequence of inputs up to $t$. The summary is essentially lossy as the arbitrary length sequence of inputs $(x^(t), x^(t-1), ... ,x^(1)) is mapped to a fixed length vector.
