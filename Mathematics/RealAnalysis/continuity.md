TARGET DECK: RealAnalysis
FILE TAGS: BabyRudin Continuity

START
Basic
Front: Define the limit of a function.
Back: Let X and Y be metric spaces suppose $E \subset X$, $f$ maps $E$ into $Y$ and $p$ is a limit point of $E$. Then $f(x) \to q$ as $x \to p$ or $$ lim_{x \to p} = q $$ if there is a point $q\in Y$ with the following property:
For every $\epsilon > 0$ there exists a $\delta > 0$ such that:
$$ d_Y(f(x),q) < \epsilon$$
for all points $x \in E$ for which
$$0<d_X(x, p) < \delta$$.
<!--ID: 1613827945560-->
END

START
Basic
Front: Comment on the limit of a function $f(x)$ in $E \subset X$ with respect to the limit of sequences $\{p_n\}$ in $E$.
Back:
$$lim_{x \to p} f(x) = q$$
if and only if
$$lim_{n \to \infty} f(p_n) = q$$
for every sequence $\{p_n\}$ in E such that
$$p_n \neq  p, lim_{n \to \infty} p_n=p$$

If $f$ has a limit at $p$, this limit is unique.
<!--ID: 1613829281481-->
END

START
Basic
Front: Define continuity of a function at a point and hence define continuous functions.
Back: Let $X$ and $Y$ be metric spaces, $E \subset X$ and $f$ maps $E$ into $Y$. Then $f$ is said to be continuous at $p$ if for every $\epsilon>0$ there exists a $\delta>0$ such that
$$d_Y(f(x),f(p))<\epsilon$$
for all points $x \in E$ for which $d_X(x, p)< \delta$
If $f$ is continuous at every point of $E$ then $f$ is said to be continuous on $E$.
<!--ID: 1613829380436-->
END

START
Cloze
Text: A function $f$ is continuous at $p$ if and only if {{c1::$lim_{x \to p} f(x) = f(p)$}}.
<!--ID: 1613830039308-->
END

START
Basic
Front: Define a composition of two mappings and the criteria for continuity of a composite mapping.
Back: Suppose $X, Y$ and $Z$ are metric spaces. Let $f$ map $E \subset X$ into $Y$ and $g$ map the range of $f$ into $Z$. Then
$$h(x) = g(f(x)), x\in E$$ 
is called the composition of the mapping $f$ and $g$
$$h = g \circ f$$
If $f$ is continuous at a point $p\in E$ and $g$ is continuous at $f(p)$ then $h$ is continuous at p.
<!--ID: 1613848583985-->
END

START
Basic
Front: Define a continuous mapping in terms of open sets.
Back: A mapping $f$ of a metric space $X$ into a metric space $Y$ is continuous on $X$ if and only if $f^{-1}(V)$ is open in $X$ for every open set $V$ in $Y$.
<!--ID: 1613848584001-->
END

START
Cloze
Text: Let $f_1, f_2, \dots, f_k$ be real functions on a metric space $X$ and let $f$ be the mapping of $X$ into $\mathrm{R}^k$ defined by:
$$ f(x) = (f_1(x), f_2(x), \dots , f_k(x)); x\in X$$
then, $f$ is continuous {{c1::if and only if each of the functions $f_1, f_2, \dots , f_k$ is continuous}}.
<!--ID: 1613848584015-->
END

START
Basic
Front: Define a bounded mapping.
Back: A mapping $f$ of a set $E$ into $R^k$ is said to be bounded if there is a real number $M$ such that $|f(x)| \leq M$ for all $x \in E$.
<!--ID: 1613848584030-->
END

START
Cloze
Text: A {{c1::continuous}} mapping of a compact metric space is {{c2::compact}}.
<!--ID: 1613848584041-->
END

START
Cloze
Text: If $f$ is a continuous mapping of a compact metric space $X$ into $R^k$ then $f(X)$ is {{c1::closed and bounded}}. Thus, $f$ is {{c1::bounded}}.
<!--ID: 1613848584053-->
END

START
Cloze
Text: If $f$ is a continuous real function on a compact metric space $X$ then, $f$ attains its {{c1:: supremum and infimum}} on $X$. That is, 
there exist points $p$ and $q \in X$ such that {{c1::$f(p) = sup_{x \in X}f(x)$ and $f(q) = inf_{x \in X}f(x)$}}.
<!--ID: 1613848584067-->
END

START
Cloze
Text: The {{c1::inverse}} mapping of a {{c2::continuous 1-1}} mapping of a {{c3::compact}} metric space $X$ onto a metric space $Y$ is continuous.
<!--ID: 1613849771802-->
END

START
Basic
Front: Define continuity of a mapping from a metric space $X$ into $R^k$ in terms of coordinate-wise continuity.
Back: Let $f_1, f_2, \dots ,f_k$ be real functions on $X$ and let $f$ be the mapping from $X$ into $R^k$ defined by
$$f(x) = (f_1(x), f_2(x), \dots , f_k(x)); x \in X$$
then $f$ is continuous if and only if each of the $f_i$s is continuous.
<!--ID: 1613886935525-->
END

START
Basic
Front: Define a bounded mapping from a set $E$ into $R^k$.
Back: A mapping $f$ from $E$ into $R^k$ is said to be bounded if there is a real number $M$ such that $|f(x)|\leq M$ for all $x \in E$.
<!--ID: 1613886935543-->
END

START
Cloze
Text: A continuous mapping of a {{c2::compact metric}} space is {{c1::compact}}.
<!--ID: 1613886935558-->
END

START
Cloze
Text: If $f$ is a continuous mapping of a metric space $X$ into $R^k$ then $f(X)$ is {{c1::closed and bounded}}. Thus $f$ is {{c2::bounded}}.
<!--ID: 1613886935569-->
END

START
Cloze
Text: A continuous function on a compact metric space attains its {{c1::maximum}} and {{c1::minimum}} on $X$.
<!--ID: 1613886935578-->
END

START
Cloze
Text: The inverse mapping of a {{c1:: continuous 1-1}} mapping $f$ of a compact metric space $X$ onto a metric space $Y$ is {{c1::continuous}}.
<!--ID: 1613886935588-->
END

START
Basic
Front: Define uniform continuity.
Back: Let $f$ be a mapping of a metric space $X$ into a metric space $Y$. $f$ is uniformly continuous on $X$ if for every $\epsilon >0$ there exists a $\delta > 0$ such that
$$d_Y(f(p), f(q)) < \epsilon$$
for every $p, q \in X$ for which $d_X(p,q) < \delta$.
<!--ID: 1613886935598-->
END

START
Cloze
Text: If $f$ is a continuous mapping of a compact metric space $X$ into a metric space $Y$ then $f$ is {{c1::uniformly continuous}} on $X$.
<!--ID: 1613886935608-->
END

START
Cloze
Text: Continuous mappings map {{c1::connected}} sets to {{c1::connected}} sets.
<!--ID: 1613886935617-->
END

START
Basic
Front: State the Intermediate Value Theorem.
Back: Let $f$ be a continuous real function on the interval $[a,b]$. If $f(a) < f(b)$ and if $c$ is a number such that $f(a)<c<f(b)$, then there exists a point $x\in (a,b)$ such that $f(x)=c$.
<!--ID: 1613886935626-->
END

START
Basic
Front: Define right hand and left hand limits.
Back: Let $f$ be defined on $(a,b)$. Consider any point $x$ such that $a\leq x<b$. 
$$f(x+) = q$$ 
If $f(t_n)\to q$ for all sequences ${t_n}$ in $(x, b)$ such that $t_n \to x$. This is known as the right hand limit. A similar analogue of the left hand limit is obtained by considering an $x$ in $a <x \leq b$ and considering a sequence in $(a,x)$.
<!--ID: 1613887703870-->
END

START
Basic
Front: Define the different types of discontinuity.
Back:
1)  If $f$ is discontinuous at $x$ and if $f(x-)$ and $f(x+)$ exist, then it is known as a simple discontinuity.
2) Otherwise, it is known as a discontinuity of the second kind.
<!--ID: 1613886935635-->
END

START
Basic
Front: State the two conditions under which a point can be a discontinuity of the first kind.
Back:
1) $f(x+) \neq f(x-)$
2) $f(x+) = f(x-) \neq f(x)$
<!--ID: 1613886935644-->
END

START
Basic
Front: Define a monotonic function.
Back: Let $f$ be real on $(a,b)$. $f$ is said to be monotonically increasing on $(a,b)$ if $a<x<y<b$ implies $f(x) \leq f(y)$. Reversing the last inequality gives a monotonically decreasing function.
<!--ID: 1613886935653-->
END

START
Cloze
Text: The {{c1::left and right hand limits}} exist at every point on which a monotonic function is defined. For a monotonically increasing function,
{{c1::$sup_{a<t<x}f(t)$}}$=f(x-) \leq f(x) \leq f(x+)=${{c1::$inf_{x<t<b}f(t)$}}.
<!--ID: 1613886935662-->
END

START
Cloze
Text: Monotonic functions have no discontinuities of the {{c1::second kind}}.
<!--ID: 1613886935671-->
END

START
Cloze
Text: Let $f$ be monotonic on $(a,b)$. Then the set of points of $(a,b)$ at which $f$ is discontinuous is {{c1::atmost countable}}.
<!--ID: 1613886995845-->
END
