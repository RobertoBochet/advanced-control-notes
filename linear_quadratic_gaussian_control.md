# Linear Quadratic Gaussian control (LQG)

## Goal

We want design a controller for a linear system affected by gaussian noises with the LQ control approach described by

$$
    \dot{x}(t) = Ax(t) + Bu(t) + \upsilon_x(t) \\
    y(t) = Cx(t) + \upsilon_y(t)
$$

The idea is designed a observer for the state of the system with the Kalman filter and use this to feed a controller designed with LQ control approach.

## Procedure

### Index number

We assume that all the assumptions introduced for the Kalman filter are satisfied.

We want minimize

$$J=\lim\limits_{T\to\infty}\frac{1}{T}E\left[\int\limits_0^T x'(t)Qx(t)+u'(t)Ru(t)\;dt\right]$$

when the state x is nonmeasurable.

We must note that in this case also in a closed-loop $x$ (and as a result $u$) cannot vanish because of there are white noises on the system, therefore the integral in the index number cannot converge; it's introduced the term $\frac{1}{T}$ so that we can assume that $J$ is finite.

If we assume that LQG law guarantees asymptotic stability of the closed-loop system, then $x$ and $u$ are stationary stochastic processes, therefore we can write

$$J=E\left[x'(t)Qx(t)+u'(t)Ru(t)\right]$$

### Synthesis

Now, we assume that all the assumptions introduced for the LQ control are satisfied.
