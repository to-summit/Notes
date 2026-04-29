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
We have seen many binary operations. Many of them have the same *structural property*. It's time to give a definition for **isomorphism:** An isomorphism of $S$ and $S'$ is a *one-to-one function* $\phi$ mapping $S$ *onto* $S'$ such that: $$\phi(x * y) = \phi(x) *' \phi(y)\text{ for all x, y} \in S$$ we call this ***homomorphism property***. The *homomorphism* is not necessary an *isomorphism*. If we can find such a function, we can infer that $S$ and $S'$ are ***isomorphic binary structure***. Simply denoted as $$ S \simeq S' $$
#### How to Show That Binary Structures Are Isomorphic
1. *Define a function $\phi$ gives the isomorphism of $S$ with $S'$* ($\forall x\in S,\text{ what is } \phi(x)\text{ to be}$)
2. *Show that $\phi$ is one-to-one function* ($\phi(x)=\phi(y) \Rightarrow x = y$)
3. *Show that $\phi$ is onto $S'$* ($\forall s \in S', \exists x \in S\text{ satisfy }\phi(x) = s$)
4. *Show that $\phi(x*y) = \phi(x)*'\phi(y)$* (show two sides is equal)

#### How to Show That Binary Structures Are Not Isomorphic
1. *Show that $S$ and $S'$ don't have the same cardinality*.
    > **Hint** you can't show this by indicate $S' \subset S$, think of $\mathbb{Z}$ and $2\mathbb{Z}$.
2. What if they indeed have the same cardinality? *We can think about show that some structural property in $S$ is not the $S'$ possesses*. Here is an amazing example and some usual properties: 
![[example-and-properties.png]] 
> 	**Hint** , if exists identity element is a structural property.

### Groups
How to apply these binary operations to the solution of problems?
From solving a linear equation. How can we solve $$a + 5 = 2, b\times 2=3$$ $$a+5+(-5)=2+(-5),  b\times 2\times \frac{1}{2}= 3\times\frac{1}{2}$$
$$a+0 = -3, b\times1 = \frac{3}{2}$$
$$a = -3, b = \frac{3}{2}$$
In order to solve an equation, we need to have **inverse $x'$($-5,\frac{3}{2}$), identity $e$($0, 1$), commutative.** We naturally define a binary operations, having these structural properties, and its underlying set is a **group**.

***FORMALLY DEFINITION*** : A group $\langle G, *\rangle$ is a **set** $G$, closed under a binary operation, satisfying following axioms:
$$\mathcal{G}_1: \forall a, b, c \in G, (a*b)*c = a*(b*c) \Rightarrow \text{associativity of } *$$ 
$$\mathcal{G}_2: \exists e\in G, \forall x\in G, x*e= e*x = x \Rightarrow\text{identity element of }*$$
$$\mathcal{G}_3:\forall a\in G, \exists a'\in G,a*a'=e \Rightarrow\text{inverse element under *}$$
Because **isomorphism** stands for *same structural property*, we just define a group by specify some necessary property. So if $\langle G,*\rangle \simeq \langle G', *'\rangle$ and $G$ is a group, then $G'$ is definitely a group. For simplicity, a group $\langle G, *\rangle$ is denoted by $G$.

A group $G$ is an **abelian** if its binary operation is commutative.
All *invertible* $n\times n$ matrices under matrix multiplication is a group, denoted by $GL(n, \mathbb{R})$ , the so-called **general linear group of degree n.** According to *vector space*, all invertible matrix can give rise to an invertible linear transformation. We denoted this transformation by $GL(\mathbb{R}^n)$. Of course, $$GL(n,\mathbb{R}) \simeq GL(\mathbb{R}^n)$$
Iteration-like three theorems:
1. The **left and right cancellation laws** holds in group. That is to say $$a*b = a* c \Rightarrow b = c, b* a = c* a\Rightarrow b = c$$ This can be simply proofed b
2. If 
$$\forall a, b \in G, a*x= b,y*a =b$$have unique solutions $x$ and $y$ in $G$. This can be proofed by definition and last theorem. **Note** $a'b$ and $ba'$ no need to be the same unless it's an abelian.
3.  The identity and inverse are unique in a group. The uniqueness of identity in a group is decided by the binary operation. The uniqueness of inverse can be proofed by the last two theorem and definition.

**Corollary:** $$\forall a, b \in G, (a*b)' = b'*a'$$
$\mathcal{G}_1$ can only define a **semigroup** and $\mathcal{G}_1 \wedge \mathcal{G}_2$ can only get a **monoid**. 
We can weaken the conditions defining a group: 
$$\mathcal{G}_1:*\text{ is associative} $$
$$\mathcal{G}_2:\exists e_l \in G, e_l*a = a,\text{ for } \forall a \in G$$
$$\mathcal{G}_3: \exists a'_l\in G,a'_l*a = e, \text{for }\forall a\in G$$
### Finite Groups and Group Tables
We have seen that we can use tables to research the finite sets. What the table will be if the set is a group? The answer is in each row and each column, any an element in $S$ must be appear **only once**. Through this conclusion, we can proof all groups with one or two or three elements are *isomorphic*. (I feel some relations with little like eight-queens question)

