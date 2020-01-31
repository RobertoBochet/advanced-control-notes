# Linear Quadratic Gaussian control (LQG)

## Goal

We want design a controller for a linear system affected by gaussian noises with the LQ control approach described by

$$
    \dot{x}(t) = Ax(t) + Bu(t) + \upsilon_x(t) \\
    y(t) = Cx(t) + \upsilon_y(t)
$$

The idea is designed a observer for the state of the system with the Kalman filter and use this to feed a controller designed with LQ control approach.