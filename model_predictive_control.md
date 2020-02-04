# Model Predictive Control (MPC)

In the **MPC** is a family of algorithms that has had an enormous industrial impact.

The main idea below the **MPC** is similar to that **optimal control**, we have to minimize a cost function, but unlike **OC** in the **MPC** the optimal control function is recalculated at each step: MPC is an online control technique.

## Linear system

Let's consider linear system

$$
    \dot{x}(t) = Ax(t) + Bu(t)\\
    y(t) = Cx(t)
$$

### Closed-loop control

We want compute a MPC control law minimizing the cost function

$$J(x(k),u(\cdot),k)=\sum\limits_{i=0}^{N-1}\left[ x'(k+i)Qx(k+i) + u'(k+i)Ru(k+i) \right] + x'(k+N)Sx(k+N)$$

where **N** is the size of the prediction window. Just like for **LQ control** we find the solution

$$u^0(k+i)=-K(i)x(k+i), \qquad i=0,1,...,N-1$$

where

$$K(i)=\left(R+B'P(i+1)B\right)^{-1}B'P(i+1)A$$

and $P(i)$ is computed by difference Riccati equation

$$P(i)=Q+A'P(i+1)A-A'P(i+1)B(R+B'P(i+1)B)^{-1}B'P(i+1)A$$

with $P(N)=S$

Now, we'll use only $K(0)$ to compute the control law at the current step

$$u_{MPC}(k)=-K(0)x(k)$$

For the next step we'll have to repeat each previous operations and so on.

### Open-loop control

We can say that

$$x(k+i)=A^ix(k)+\sum\limits_{j=0}^{i-1}A^{i-j-1}Bu(k+j), \qquad i>0$$

then, define

$$
    X(k)=\begin{bmatrix}
        x(k+1) \\ x(k+2) \\ \cdots \\ x(k+N)
    \end{bmatrix},\quad
    \mathcal{A}=\begin{bmatrix}
        A \\ A^2 \\ \cdots \\ A^N
    \end{bmatrix},\quad
    U(k)=\begin{bmatrix}
        x(k) \\ x(k+1)\\ \cdots \\ x(k+N-1)
    \end{bmatrix} \\\,\\
    \mathcal{B}=\begin{bmatrix}
        B & 0 & \cdots & 0 \\
        AB & B & \cdots & 0 \\
        \cdots & \cdots & \cdots & \cdots \\
        A^{N-1}B & A^{N-2}B & \cdots & B \\
    \end{bmatrix}
$$

so

$$X(k)=\mathcal{A}x(k)+\mathcal{B}U(k)$$

if we define the cost matrixes

$$
    \mathcal{Q}=\begin{bmatrix}
        Q & 0 & \cdots & 0 & 0 \\
        0 & Q & \cdots & 0 & 0 \\
        \cdots & \cdots & \cdots & \cdots & \cdots \\
        0 & 0 & \cdots & Q & 0 \\
        0 & 0 & \cdots & 0 & S \\
    \end{bmatrix},\quad
    \mathcal{R}=\begin{bmatrix}
        R & 0 & \cdots & 0 & 0 \\
        0 & R & \cdots & 0 & 0 \\
        \cdots & \cdots & \cdots & \cdots & \cdots \\
        0 & 0 & \cdots & R & 0 \\
        0 & 0 & \cdots & 0 & R \\
    \end{bmatrix}
$$

and define a new cost function

$$\bar{J}(x(k),U(k),k)=X'(k)\mathcal{Q}X(k)+U'(k)\mathcal{R}U(k)$$
