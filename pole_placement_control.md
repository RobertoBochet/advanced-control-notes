# Pole placement control

## Goal

We want to impose the poles of a system with a linear control law respect with state.

## Procedure

Let's introduce linear system

$$
    \dot{x}(t) = Ax(t) + Bu(t) \\
    y(k) = Cx(t)
$$

and we impose the control law

$$u(t)=-Kx(t)+v(t)$$

so, the closed-loop system became

$$\dot{x}(t) = (A-BK)x(t) + Bv(t)$$

$K$ can be selected to choose the eigenvalues of the matrix $(A-BK)$ therefore stability of the inner closed-loop system. The input $v(t)$ can be used to close an outer control loop to meet static or dynamic specifications.

### Stability

If the pair $(A,B)$ is reachable, then $K$ can be arbitrarily chosen.