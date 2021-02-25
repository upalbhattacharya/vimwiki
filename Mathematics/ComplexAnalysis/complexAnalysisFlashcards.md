TARGET DECK: Complex Analysis

START
Cloze
A function $u$ which satisfies Laplace's Equation {{c1:: $\Delta u = 0$}} is said to be {{c2::harmonic}}. The real and imaginary parts of an analytic function are thus {{c2::harmonic}}.
Tags: ahlfors_ch2
<!--ID: 1609604496876-->
END

START
Cloze
A sequence in the complex space is convergent if and only if it is a {{c1::Cauchy Sequence}}.
Back Extra: The real and imaginary parts of a Cauchy sequence are again Cauchy and if they converge, so does the original sequence.
Tags: ahlfors_ch2
<!--ID: 1609604496891-->
END

START
Cloze
Absolute convergence {{c1::$\implies$}} convergence. Explain.
Back Extra: A series $$a_1 + a_2 + a_3 + \dots$$ can be compared with the series of the absolute values of the terms i.e. $$|a_1| + |a_2| + |a_3|+ \dots$$. Now,
$$|a_1 + a_2 + a_3 + \dots + a_n| < |a_1| + |a_2| + |a_3|+ \dots + |a_n|$$
Therefore, if the series of absolute values is Cauchy and thus convergent, then so is the series itself.
Tags: ahlfors_ch2
<!--ID: 1609604496905-->
END

START
Cloze
An analytic function is {{c1::necessarily continuous}}. If $f(z) = u(z) + iy(z)$ what can be additionally said about x and y if f is analytic?
Back Extra: $x$ and $y$ are also continuous.
Tags: ahflors_ch2
<!--ID: 1609604496919-->
END

START
Basic
Front: Define a continuous function.
Back: A function $f(x)$ is said to be continuous at a point $a$ if and only if $\lim_{x\to a } f(x) = f(a)$. A continuous function is one which is continuous at all points where it is defined.
Tags: ahlfors_ch2
<!--ID: 1609604496936-->
END

START
Basic
Front: Define a pole and the order of a pole of a rational function.
Back: $$R(z) = \frac{P(z)}{Q(z)}$$ where $P(z)$ and $Q(z)$ are two polynomials with no terms in common, the zeros of Q(z) are called the poles of R(z).

The order of a zero of Q(z) is the order of the pole of $R(z)$. $R(z)$ is given a value $\infty$ at the zeros of $Q(z)$ and is thus considered as a function with values in the extended plane.
Tags: ahlfors_ch2
<!--ID: 1609604496950-->
END

START
Basic
Front: Define a zero of a polynomical of $deg\ h$.
Back: Given a polynomial $P(z)$ of degree n, in its factorization as:
\[P(z) = a_n(z - \alpha_1)(z - \alpha_2)(z - \alpha_3) \dots (z - \alpha_n)\]
where each $\alpha_i$ is a solution to $P(z) = 0$ and where not all $\alpha$s are necessarily distinct, if exactly h of them coincide, their common value is called a zero of $P(z)$ of order h.
Tags: ahlfors_ch2
<!--ID: 1609604496964-->
END

START
Basic
Front: Define the order of a rational function.
Back: A rational function has the same number of poles and zeros and this common number is known as the **order** of the rational function.
Tags: ahlfors_ch2
<!--ID: 1609604496978-->
END

START
Basic
Front: Define uniform convergence.
Back: The sequence $\{f_n(x)\}$ converges uniformly to $f(x)$ on the set $E$ if to every $\epsilon > 0$ there exists and $n_0$ such that $|f_n(x) - f(x)| < \epsilon$ for all $n \geq n_0$ and for all x in E.
Tags: ahlfors_ch2
<!--ID: 1609604496993-->
END

START
Basic
Front: Define the limit of a function.
Back: A function $f(x)$ is said to have the limit A as x tends to a:
\[\lim_{x\to a}f(x) = A\]
if and only if the following is true:

For every $\epsilon > 0$ there exists a $\delta > 0$ with the property that $|f(x) - A| < \epsilon$ for all values of x such that $|x - a| < \delta$ and $x \neq a$.
Tags: ahlfors_ch2
<!--ID: 1609604497007-->
END

START
Basic
Front: Express $f'(z)$ in terms of the partial derivatives of x and y. Express $|f'(z)|^2$ in terms of the partial derivatives. Comment on what this can be viewed as.
Back: Of the four forms of expressing $f'(z)$, one form is:
$f'(z) = \frac{\partial u}{\partial x} + i\frac{\partial v}{\partial x}$
The expression of $|f'(z)|^2$ in terms of the partial derivatives is:
$|f'(z)|^2 = \Big(\frac{\partial u}{\partial x}\Big)^2 + \Big(\frac{\partial u}{\partial y}\Big)^2 =  \Big(\frac{\partial u}{\partial x}\Big)^2 + \Big(\frac{\partial v}{\partial x}\Big)^2 = \frac{\partial u}{\partial x}\frac{\partial v}{\partial y} - \frac{\partial u}{\partial y}\frac{\partial v}{\partial x}$
The last expression shows that $|f'(z)|^2$ is the Jacobian of u and v with respect to x and y.
Tags: ahflors_ch2
<!--ID: 1609604497020-->
END

START
Basic
Front: For an analytic function f(z), what is the derivative of f with respect to $\bar{z}$? What does this suggest?
Back: $\frac{\partial f}{\partial \bar{z}} = 0$.

Thus the function is independent of $\bar{z}$ and a function of z alone.
This suggests that an analytic function is a true complex function of a complex variable as opposed to a complex function of two real values.
Tags: ahlfors_ch2
<!--ID: 1609604497034-->
END

START
Basic
Front: Given a harmonic function $u(x,y)$, how do you find an analytic function using it?
Back:Define $u(x,y) = \frac{1}{2}[f(x+iy) + \bar{f}(x - iy)]$
Then,
$$u(z/2, z/2i) = \frac{1}{2}[f(z) + \bar{f}(0)]$$
Assuming $\bar{f}(0)$ is real and equal to u(0,0), this yields,
$$f(z) = 2u(z/2, z/2i) - u(0,0)$$
Tags: ahlfors_ch2
<!--ID: 1609604497046-->
END

START
Basic
Front: How do you find the value of $R(\infty)$ for a rational function R(z)?
Back: Considering the function $R_1(z) = R(1/z)$ allows one to investigate the nature of the zero/pole at $\infty$ of R by investigating the nature of the zero/pole at 0 of $R_1$.
If
$$R(z) = \frac{a_0 + a_1z + \dots + a_nz^n}{b_0 + b_1z + \dots + b_mz^m}$$
then
$$R_1(z) = z^{m-n}\frac{a_0z^n +a_1z^{n-1} + \dots + a_n}{b_0z^m + b_1z^{m-1}+\dots +b_m}$$
If $m>n$, $R(z)$ has a zero of order $m-n$ at $\infty$. If $m<n$, the point at $\infty$ is a pole of order $n-m$. If $m=n$, $R(\infty) = a_n/b_m \neq 0,\infty$ i.e, it is neither a zero or a pole.
Tags: ahlfors_ch2
<!--ID: 1609604497060-->
END

START
Cloze
If $\{a_n\}$ is a sequence and it is known that $|b_m - b_n| \leq |a_m - a_n|$ for all pairs of subscripts for a sequence $\{b_n\}$ then if $\{a_n\}$ is a Cauchy sequence, $\{b_n\}$ {{c1:: is also a Cauchy sequence}}. Hence convergence of {{c1:: $\{a_n\}$}} implies convergence of $\{b_n\}$.
Tags: ahlfors_ch2
<!--ID: 1609604497074-->
END

START
Cloze
If $R(z) = \frac{P(z)}{Q(z)}$ is a rational function where P(z) is a polynomial of degree n and Q(z) is a polynomial of degree m, the total number of zeros and poles of R(z) are {{c1::equal}} only when considering the poles/zeros at {{c1::$\infty$}} and is equal to {{c1::the greater of the numbers m and n}}.
Tags: ahlfors_ch2
<!--ID: 1609604497089-->
END

START
Basic
Front: If a rational function $R(z)$ is of order p and it is known that $R(z) - a$ has the same poles as $R(z)$, then what can be said of the roots of $R(z) = a$?
Back: $R(z) = a$ has exactly p roots. It is known that $R(z)$ and $R(z) - a$ have the same poles and the number of poles and zeros are equal in number(the order of the rational function).
Tags: ahlfors_ch2
<!--ID: 1609604497102-->
END

START
Basic
Front: If $f(x)$ has the limit A as x tends to a, what can be said about the real and imaginary parts of $f(x)$?
Back: $$\lim_{x \to a} Re\ f(x) = Re\ A$$
$$\lim_{x \to a} Im\ f(x) = Im\ A$$
Tags: ahlfors_ch2
<!--ID: 1609604497115-->
END

START
Cloze
If $P(z)$ is a polynomial containing a zero $\alpha$ such that
$P(\alpha) = {{c2::P'(\alpha) = \dots = P^{(h-1)}(\alpha)}}= 0$ and {{c2::$P^{(h)}(\alpha) \neq 0$}} then $\alpha$ is {{c1:: a zero of $P(z)$ of order $h$}}.
Tags: ahlfors_ch2
<!--ID: 1609604497129-->
END

START
Cloze
If $R(z)$ is a rational function and a is any constant then the poles of $R(z) - a$ are {{c1::the same as that of $R(z)$}}.
Tags: ahlfors_ch2
<!--ID: 1609604497143-->
END

START
Cloze
If two {{c1::harmonic}} functions u and v satisfy the {{c1::CR equations}} then v is said to be the {{c2::conjugate harmonic function of u}}.
Tags: ahlfors_ch2
<!--ID: 1609604497157-->
END

START
Cloze
If $u(x,y)$ and $v(x,y)$ have {{c1:: continuous first-order partial derivatives}} which satisfy the CR equations then, $f(z) = u(z) + iv(z)$ is {{c1::analytic}} with continuous derivatives $f'(z)$ and conversely.
Tags: ahlfors_ch2
<!--ID: 1609604497170-->
END

START
Basic
Front: State and explain the Cauchy-Riemann differential equations and their importance.
Back: For an analytic function, $f(z) = u(z) + i v(z)$, the derivative exists at every point and the limit of the difference quotient does not depend on the path taken to approach this limit. Thus, if real values are chosen for h, the imaginary part ($z = x + iy$) becomes constant and the derivative becomes a partial derivative in x. This yields,
$$f'(z) = \frac{\partial u}{\partial x} + i\frac{\partial v}{\partial x}$$

In a similar fashion, taking only purely imaginary values ik instead of h,
$$f'(z) = -i\frac{\partial u}{\partial y} + \frac{\partial v}{\partial y}$$

Thus, by uniqueness of the derivative,
$$\frac{\partial f}{\partial x} =  -i \frac{\partial f}{\partial y}$$
which yields the CR equations:
$$ \frac{\partial u}{\partial x} = \frac{\partial v}{\partial y}\ and\ \frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}$$

These are the CR equations which must be satisfied by the real and imaginary part of any analytic function.
Tags: ahlfors_ch2
<!--ID: 1609604497184-->
END

START
Basic
Front: State and prove Lucas' Theorem.(Gauss-Lucas Theorem)
Back: **Theorem**: *If all zeros of a polynomial $P(z)$ lie in a half plane, then all the zeros of the derivative $P'(z)$ lie in the same half plane* **OR**
**Theorem**: *If $P(z)$ is a nonconstant polynomial with complex coefficients, all zeros of the derivative $P'(z)$ belong to the convex hull of the set of zeros of $P$.*

**Proof**: 
If $P(z) = a_n \prod_{i =1}^{n}(z - \alpha_i)$,
taking the logarithm derivative gives
$$\frac{P'(z)}{P(z)} = \sum_{i=1}^{n} \frac{1}{z - \alpha_i}$$
If some z is a zero of P' and $P'(z) \neq 0$ then,
$$\sum_{i=1}^{n}\frac{1}{z-\alpha_i} = 0$$
$$\implies \sum_{i=1}^{n}\frac{\bar{z} - \bar{\alpha_i}}{|z-\alpha_i|^2} = 0$$

This can be rewritten to show that
$$\Big(\sum_{i=1}^{n}\frac{1}{|z-\alpha_i|^2}\Big)\bar{z} = \Big(\sum_{i=1}^{n}\frac{1}{|z-\alpha_i|^2}\bar{\alpha_i}\Big)$$

Taking the conjugates it shows that the zero of $P'$, $z$, is a weighted sum with positive coefficients that sum to one of the zeros of the polynomial $P$ i.e. it is in the convex hull of the zeros of $P$.
By the above formulation if, for some $z$, $P(z) =P'(z) = 0$, then $z$ must be one of the zeros of $P(z)$ and is thus still in the convex hull of the zeros of $P(z)$ as it can be expressed as
$$z = 1\cdot \alpha_i + \Big(\sum_{j=1,j\neq i}^{n} 0\cdot a_j \Big)$$.
Tags: ahlfors_ch2
<!--ID: 1609604497196-->
END

START
Cloze
The derivative of a real function of a complex variable either has derivative {{c1::zero}} or the derivative is {{c1::non-existent}}. Explain why.
Back Extra: By the definition of the derivative of the function, $f'(a)$ can be real on one side as it is the limit of the quotients
$$\frac{f(a+h) - f(a)}{h}$$ 
as h tends to zero through real values.
On the other side it is also the limit of the quotients:
$$\frac{f(a+ih) - f(a)}{ih}$$
and as such is purely imaginary. However, since the derivative of the function is defined at a, it is unique and irrespective the path taken and so has to be zero, if it exists.
Tags: ahlfors_ch2
<!--ID: 1609604497210-->
END

START
Cloze
The derivatives of an analytic function are {{c1::itself analytic}}. Thus the functions u and v of an analytic function will have {{c1:: continuous partial derivatives of all orders}} and in particular {{c1:: the mixed derivatives will be equal}}. Using the information from the CR equations of an analytic function, express what $\Delta u$ and $\Delta v$ should be equal to.
Back Extra: $$\Delta u = \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0$$
$$\Delta v = \frac{\partial^2 v}{\partial x^2} + \frac{\partial^2 v}{\partial y^2} = 0$$
Tags: ahlfors_ch2
<!--ID: 1609604497224-->
END

START
Cloze
The general term of a convergent series tends to {{c1:: zero}}. Explain.
Back Extra: Applied to a series, Cauchy's convergence test yields
A series
$$a_1 + a_2 + a_3 + \dots$$

converges if and only if for every $\epsilon > 0$ there exists an $n_0$ such that 

$$|a_n + a_{n+1} + \dots + a_{n+p}| < \epsilon (i.e. |s_{n+p} - s_{n}| < \epsilon)$$

for all $n \geq n_0$ and $p \geq 0$. For p = 0, this yields $|a_n| <\epsilon$ thus showing that the general term of a convergent series tends to zero.
Tags: ahlfors_ch2
<!--ID: 1609604497238-->
END

START
Cloze
The sum of the orders of the {{c1::zeros}} of a polynomial is equal to the {{c2:: degree}} of the polynomial.
Tags: ahlfors_ch2
<!--ID: 1609604497252-->
END

START
Basic
Front: What are the poles of the derivative of a rational function $R(z)$? What are their orders?
Back: The derivative of a rational function R(z) is given as:
$$R'(z) = \frac{P'(z)Q(z) - Q'(z)P(z)}{Q(z)^2}$$
This derivative, when it exists, has the same poles as $R(z)$ but the order of each pole being increased by one.
Tags: ahlfors_ch2
<!--ID: 1609604497264-->
END

START
Basic
Front: What is a zero of a polynomial of order 1 also called?
Back: A simple zero.
Tags: ahlfors_ch2
<!--ID: 1609604497278-->
END

START
Basic
Front: What is absolute convergence?
Back: A series with the property that the series formed by the absolute values of the terms converges is said to be absolutely convergent.
Tags: ahlfors_ch2
<!--ID: 1609604497292-->
END

START
Basic
Front: What is an analytic function?
Back: The class of analytic functions is formed by complex functions of a complex variable which possess a derivative where the function is defined.
Tags: ahlfors_ch2
<!--ID: 1609604497305-->
END

START
Basic
Front: What is the singular part of a rational function $R(z)$?
Back: If
$$R(z) = \frac{P(z)}{Q(z)}$$
and R(z) has a pole at $\infty$, then on dividing $P(z)$ by $Q(z)$ until the remainder is of the degree at most equal to the denominator, $R(z)$ can be expressed as:
$$R(z) = G(z) + H(z)$$
where G(z) is a polynomial without constant term and $H(z)$ is finite at $\infty$. The degree of $G(z)$ is the order of the pole at $\infty$ and this polynomial $G(z)$ is known as the singular part of $R(z)$ at $\infty$.
Tags: ahlfors_ch2
<!--ID: 1609604497317-->
END

START
Basic
Front: When does the derivative of a complex function of a real variable exist?
Back: If $$z(t) = x(t) + i y(t)$$
where x and y are real valued functions then,
$$z'(t) = x'(t) + i y'(t)$$
and the existence of the derivative is equivalent to the simultaneous existence of the derivatives $x'(t)$ and $y'(t)$.
Tags: ahlfors_ch2
<!--ID: 1609604497329-->
END

START
Basic
Front: When does the derivative of a rational function $R(z)$ exist?
Back: The derivative of a rational function R(z) is given as
$$R'(z) = \frac{P'(z)Q(z) - Q'(z)P(z)}{Q(z)^2}$$
The derivative exists when $Q(z) \neq 0$
Tags: ahlfors_ch2
<!--ID: 1609604497341-->
END

START
Cloze
While in the real case, one can distinguish between the limits {{c1::$+\infty$}} and {{c1::$-\infty$}}, in the complex case there is only {{c1:: one infinite limit point}}.
Tags: ahlfors_ch2
<!--ID: 1609604497352-->
END

START
Basic
Front: Why is every polynomial an analytic function?
Back: Every constant is an analytic function with derivative 0. The simplest non-constant analytic function is z whose derivative is 1. Since the sum and product of two analytic functions are analytic, it follows that every polynomial
$$P(z) = a_0 + a_1z + a_2z^2 + \dots + a_n z^n$$
is an analytic function.
Tags: ahlfors_ch2
<!--ID: 1609604497364-->
END

