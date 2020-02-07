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