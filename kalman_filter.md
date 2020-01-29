# Kalman filter (KF)

## Goal

Design an state observer for a linear system

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