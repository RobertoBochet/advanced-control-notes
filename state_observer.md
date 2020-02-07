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
    \hat{x}(k+1|k)=A\hat{x}(k|k-1)+Bu(k) + L\left[y(k)-C\hat{x}(k|k-1)\right]
$$

where $A\hat{x}(k|k-1)+Bu(k)$ is the obvious evolution of the state and $L\left[y(k)-C\hat{x}(k|k-1)\right]$ is a feedback corrective factor to vanish the observer's error. The goal is to choose $L$ to asymptotically vanish the observer's error.

If we define the observer's error as $\hat{e}(k|k-1)=x(k)-\hat{x}(k|k-1)$ tanking into account the dynamic of the system and the observer we can write

$$\hat{e}(k+1|k)=(A-LC)\hat{e}(k|k-1)$$

for this reason we have to choose $L$ so that the eigenvalues of $(A-LC)$ are all with negative real part.

#### Stability of state predictor

If the pair $(A,C)$ is observable it is possible to choose the gain matrix $L$ arbitrarily, then choose the position of the observer's eigenvalues.

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

#### Stability of state filter

If the pair $(A,CA)$ is observable it is possible to choose the gain matrix $L$ arbitrarily, then choose the position of the observer's eigenvalues.

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

### Reduced order observer

We can get an observer of order $n-p$ where $n$ is the number of state of the system and $p$ is the number of the output. We must create a new system that has $n-p$ state and the remaining $p$ states are expressing as the outputs of the original system. The new system has the this form

$$
    \tilde{x}(k+1)=\tilde{A}\tilde{x}(k)+\tilde{B}\tilde{u}(k) \\\quad\\
    y(k+1)=\tilde{C}\tilde{x}(k)+\tilde{D}\tilde{u}(k)
$$

where

$$
    \tilde{u}(k)=\begin{bmatrix}
        u(k) \\ 
        y(k) 
    \end{bmatrix}, \quad
    Tx
    =\begin{bmatrix}
        C \\ 
        T_1 
    \end{bmatrix}x
    =\begin{bmatrix}
        y \\ 
        \tilde{x} 
    \end{bmatrix}
$$

with $\det(T)\ne0$.

For this new system we can use a state observer

$$
    \hat{\tilde{x}}(k+1|k+1)
    =\tilde{A}\hat{\tilde{x}}(k|k) + \tilde{B}\tilde{u}(k) + L\left[y(k+1)-\tilde{C}\tilde{x}(k|k)-\tilde{D}\tilde{u}(k)\right]\\
    =(\tilde{A}-L\tilde{C})\hat{\tilde{x}}(k|k) + \tilde{B}\tilde{u}(k) + L\left[y(k+1)-\tilde{D}\tilde{u}(k)\right]
$$

and convert the observer to the real system  

$$
    \hat{x}(k|k)=T^{-1}
    \begin{bmatrix}
        y(k) \\ 
        \hat{\tilde{x}}(k|k) 
    \end{bmatrix}
$$

#### Stability of reduced order observer

If the pair $(A,C)$ is observable it is possible to choose the gain matrix $L$ arbitrarily, then choose the position of the observer's eigenvalues.