# Linear Quadratic control (LQ control)

## Goal

Design a regulator which controls a linear system

$$\dot{x}(t) = Ax(t) + Bu(t),\quad x(0)=x_0$$

in according to minimize the cost function

$$ J(x_0,u(\cdot)) = \int_0^\infty x'(\tau)Qx(\tau) + u'(\tau)Ru(\tau) d\tau $$

with

$$ Q\geq0,\quad R>0 $$

NB: Q and R can be not symmetric because of for a not symmetric matrix $\Gamma$ and vector $\upsilon$ is real that

$$ \upsilon'\Gamma\upsilon = \upsilon'\frac{\Gamma+\Gamma'}{2}\upsilon $$

## Procedure

### Find an optimal solution

We need to find

$$ J^0(x(t),t) = \min\limits_{u} \int_0^\infty x'(\tau)Qx(\tau) + u'(\tau)Ru(\tau) \; d\tau $$

as for the generic optimal control we have to solve

$$ \frac{\partial J^0(x,t)}{\partial t} = - \min\limits_{u} x'Qx + u'Ru + \frac{\partial J^0(x,t)}{\partial x} (Ax + Bu) $$

find the optimal $u$ by setting to 0 the partial derivative of the part to be minimized with respect to $u$, in this way we find

$$ u^0 = -\frac{1}{2}R^{-1}B'\left(\frac{\partial J^0(x,t)}{\partial x}\right)' $$

complete the HJB equation with the optimal ingress $u^0$

$$x'Qx + (u^0)' Ru^0 + \frac{\partial J^0(x,t)}{\partial x} (Ax + Bu^0) + \frac{\partial J^0(x,t)}{\partial t} = 0$$

We try to solve it with the candidate function $J^0(x,t) = x'P(t)x$ where $P(t)$ is a symmetric matrix

$$ \frac{\partial J^0(x,t)}{\partial t} = x'\dot{P}(t)x, \quad \frac{\partial J^0(x,t)}{\partial x} = 2x'P(t) $$

compute the optimal input

$$ u^0 = -R^{-1}B'P'(t)x(t) = -K(t)x(t), \quad K(t)=R^{-1}B'P'(t) $$

Now we need to know the dynamic of $P(t)$, so we complete the HJB equation

$$x'Qx + 2x'P(t)Ax - x'P(t)BR^{-1}B'P'(t)x + x'\dot{P}(t)x = 0$$

where $2x'P(t)Ax=2x'\frac{P(t)A+A'P'(t)}{2}x$ so, simplify the $x'$ and $x$ we can rewrite the HJB as

$$\dot{P}(t) + Q - P(t)BR^{-1}B'P'(t) + P(t)A + A'P'(t) = 0$$

this equation is known as the differential Riccati equation.

### Time invariant control law

*If the pair $(A, B)$ is reachable, then the differential Riccati equation with $P(0)=0$ tends to a constant matrix $\bar{P} \geq 0$, semi-definite positive solution of*

$$A'\bar{P} + \bar{P}A + Q - \bar{P}BR^{-1}B'\bar{P} = 0$$

So, we can find a constant value for the gain $K$ and a time invariant control law

$$\bar{K} = R^{-1}B'\bar{P}, \quad u(t)=-\bar{K}x(t)$$

## Stability of the closed-loop system

With the time invariant control law the dynamics system became

$$\dot{x}(t) = (A - B\bar{K})x(t)$$

We introduce the concept that the matrix $Q$ can be partitioned as $Q=C'_qC_q$ where $C_q$ is not unique

*If the the **pair $(A,B)$ is reachable** and **pair $(A,C_q)$ is observable**, then the closed-loop is **asymptotically stable**.*

### Robustness

It's possible prove that the **LQ control** have a **margin phase at least equal of $60°$** and a **gain margin at least equal of $1/2$**

## Conclusion

The closed-loop system is in the form

$$\dot{x}(t) = (A - B\bar{K})x(t)$$

which is the same is gotten with the **poles placement** approach.
So, why use the **LQ control** instead of **poles placement control** if both lead us to the same control schema?

In **PLC** we have to choose the position of the poles of the closed-loop, understand where the poles have to be placed to get a wanted behavior is not so easy, so in **LQ control** we don't impose the poles but instead we "tell" to the system which behavior should be penalized.