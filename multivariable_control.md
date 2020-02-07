# Multivariable control

## Poles

As in **SISO** system the poles are the solution of the equation

$$\det(sI-A)=0$$

### Compute from $G(s)$

It's possible compute the poles of the system directly from the transfer function $G(s)$.

If we call $\phi(s)$ the characteristics polynomial of the minimal realization of the system it's defined as $\det(sI-A)$ or the least common denominator of all the non null minors of any order of $G(s)$.

## Invariant zeros

If $\lambda$ is a invariant zero of the system, then exists an initial state $x_0$ and a vector $u_0$ such that, given an input $u(t)=u_0e^{\lambda t}$, the output is $y(t)=0, t\geq0$

So, it is clear that not all zeros of singular elements of $G(s)$ is an invariant zero

### Compute from $G(s)$

If we call $z(s)$ the polynomial of the invariant zeros of $G(s)$, it is defined as the greater common divisor of all numerators of all the minors of order $rank(G(s))$ of $G(s)$.

## Principal gains

In a MIMO system it is useful can plot a summarize of the bode plot of the elements of $G(s)$ for this reason we introduce the concept of principal gains.

$$
    \underline{\sigma}(G(j\omega)) \leq 
    \frac{
        \begin{Vmatrix}
            G(j\omega)U(j\omega)
        \end{Vmatrix}_2
    }{
        \begin{Vmatrix}
            U(j\omega)
        \end{Vmatrix}_2
    }
    \leq \overline{\sigma}(G(j\omega))
$$

Another index useful for the analysis of the system is

$$\gamma(G(j\omega))=\frac{\overline{\sigma}(G(j\omega))}{\underline{\sigma}(G(j\omega))}$$

the idea is that a system with $G(j\omega)$ are easy to be controlled with a $\gamma(G(j\omega))$ close to $1$.

## Closed-loop

### Stability

#### Nyquist criteria

Let $P_{ol}$ be the number of the poles with positive real part of the open-loop system $L(s)$. The closed-loop with negative feedback is asymptotically stable if and only if the Nyquist plot of $\det(I+L(s))$ doesn't pass through the origin and the number of its encirclements (anticlockwise positive) around the origin are equals to $P_{ol}$.