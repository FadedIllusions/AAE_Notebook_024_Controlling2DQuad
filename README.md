# AAE_Notebook_024_Controlling2DQuad
So far, we've been working in the 1D case so as to keep the concepts and related math relatively easy. 

As we've seen, in 1D, we can easily figure out the net force of a vehicle:

![1D Net Force](/images/1D_Fnet.png)

But, what happens when we move into 2D? When the vehicle is allowed to pitch or roll, then we must contend with forces at angles (using Trigonometry)...

![2D Force Vectors](/images/2D_Force.png)

In looking at the following picture, we notice that we have a vehicle inclined at some angle phi in reference to the world frame. Having this inclination, we can decompose the vector F_thrust into verticle and horizonal components.

![decomposition](/images/trig_thrust.png)

Since this is a right angle, we can use trigonometric relationships to solve for the horizontal and verticle components of the thrust. In example, we know that the sine of phi is equal to the opposite side divided by the hypoteneus -- which, in this case, is the horizontal component of force divided by the total force. We can, then, solve this equation to find that the horizontal component of the thrust is just the total force times the sine of the angle phi.

***   ***   ***   ***   ***   ***   ***   ***   ***

In the first image, the thrusts from the two props have been combined into a single F_thrust. Let's assume the quantities shown to have the following values:

![quantity graph](/images/quatities.png)

Which equation would give us the total force in the horizontal-y direction, F_y?  (F_y = F_thrust(sin_phi))

What will be the resulting acceleration in the y-direction, y_dot_dot? (6 m/s^2)

![y-acceleration](/images/y_dot_dot.png)

***   ***   ***   ***   ***   ***   ***   ***   ***

With having approached 2D thrust, let us consider the vehicle's moments so as to describe the full 2-dimensional motion of the vehilce.

As we know, a clockwise spinning rotor will introduce a counter-clockwise moment/torque about the axis of rotation. This is known as a reaction moment because it's a rotational moment that is an opposite reaction to some other relational motion. But, reaction moments only explain yaw. What about pitch and roll?

When experiencing a positive roll about the x-axis, the left motor of the quadrotor produces a larger force vector than that of the right motor, causing the clockwise rotational force created by the perpendicular force to be greater than that of the adjacent counter-clockwise rotational force.

![Perpendicular Rotation](/images/perpendicular_rotation.png)

When calculating perpendicular force, we multiply the force by the length of the lever arm:

![Perpendicular Force](/images/perpendicular_force.png)

In the case of our quadrotor, we'll assume that the body frame is align along the arms of the quad, as pictured...

![Body Frame Alignment](/images/aligned_body_frame.png)

If we wanted to figure out the size of the moment about the x-axis, M_x, caused by the right-most propeller, we'd use the following equations:
     M_x = F x d_perp,x
     M_x = F x L

Here, the perpendicular distance to the x-axis, d_perp,x, is exactly equal to the length of the propeller arm; but, that's not always the case. Conside the following picture, in which the body-frame coordinate system is rotated with respect to the arms:

![Rotate Body Frame](/images/rotated_body_frame.png)

As the picture shows, the perpendicular distance from the propeller to the x-axis is less than the length of the propeller arm. Noting the right triangle formed with the body frame by this configuration, what is the correct value of d_perp,x in terms of the arm length L and an angle theta?
d_perp,x = L(cos_theta); thus, M_x = F x L(cos_theta).

The coordinate system that we actually use for a quadrotor body frame has the x and y axes pointing halfway between the rotor arms. Thus, each arm is always 45 degrees relative to each axis. Since cos45 = 1/sqrt(2)... M_x = F(L) / sqrt(2). Sometimes, we will also use l = L/sqrt(2) to represent the perpendicular distance.

***   ***   ***   ***   ***   ***   ***   ***   ***

![Moments and Thrusts](/images/rot_rates_to_moments_and_thrusts.png)

Thusfar, we have an intuitive understanding of how the rotation rates of four propellers can lead to four quantities that we're interested in: the total upward thrust, F_thrust (translational); the total moment about the x-axis, M_x, which causes the vehicle to roll; the total moment about the y-axis, M_y, which causes the vehicle to pitch; and, the total reaction moment about the z-axis, M_z, which causes the vehicle to yaw. However, when working in only 2D, we need only worry about the first two.

```
def get_thrust_and_moment(self):
  """Helper Function Which Calculates And Returns The
  Collective Thrust And The Moment About The x-axis"""

  f1 = self.k_f * self.omega_1**2
  f2 = self.k_f * self.omega_2**2

  # c Is Ofter Used To Indicate "Collective" Thrust
  c = f1 + f2

  M_x = (f1 - f2) * self.l

  return c, M_x
```
