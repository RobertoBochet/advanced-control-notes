# State observer

## Goal

We want create an an observer for the state of a linear system

## Discrete time

Let's consider the system

$$
    x(k+1) = Ax(k) + Bu(k)\\
    y(k) = Cx(k)
$$

### State predictor

We begin from the **state predictor**, we'll write $\hat{x}(k+1|k)$ to mean the prediction at the step $k$ of the state at the step $k+1$ 

$$
    \hat{x}(k+1|k)=A\hat{x}(k|k-1)+Bu(k) + L\left[y(k)-\hat{y}(k|k)\right]
$$

where $A\hat{x}(k|k-1)+Bu(k)$ is the obvious evolution of the state and $L\left[y(k)-\hat{y}(k|k)\right]$ is a feedback corrective factor to vanish the observer's error. The goal is to choose $L$ to asymptotically vanish the observer's error.

If we define the observer's error as $\hat{e}(k|k-1)=x(k)-\hat{x}(k|k-1)$ tanking into account the dynamic of the system and the observer we can write

$$\hat{e}(k+1|k)=(A-LC)\hat{e}(k|k-1)$$

for this reason we have to choose $L$ so that the eigenvalues of $(A-LC)$ are all with negative real part.

### State filter

If we need $\hat{x}(k|k)$ we are talking about **state filter** and similar considerations of **state predictor** can be done.

$$
    \hat{x}(k+1|k+1)=A\hat{x}(k|k)+Bu(k) + L\left[y(k+1)-\hat{y}(k+1|k+1)\right]
$$

Obviously without $\hat{x}(k+1|k+1)$ we cannot have $\hat{y}(k+1|k+1)$ so for replace it we can use it's best estimator $C(A\hat{x}(k|k)+Bu(k))$ so, we'll write

$$
    \hat{x}(k+1|k+1)=A\hat{x}(k|k)+Bu(k) + L\left[y(k+1)-C(A\hat{x}(k|k)+Bu(k))\right]
$$

again, if we define the observer's error as $\hat{e}(k|k)=x(k)-\hat{x}(k|k-1)$ we get

$$\hat{e}(k+1|k+1)=(A-LCA)\hat{e}(k|k)$$

### Estimate constant disturbance

If the system is affected by a constant disturbance 

$$
    x(k+1) = Ax(k) + Bu(k) + Md\\
    y(k) = Cx(k) + Nd
$$

we can enlarge the system's state to consider the disturbance as a constant state $d(k+1)=d(k)$

$$
    \begin{bmatrix}
        x(k+1) \\ 
        d(k+1) 
    \end{bmatrix} = 
    \begin{bmatrix}
        A & M \\ 
        0 & I 
    \end{bmatrix}
    \begin{bmatrix}
        x(k) \\ 
        d(k) 
    \end{bmatrix} +
    \begin{bmatrix}
        B \\ 
        0 
    \end{bmatrix}u(k)
$$

Now, we can estimate the disturbance with an state observer.