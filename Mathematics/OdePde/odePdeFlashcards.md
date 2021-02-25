TARGET DECK: ODEPDE

START
Basic
Front: Define a separable ODE.
Back: An ODE that can be reduced to the form:
$$g(y) y' = f(x)$$
is known as a variable separable ODE as the two sides of the equations are functions of x and y alone separately. This ODE can be solved by integrating both sides with respect to x to obtain:

$$\int g(y) y' dx = \int f(x) dx + c$$
$$\implies \int g(y) dy = \int f(x) dx + c$$
Tags: schaums_ch1
<!--ID: 1609597743225-->
END

START
Basic
Front: Define a solution of an ODE. Hence define general and particular solutions of an ODE.
Back: A function $y = h(x)$ is called a solution of a given ODE on some open interval if $h(x)$ is defined & differentiable throughout the interval and is such that the equation becomes an identity if $y$ and $y'$ are replaced with $h$ and $h'$ respectively.

A solution to an ODE containing an arbitrary constant is called a general solution of the ODE.
The general solution of an ODE is a family of infinitely many solution curves parameterized by the arbitrary constant. Choosing a particular value of the constant gives a particular solution of the ODE.
Tags: schaums_ch1
<!--ID: 1609597743238-->
END

START
Basic
Front: Define the Initial Value Problem for an ODE.
Back: An ODE $y' = f(x,y)$ together with an initial condition $y(x_0) = y_0$ is called an initial value problem.
Tags: schaums_ch1
<!--ID: 1609597743250-->
END

START
Basic
Front: Discuss exact ODEs, the method of checking exactness and their solutions.
Back: An ODE of the form:
$$Mdx + Ndy = 0$$
is said to be an exact ODE if:
$$\frac{\partial M}{\partial y} = \frac{\partial N}{\partial x}$$

The idea comes from assuming a function u in two variables x and y and defining $du = 0$
Thus, taking the differential of u we get,
$$du = \frac{\partial u}{\partial x} dx + \frac{\partial u}{\partial y}dy = 0$$
Thus $M = \frac{\partial u}{\partial x}$ and $N =  \frac{\partial u}{\partial y}$ and so their cross derivatives should be the same.
To solve the ODE, find
$$u(x,y) = \int M dx + k(y)$$ where k is a constant as a function of y.
Then take the derivative of u with respect to y, equate it to N and find $\frac{dk}{dy}$. Integrate it to get k(y) to obtain the solution of u(x,y).
Tags: schaums_ch1
<!--ID: 1609597743263-->
END

START
Basic
Front: Discuss solutions to Bernoulli Equations(reducible to linear ODEs).
Back: An ODE of the form,
$$y' + p(x)y = g(x)y^{\alpha},\ \alpha \neq 0, 1$$ can be converted to a linear form by taking $u = y^{1-\alpha}$
The final form of the linear ODE is:
$$u' + (1 - \alpha)pu = (1-\alpha)g$$
*(Write out the solution steps for yourself on the whiteboard)*.
Tags: schaums_ch1
<!--ID: 1609597743277-->
END

START
Basic
Front: Discuss the solutions of Homogeneous and Non-homogeneous Linear ODEs.
Back: **Homogeneous Linear ODE**
$$y' + p(x)y = 0$$
The solution is given by:
$$y = ce^{-\int p(x) dx}$$

**Nonhomogeneous Linear ODE**
$$y' + p(x)y = r(x)$$
Taking the form $(py - r)dx  + dy = 0$ creates an ODE of the exact or reducible to exact variety. The integrating factor is given by $F = e^{\int p(x)dx}$
Thus the final solution is given by:
$$y = e^{-\int p(x)dx}\int e^{\int p(x)dx}r(x) dx + ce^{-\int p(x)dx}$$
Tags: schaums_ch1
<!--ID: 1609597743290-->
END

START
Basic
Front: Give the notion of how an ODE can be reduced to the format of a separable ODE.
Back: For an ODE $y' = \frac{y}{x}$ putting $u = \frac{y}{x}$ allows one to convert the ODE into a variable separable one in u and x.
Tags: schaums_ch1
<!--ID: 1609597743303-->
END

START
Basic
Front: How do you find the integrating factor for an ODE that is reducible to exact form?
Back: If an equation $Pdx + Qdy = 0$ is not exact then it may be made exact by mutliplying with an appropriate integrating factor function F.
If the quantity $$R = \frac{1}{Q}\Big[\frac{\partial P}{\partial y} - \frac{\partial Q}{\partial x}\Big]$$ is a function of x alone then the integrating factor is given by:
$$F(x) = e^{\int R(x) dx}$$
If the quantity $$R = \frac{1}{P}\Big[\frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y}\Big]$$ is a function of y alone then the integrating factor is given by:
$$F(y) = e^{\int R(y) dy}$$.
Tags: schaums_ch1
<!--ID: 1609597743315-->
END

START
Basic
Front: How do you find the orthogonal trajectories for a given family of curves?
Back: Given a family of curves $G(x,y,c) = 0$ first eliminate the constant and define the first order ODE

$$y' = f(x,y)$$

Then solve the ODE:

$$y'_1 = \frac{1}{f(x,y_1)}$$ to get the orthogonal family.
Tags: schaums_ch1
<!--ID: 1609597743329-->
END

START
Basic
Front: State the existence theorem.
Back: Let the right side f(x,y) of the ODE in the initial value problem
$$y'=f(x,y), y(x_0) = y_0$$
be continuous at all points (x,y) in some rectangle
$$R: |x - x_0| < a, |y - y_0| < b$$
and bounded in R i.e. $|f(x,y) \leq K \forall (x,y) \in R$.

Then the IVP has at least one solution. This solution exists at least for all x in the sub interval $|x-x_0|<\alpha$ where $\alpha$ is the smaller of a and b/K.
Tags: schaums_ch1
<!--ID: 1609597743343-->
END

START
Basic
Front: State the uniqueness theorem.
Back: Let f and its partial derivative $\frac{\partial f}{\partial y}$ in an IVP be continuous in all (x,y) in a rectangle R and bounded i.e.
$$|f(x,y)| \leq K\ and\ |f_y(x,y)| \leq M\ \forall x,y \in R$$
Then the IVP has atmost one solution y(x).
Tags: schaums_ch1
<!--ID: 1609597743356-->
END

START
Basic
Front: What are the implicit and explicit forms of a first order ODE?
Back:Implicit form:
$$F(x,y,y') = 0$$

Explicit form:
$$y' = f(x, y)$$
Tags: schaums_ch1
<!--ID: 1609597743368-->
END

START
Basic
Front: What is a first order linear ODE?
Back: A first order ODE is linear if it is of the form:
$$y' + p(x)y = r(x)$$
Tags: schaums_ch1
<!--ID: 1609597743380-->
END

START
Basic
Front: What is the order of an ODE?
Back: An ODE is said to be order of **n** if the nth derivative of the unknown function y is the highest derivative in the equation. A first-order ODE is one which contains only the first derivative y' and may contain y and any given function of x.
Tags: schaums_ch1
<!--ID: 1609597743393-->
END
