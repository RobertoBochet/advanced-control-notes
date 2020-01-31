# Kalman filter (KF)

## Goal

Design a state observer for a linear system

$$
    \dot{x}(t) = Ax(t) + Bu(t) + \upsilon_x(t) \\
    y(t) = Cx(t) + \upsilon_y(t)
$$

where $\upsilon_x$ and $\upsilon_y$ are white gaussian noise, with

$$
    \upsilon(t) =
        \begin{bmatrix}
            \upsilon_x(t)\\ 
            \upsilon_y(t)
        \end{bmatrix} \quad
    E[\upsilon(t)] = 0 \quad E[\upsilon(t1)\upsilon(t2)] = V\delta(t1-t2) \quad
    V =
        \begin{bmatrix}
            \tilde{Q} & Z\\ 
            Z' & \tilde{R}
        \end{bmatrix} \quad
$$
$\delta(t)$ is the *kronecker index* and $\tilde{Q} \geq 0$, $\tilde{R}>0$

## Procedure

Let's start from the trivial observer

$$\dot{\hat{x}}(t) = A\hat{x}(t) + Bu(t) + L(t)\left[y(t)-C\hat{x}(t)\right]$$

we define the estimation error as $e(t)=x(t)-\hat{x}(t)$ and we compute its dynamic

$$\dot{e}(t)=\left[A-L(t)C\right]e(t)+\upsilon_x(t) - L(t)\upsilon_y(t)$$

if we apply the **expected value** operator on $e$ we can write

$$\bar{e}(t)=E\left[e(t)\right],\quad \dot{\bar{e}}(t)=\left[A-L(t)C\right]\bar{e}(t)$$

If it's true that the initial state is known

$$E\left[e(t)\right]=E\left[x(0)-\hat{x}(0)\right]=0 \quad\Rightarrow\quad \bar{e}(t)=0, \forall t\geq0 $$

### Steady state filter

We introduce $\tilde{Q}=B_qB'_q$ so

*If the **pair $(A,B_q)$ is reachable** and the **pair $(A,C)$ is observable**, then the steady state observer is **asymptotically stable** and it's defined as*

$$\dot{\hat{x}}(t) = \left(A-\bar{L}C\right)\hat{x}(t) + Bu(t) + \bar{L}y(t)$$

with

$$\bar{L}=\bar{P}C'\tilde{R}^{-1}$$

where $\bar{P}$ is the unique positive solution of

$$0=A\bar{P}+\bar{P}A'+\tilde{Q}-\bar{P}C'\tilde{R}^{-1}C\bar{P}$$