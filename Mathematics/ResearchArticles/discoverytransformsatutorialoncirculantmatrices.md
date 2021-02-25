# Discovering Transforms: A tutorial on Circulant Matrices, circular convolution and the discrete Fourier Transform

* Simultaneous diagonalization of any class of linear operators or matrices is the ultimate way to understand their actions, by reducing the entire class to the simplest form of linear operations as diagonal matrices simultaneously.

**Circulant Matrix:**

Given an n-vector $a = (a_0, a_1, ... , a_n-1)$, the associated matrix $C_a$ whose first column is made up of the elements of $a$ and each subsequent column is obtained by a **circular shift** of the previous column as:

 _							_
| a_0   a_n-1 a_n-2 ... a_1  |
| a_1    a_0  a_n-1 ... a_2  |
| a_2    a_1   a_0  ... a_3  |
| .       .      .  ...  .   |
| a_n-1 a_n-2 a_n-3 ... a_0  |
            

is known as a circulant matrix. It can be similarly defined as a matrix in which each row is a circular shift of the previous row. Thus, it is completely determined by any one of its rows or columns.

**Discrete Fourier Transform:**

Given an n-vector $a$ as before, its DFT is another n-vector whose coordinates are given by:

$ a_k = sum l over n coordinates (a_l e^(-i*2*pi/n*k*l))

* The eigenvalues of a Circulant matrix are precisely the coordinates of the DFT of its vector $a$.

* **A set of matrices is simultaneously diagonalizable iff they share a full set of eigenvectors.**

* An equivalent condition to the above is that they are diagonalizable and they all mutually commute.

* Given a mutually commuting set of matrices, by finding their shared eigenvectors, one finds the special transformation that simultaneously diagonalizes all of them.

* Circulant matrices represent a class of mutually commuting operators that also commute with the action of the group $Z_n$.

## Simultaneous Diagonalization of Commuting Matrices

* When a matrix $M$ can be diagonalized with a similarity transformation (i.e. $D = P^(-1)MP$ where $D$ is diagonal) then it provides a change of basis in which the linear transformation has the simple diagonal matrix representation and its properties can be easily understood.

* **Definition:** A set $S$ of matrices is called **simultaneously diagonalizable** if there exists a single transformation that diagonalizes all matrices in $S$. There exists a single non-singular matrix $V$ such that

for each $M$ in $S$, $V^(-1)MV$ is diagonal.

* Thus simultaneous diagonalization allows all kinds of products, sums and inverses to also be diagonalized easily.

* **If a matrix** $A$ **has simple eigenvalues, then** $A$ **and** $B$ **are simultaneously diagonalizable iff they commute. In that case, the diagonalizing bassis is made up of the eigenvectors of** $A$.

## Structural Properties of Circulant Matrices

* For a circulant matrix $C_a$, the entries of the matrices can be defined up modulo arithmetic of the indices. i.e

${C_a}_kl = a_k-l$

The $(k,l)$th entry of the circulant matrix is the $k-l$ coordinate value of the vector $a$ where $k-l$ is calculated under $mod n$.

### Symmetry Properties of Circulant Matrices

* The **circular right-shift operator** $S$ and its adjoint, the **circular left-shift operator** $S*$ are special circulant matrices. Their actions on a vector $x$ is given by:
	$S(x_0, x_1, ... , x_n-2, x_n-1) = (x_n-1, x_0, ... , x_n-2)$
	$S*(x_0, x_1, ... , x_n-2, x_n-1) = (x_1, x_2, ... , x_n-1, x_0)$
	OR
	$(Sx)_k = x_k-1$
	$(S*x)_k = x_k+1$
where modular arithmetic is used for computing the final indices.

* The circular shift operator $S$ commutes with any circulant matrix as left mutliplication amounts to row permutations while right multiplication amounts to column permutation. The structure of the circulant matrix shows these two as yielding the same matrix. Thus
$SC_a = C_aS$

* **A matrix** $M$ **is circulant iff it commutes with the circular shift operator** $S$. i.e. $SM = MS$.

* The above lemma provides shift invariance of of circulant matrices. If the circulant matrix $M$ was considered a linear operator acting on a vector $x$, then we have:
	$SMx = MSx$
which shows that the action of $M$ on x is shift-invariant.

### Circular Convolution

* The action of a circulant matrix on a vector is a circular convolution as given by:
	$ y = C_a x$
Thus the coordinates of the vecor are:
	$y_k = sum l from 0 to n-1(a_k-l x_l)$
Which is exactly a circular covolution of two vectors:
	$y = a * x  <=> y_k = sum l from 0 to n-1(a_k-l x_l)$

* **Circular convolution of any two vectors can be written as a matrix-vector product with a circulant matrix**.
	$a * x = C_a x = C_x a$

* The product of any two circulany matrices is another circulant matrix and circulant matrices commute.

* The set of all n-vectors forms a commutative algebra under the operation of circular convolution and the set of $n x n$ circulant matrices under standard matrix multiplication is also a commutative algebra isomorphic to n-vectors with circular convolution.

## Simultaneous Diagonalization of all Circulant Matrices yields the DFT

* Finding the eigenvector decomposition of $S$ or $S*$ will provide the matrix to carry out simultaneous diagonalization of all circulant matrices.

* Eigen decomposition of $S*$ provided the classically defined DFT.

### Construction of Eigenvectors/Eigenvalues of S*

* If $lambda$ is an eigenvalue of $S*$ with eigenvector $w$, then $(lambda)^l$ is an eigenvalue of $(S*)^l$ with eigenvector $w$. Using the nature of the $S*$ operator,
	$S* w = lambda w <=> w_k+1  = lambda w_k$
	$(S*)^l w = (lambda)^l w <=> w_k+1 = (lambda)^l w_k$
Now, the periodic nature of circular matrices shows that
	$w_k+n = (lambda)^n w_k <=> w_k = (lambda)^n w_k$
As eigenvectors are non-zero vectors by definition, one of the coordinates of the eigenvector $w$ is non-zero. Therefore
	$(lambda)^n = 1$
Solving the above shows that the $n$ eigenvalues of $S*$ are precisely the $n$ distinct $nth$ roots of unity.

Taking the $mth$ of the $n$ roots of unity($p_m$), the corresponding eigenvector is given by
	$w^(m) = w_0(1, p_m, p_m^2, ... , p_m^n-1)$
where $w_0$ is the first coordinate of the eigenvector.

* The $n$ distinct eigenvalues of the $S*$ are the $nth$ roots of unity $p_m = e^(i*2pi/nm) = p^m$ and the corresponding eigenvectors are 
  $w^(m) = (1, p^m, p^2m, .. , p^m(n-1)$

### Eigenvalues Calculation of a Circulant Matrix Yields the DFT

The eigendecomposition of either $S$ or $S*$ yields the eigenbasis that facilitates simultaneous diagonalization of circulant matrices. Thus, the eigenvectors of a circulant matrix are:
	$ w^(m) = (1, p^m, p^2m, ... , p^m(n-1))$
Looking at the eigen problem:
	$C_a w^(m) = (lambda)_m w^(m)$
Yields a form which can be solved to get the equation:
	$lambda_m = a_0 + a_n-1 p_m + ... + a_1 p^(n-1)_m$
	$= sum l from 0 to n-1 (a_l p^(-ml))$
	$= sum l from 0 to n-1 (a_l e^(-i*2pi/n*ml))$
Which is precisely the classically-defined DFT of the vector $a$.

* The matrix $W$ consisting of the eigenvectors $w^m$ is a unitary matrix. Multiplying a vector by its complex conjugate $W*$ directly gives the DFT while multiplying by $1/n W$ gives the inverse DFT.

* The action of a circulant matrix $C_a$ on $x$ or equivalently, the circular convolution of the underlying vector $a$ of $C_a$ with $x$ can be performed by:
	* first tasking the DFT of $x$
	* then multiplying the resulting vector component-wise by the DFT of the vector $a$
	* and finally taking an inverse DFT by multiplying it by $1/n W$
