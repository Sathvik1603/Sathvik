Cart-Pole Optimal Control Assignment
Introduction
I analyze and tune a Linear Quadratic Regulator (LQR) controller for a cart-pole system exposed to earthquake-simulation disturbances in this research. The major objective is to keep the cart inside the physical track constraints (±2.5 m) while maintaining the inverted pendulum in its upright posture. We propose quantitative criteria for assessing cart-pole stability and examine the effects of various LQR tuning settings on performance under strong, continuous disturbance forces.
System Description
Physical Setup:
Inverted pendulum mounted on a cart
Cart traversal range: ±2.5m (total range: 5m)
Pole length: 1m
Cart mass: 1.0 kg
Pole mass: 1.0 kg
Disturbance Generator:
The system includes an earthquake force generator that introduces external disturbances:
Generates continuous, earthquake-like forces using the superposition of sine waves
Base amplitude: 15.0N (default setting)
Frequency range: 0.5-4.0 Hz (default setting)
Random variations in amplitude and phase
Additional Gaussian noise
Tuning Objective:  
•	Cut Down on Cart Travel: prevent too much lateral movement to prevent running into ±2.5 m.
•	Preserve or enhance pole stability by keeping θ and θ modest in spite of significant disruptions.
•	Management of Control Effort: Steer clear of force orders that are too big or loud.
LQR Controller Tuning:
Existing tuning parameters:
# LQR cost matrices
•	self.Q = np.diag ([1.0, 1.0, 10.0, 10.0])  # State cost
•	self.R = np.array ([[0.1]])  # Control cost

After systematic tuning, the final LQR tuning parameters are:

# LQR cost matrices

•	self.Q = np.diag([1.0, 3.0, 1.0, 3.0])  # State cost
•	self.R = np.array([[0.1]])  # Control cost

Analysis:
•	Pole angle and cart position are given equal weight.
•	Higher weights (3.0) are assigned to velocity terms, which emphasize rapid stabilization and seamless transitions.
•	To permit moderate force consumption while preventing excessive actuator effort, the control effort penalty is kept at 0.1.
•	Simulation duration: 80s and recovery time is 2.5s
Observations:
•	Improved Pole Stability: The maximum variation of the pole angle dropped from about 7.5 to 5.2.
•	Decreased Cart Travel: To avoid track-limit infractions, the RMS cart position was lowered from 0.48 m to 0.38 m.
•	Improved Recovery: The adjusted controller bounces back from disruptions around 30% quicker.
•	Control Effort: The reaction was smooth and free of excessive oscillations, despite a modest rise in the peak control force.
Results:
 
                                              Cart-Pole Optimal Control



Conclusion:
The final LQR parameters successfully decreased pole angle deviations, minimized cart displacement, and accelerated recovery periods, all of which greatly increased stability during seismic disturbances. Increasing the velocity weights allowed the controller to respond more smoothly and be more resilient to outside disturbances. This modification preserved system restrictions while enabling quicker stabilization. Furthermore, the control effort was kept within reasonable bounds to prevent unnecessarily high force corrections that can cause oscillations or instability. The total tuning ensured dependable performance across a range of seismic circumstances by striking a compromise between quick disturbance rejection and economical energy use.
