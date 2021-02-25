# Laplace-Beltrami Eigenfunctions Towards and algorithm that understands geometry

* The Laplace-Beltrami operator, when applied to general objects and surfaces, defines a function basis well adapted to the geometry and the topology of the object.

## The discrete setting: Graph Laplacians

* The Graph Laplacian is a matrix defined by:
	$a_ij = w_i,j > 0$ if $(i,j)$ is an edge
	$ a_ii = -sum w_ij$
	$a_ij = 0$ if $(i,j)$ is not an edge
	
where $w_ij$ are the weights associated with the edges of the graph.

* In the eigendecomposition of the Graph Laplacian, the first eigenvector is $(1, 1, ... , 1)$ associated with the eigenvalue $0$.

* The second eigenvector is called the **Fiedler vector** and has several applications such as natural ordering of vertices and giving a general idea about the number of clusters that might exist in the data.

* The **Fiedler vector** $u$ is the solution of the following constrained minimzation problem:
	$Minimize F(u) = u^t L u = sum over i and j(w_ij (u_i - u_j)^2)$
	$ Subject to: sum over i (u_i) = 0 and sum over i(u_i^2) = 1$

* Thus, given a graph embedded in some space and the edge weights $w_ij$, the Fiedler vector defines a 1-dimensional embedding of the graph on a line that tries to respect the edge lengths/weights of the graph.

* The domain of finding such embeddings using more vectors is dimension reduction.

* The central goal of dimension reduction is to find a good parameterization of the input space given some input data. Most of these methods comprise of finding the eigenvectors of a similarity matrix which parameterizes the original input space.

* While for certain methods, the eigendecomposition is carried out for a dense similarity matrix for all data points, methods based on the Graph Laplacian use only local neighbourhoods. As a consequence, the used matrix is sparse and extracting its eigenvectors requires lighter computations. Due to the orthogonal nature of eigenvectors, the eigenvectors of the Laplacian can be used as a vector basis to represent functions.

* The eigenvectors and eigenvalues of the graph Laplacian contain both geometric and topological information.

## The continuous setting: Laplace Beltrami

* Given a surface $S$ let $X$ denote the space of real-valued functions defined on $S$. Given a norm $||.||$, the function space $X$ is said to be **complete** with respect to the norm if Cauchy sequences converge in $X$. A complete vector space is called a **Banach space**.

* The space $X$ is called a **Hilbert space** in the specific case of the norm defined by:
	$||f|| = sqrt(<f,f>)$
	where $<,>$ defines an inner product.

* The additional structure of the inner product allows one to define **function bases** and project onto these bases using the inner product. Given a function basis $b_i$ a function $f$ will be defined by:
	$f = sum over i(alpha_i b_i)$ where the coordinates $alpha_i$ with respect to the basis $b_i$ can be easily retrieved by projecting $f$ onto the basis i.e. $alpha_i = <f, b_i>$
	
### Operators

* An operator is a mapping of functions to functions. An operator is linear if it satisfies $L(af) = aL(f)$.

* An eigen function of an operator $L$ is a function $f$ such that $Lf = lambda f$ where the scalar $lambda$ is an eigenvalue of L.

* A linear operator $L$ is said to be Hermitian if $<Lf, g> = <f, Lg>$ for each f,g in X. The eigenvalues of a Hermitian operator are real and the eigenfunctions associated with different eigenvalues are orthogonal. Thus, considering the eigenfunctions of a Hermitian operator is a possible way of defining an orthonormal function basis associated to a given function space $X$.

### Chladni plates

* In cartesian space, the Laplace-Beltrami operator is given by the coordinate sum of second-order derivatives.

* The area of a surface $S$, the length of its borders and its genus can be extracted from the spectrum of the Laplace-Beltrami operator. Thus, the spectrum of the Laplace-Beltrami operator contains much geometric and topological information and is used as a signature for shape matching

* A **Nodal set** is defined to be the zero set of an eigenfunction. A nodal set partitions the surface into a set of nodal domains of constant size. Nodal sets and nodal domains are characterized by the following theorems:
	* the n-th eigenfunction has at most n nodal domains.
	* the nodal sets are curves intersecting at constant angles.
