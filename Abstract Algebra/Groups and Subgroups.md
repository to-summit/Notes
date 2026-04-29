### Binary Operations

As we have done in elementary algebra: abstract any a number by presenting it by a symbol $x$, from now on , we need to abstract any a binary operation.

Here, we can reply a question: **WHAT** is an algebra ? We think of a binary operation on a set as giving an algebra on the set. Then what we are interested in is the *structural properties* of that algebra.

First, we define it : $$ *: S \times S \mapsto S$$ $$ \forall (a,b)\in S\times S, *(a, b)\text{  is difined}$$
Then **induced operation**: $$ H \subset S, \forall a, b \in H, a * b \in H$$ To arrive a correct conclusion about if a subset $H$ of $S$ is closed. *we have to know what is means for an element to be in $H$*

**Commutative**:$$* \text{ is commutative iif } \forall a,b\in S, a*b = b*a$$
**Associative**: $$\forall a,b,c\in S, (a*b)*c = a*(b*c)$$associative property is the theorem basis of expression $a*b*c$ , otherwise we must write it clearly.

**Identity element $e$**: $$ \exists e \in S, \forall a \in S, a*e = e*a = a$$
$e$ can be proofed is unique.

**Euler's equation:**$$e^{i\theta} = \cos(\theta) + i\sin(\theta)$$
can be used to covert a complex number from **Cartesian coordinates** to **Polar coordinates**. $$ z = a + bi , |z| = \sqrt{a^2 + b^2}\text{ ,then } z = |z|e^{i\theta}$$
From the geometrically illustration or *trigonometric identities* $$\left(\cos(\theta_1) + i\sin(\theta_1)\right)\left(\cos(\theta_2) + i\sin(\theta_2)\right) = \left(\cos(\theta_1)\cos(\theta_2)-\sin(\theta_1)\sin(\theta_2)\right)+i\left(\cos(\theta_1)\sin(\theta_2)+\cos(\theta_2\sin(\theta_1)\right) = \cos(\theta_1+\theta_2) + i\sin(\theta_1+\theta_2)$$ we can view complex multiplication as vector rotate and length multiplication. According to this property, we have multiplication on $U = \{z \in \mathbb{C}\mid |z| = 1\}$ has the same *structural property* as addition modulo $2\pi$ on $\mathbb{R}_{2\pi}$, they differ only in the name of elements and operations. This one-to-one correspondence is called **isomorphism**.

$U_n = \{z\in\mathbb{C} \mid z^n = 1\}$ is called $n^{th}\text{ root of unity}$, obviously, $U_n\subset U$. Define $$\zeta = \cos(\frac{2\pi}{n}) + i\sin(\frac{2\pi}{n})$$ we can rewrite the element in $U_n$ as $$\zeta^0, \zeta^1,\cdots,\zeta^{n-1}$$we can view multiplication on $U_n: \zeta^i \zeta^j$ as $i+_n j$ . 

**HINT**: Euler's equation can be formally derived by Taylor's series expansion $$ e^x = \sum_{i=0}^{n}\frac{x^i}{i!}, \sin(x) = (-1)^{n-1}\frac{x^{2n-1}}{(2n-1)!}, \cos(x) = (-1)^n\frac{x^{2n}}{(2n!)}$$ For infinite set, we can use **table** to represent its binary operations so that we can clearly see the *commutative* presented as *symmetric*: 

|  *  |  a  |  b  |  c  |  d  |
| :-: | :-: | :-: | :-: | :-: |
|  a  |  b  |  d  |  a  |  a  |
|  b  |  d  |  a  |  c  |  b  |
|  c  |  a  |  c  |  d  |  b  |
|  d  |  a  |  b  |  b  |  c  |

### Isomorphic Binary Structures

