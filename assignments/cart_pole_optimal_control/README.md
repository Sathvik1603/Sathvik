# Cart-Pole Optimal Control

## Name - Sathvik Merugu
## ASU ID - 1235373752

## Introduction
This project involves analyzing and tuning a **Linear Quadratic Regulator (LQR)** controller for a cart-pole system subjected to **earthquake-simulation disturbances**. The primary goal is to keep the cart within the **track constraints (±2.5 m)** while ensuring the pendulum remains in its **upright position**.

## System Description

### Physical Setup
- Inverted pendulum mounted on a cart
- Cart traversal range: **±2.5m** (total range: **5m**)
- Pole length: **1m**
- Cart mass: **1.0 kg**
- Pole mass: **1.0 kg**

### Disturbance Generator
- Generates **continuous, earthquake-like forces** using the superposition of sine waves
- **Base amplitude:** 15.0N (default)
- **Frequency range:** 0.5 - 4.0 Hz (default)
- Includes **random variations** in amplitude and phase
- Adds **Gaussian noise**

## Objectives
- **Minimize cart travel** to prevent exceeding the track limits (**±2.5m**)
- **Improve pole stability** by reducing angular deviations under disturbances
- **Optimize control effort** to avoid excessive force commands

## LQR Controller
### What is LQR?
A **Linear Quadratic Regulator (LQR)** is an optimal state-feedback control method used to stabilize linear dynamic systems while minimizing a specific cost function. It is widely used in control engineering for applications requiring stability, efficiency, and robustness.

LQR is particularly useful for systems where:
- The dynamics can be modeled using **state-space equations**.
- The objective is to balance **performance and control effort optimally**.
- A **quadratic cost function** is appropriate for defining system goals.

### Mathematical Formulation
The system dynamics are represented in a **state-space model**:

```math
\dot{x} = Ax + Bu
```

where:
- \( x \) is the **state vector** (e.g., position, velocity, angle).
- \( u \) is the **control input**.
- \( A \) is the **system dynamics matrix**.
- \( B \) is the **control input matrix**.

The objective of the LQR controller is to **minimize the cost function**:

```math
J = \int_0^\infty (x^T Q x + u^T R u) dt
```

where:
- \( Q \) is the **state cost matrix** (penalizes deviations from desired states).
- \( R \) is the **control cost matrix** (penalizes excessive control effort).

LQR computes an **optimal feedback control law**:

```math
u = -Kx
```

where **\( K \) is the optimal gain matrix**, calculated as:

```math
K = R^{-1} B^T P
```

where **\( P \)** is obtained by solving the **Continuous Algebraic Riccati Equation (CARE)**:

```math
A^T P + PA - PBR^{-1} B^T P + Q = 0
```

### Existing LQR Parameters
Before tuning, the system used the following LQR cost matrices:
```python
Q = np.diag([1.0, 1.0, 10.0, 10.0])  # State cost
R = np.array([[0.1]])  # Control cost
```

### Tuned LQR Parameters
After systematic tuning, the parameters were adjusted to:
```python
Q = np.diag([1.0, 3.0, 1.0, 3.0])  # State cost
R = np.array([[0.1]])  # Control cost
```

### Tuning Adjustments
- **Increased velocity weights (3.0):** Emphasizes rapid stabilization and smooth transitions.
- **Cart position and pole angle given equal weight**
- **Control effort penalty remains at 0.1** to allow moderate force usage while preventing excessive actuator effort.

## Performance Analysis
- **Improved Pole Stability:** Angle deviation reduced from **7.5° to 5.2°**
- **Reduced Cart Travel:** RMS cart position decreased from **0.48m to 0.38m**
- **Faster Recovery:** 30% quicker recovery from disturbances
- **Smooth Control Effort:** No excessive oscillations despite a slight increase in peak control force

## Simulation Details
- **Duration:** 120s
- **Recovery time:** 3-5s

## Result
![IMG_0798](https://github.com/user-attachments/assets/b5f3c14d-d066-459d-aab9-3eaff876734c)



https://github.com/user-attachments/assets/a15cdb8c-6a6f-4a21-a2d4-339e06fa1207



![Screenshot (82)](https://github.com/user-attachments/assets/747649af-347f-4c7f-aba8-3b7d20686792)

![Screenshot (83)](https://github.com/user-attachments/assets/86fd9b63-7515-416c-90da-043c992c6dc5)




## Conclusion
The final LQR tuning **significantly improved stability** under seismic disturbances by:
- Reducing pole angle deviations
- Minimizing cart displacement
- Speeding up recovery times
- Keeping control effort within acceptable limits

This tuning achieves a balance between **fast disturbance rejection and efficient energy usage**, ensuring **robust performance** across varying seismic conditions.





