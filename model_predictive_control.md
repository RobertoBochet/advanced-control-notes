# Model Predictive Control (MPC)

In the **MPC** is a family of algorithms that has had an enormous industrial impact.

The main idea below the **MPC** is similar to that **optimal control**, we have to minimize a cost function, but unlike **OC** in the **MPC** the optimal control function is recalculated at each step: MPC is an online control technique. In other words at each step ($k$) we calculate the optimal control function for all steps in the window $[k,k+N-1]$, then we apply to the system only the control law calculate for $k$.

## Linear system

Let's consider linear system

$$
    x(k+1) = Ax(k) + Bu(k)\\
    y(k) = Cx(k)
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
        u(k) \\ u(k+1)\\ \cdots \\ u(k+N-1)
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

that we can rewrite as

$$
    \bar{J}(x(k),U(k),k)=(\mathcal{A}x(k)+\mathcal{B}U(k))'\mathcal{Q}(\mathcal{A}x(k)+\mathcal{B}U(k))+U'(k)\mathcal{R}U(k) \\
    =x(k)\mathcal{A}'\mathcal{Q}\mathcal{A}x(k)+2x'(k)\mathcal{A}'\mathcal{Q}\mathcal{B}U(k)+U'(k)(\mathcal{B}'\mathcal{Q}\mathcal{B}+\mathcal{R})U(k)
$$

and if we try to minimize this cost function respect with $U(k)$ we find

$$
    U^0(k)=-(\mathcal{B}'\mathcal{Q}\mathcal{B}+\mathcal{R})^{-1}\mathcal{B}'\mathcal{Q}\mathcal{A}x(k)=-\mathcal{K}x(k) \\
    \quad\\
    \mathcal{K}=(\mathcal{B}'\mathcal{Q}\mathcal{B}+\mathcal{R})^{-1}\mathcal{B}'\mathcal{Q}\mathcal{A}
    =\begin{bmatrix}
            \mathcal{K}(0) \\ \mathcal{K}(1) \\ \cdots \\ \mathcal{K}(N-1)
        \end{bmatrix}
$$

we get

$$u^0(k+i)=-\mathcal{K}(i)x(k), \qquad i=0,1,...,N-1$$

Now, we'll use only $\mathcal{K}(0)$ to compute the control law at the current step

$$u_{MPC}(k)=-\mathcal{K}(0)x(k)$$

### Tracking reference signal and disturbances

Let's consider this system

$$
    x(k+1) = Ax(k) + Bu(k) + Md(k)\\
    y(k) = Cx(k) + d(k)
$$

and a cost function that penalizing the tracking error $e(k)=y^0(k)-y(k)$ instead the state

$$J(x(k),u(\cdot),k)=\sum\limits_{i=0}^{N-1}\left[ e'(k+i)Qe(k+i) + u'(k+i)Ru(k+i) \right] + e'(k+N)Se(k+N)$$

and define new extended matrixes of the system

$$
    Y^0(k)=\begin{bmatrix}
        y^0(k+1) \\ y^0(k+2) \\ \cdots \\ y^0(k+N)
    \end{bmatrix},\quad
    Y(k)=\begin{bmatrix}
        y(k+1) \\ y(k+2) \\ \cdots \\ y(k+N)
    \end{bmatrix},\quad
    D(k)=\begin{bmatrix}
        d(k) \\ y(k+1) \\ \cdots \\ d(k+N)
    \end{bmatrix} \\
    \quad\\
    \mathcal{A}_c=\begin{bmatrix}
        CA \\ CA^2 \\ \cdots \\ CA^N
    \end{bmatrix},\quad
    \mathcal{B}_c=\begin{bmatrix}
        CB & 0 & 0 & \cdots & 0 & 0\\
        CAB & CB & 0 & \cdots & 0 & 0 \\
        \cdots & \cdots & \cdots & \cdots & \cdots & \cdots \\
        CA^{N-2}B & CA^{N-3}B & CA^{N-4}B & \cdots & CB & 0 \\
        CA^{N-1}B & CA^{N-2}B & CA^{N-3}B & \cdots & CAB & CB \\
    \end{bmatrix} \\
    \quad\\
    \mathcal{M}_c=\begin{bmatrix}
        CM & 0 & 0 & \cdots & 0 & 0& 0 \\
        CAM & CM & 0 & \cdots & 0 & 0& 0 \\
        \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots \\
        CA^{N-2}M & CA^{N-3}M & CA^{N-4}M & \cdots & CM & I & 0 \\
        CA^{N-1}M & CA^{N-2}M & CA^{N-3}M & \cdots & CAM & CM & I \\
    \end{bmatrix}
$$

like we did before we can predict the output as

$$Y(k)=\mathcal{A}_cx(k)+\mathcal{B}_cU(k)+\mathcal{M}_cD(k)$$

and we find the optimal solution with the cost function

$$\bar{J}(x(k),U(k),k)=(Y^0(k)-Y(k))'\mathcal{Q}(Y^0(k)-Y(k))+U'(k)\mathcal{R}U(k)$$

with the solution

$$U^0(k)=(\mathcal{B}_c'\mathcal{Q}\mathcal{B}_c+\mathcal{R})^{-1}\mathcal{B}_c'\mathcal{Q}(Y^0(k)-\mathcal{A}_cx(k)-\mathcal{M}_cD(k))$$

where obviously the vector $D(k)$ is unknown, so is a common practice to set $d(k+i)=d(k)$. Similar consideration can be do for $y^0(k)$ and also for it is common impose $y^0(k+i)=y^0(k)$.

### Explicit integrator

Obviously the **MPC** doesn't impose an integral action on the system error, so if there are errors in the model or a noise acting on the system we cannot expect that the error asymptotically vanish. It's possible enlarge the system to impose an explicit integration action on the error; let's consider this enlarged system

$$
    x(k+1) = Ax(k) + Bu(k)\\
    v(k+1) = v(k) + e(k+1)
$$

where $e(k)=y^0-y(k)$, so

$$
    x(k+1) = Ax(k) + Bu(k)\\
    v(k+1) = v(k) + y^0 - CAx(k) - CBu(k)
$$

and if we impose a change of variable with $\delta x(k)=x(k)-x(k-1)$ and $\delta u(k)=u(k)-u(k-1)$ we get

$$
    \delta x(k+1) = A\delta x(k) + B\delta u(k)\\
    e(k+1) = -CA\delta x(k) + e(k) - CB\delta u(k)
$$

this is called **velocity form**.
For this system we can use the cost function

$$J(\delta x(k),\delta u(\cdot),k)=\sum\limits_{i=0}^{N-1}\left[ e'(k+i)Qe(k+i) + \delta u'(k+i)R\delta u(k+i) \right] + e'(k+N)Se(k+N)$$