# Writeup for the PID Controller Project
My 9th Project in the Self-Driving Car Engineer Nanodegree at Udacity

---

### Student describes the effect of the P, I, D component of the PID algorithm in their implementation. Is it what you expected?

**PID** stands for **P**roportional **I**ntegral **D**erivative.

**P**: The proportional term uses the error between the car and the middle of the road. In other words, the Cross Track Error (CET). The proportional term lets the car steer towards the center of the track. The larger the CET, the larger is the car’s steering angle. Using the proportional term makes the car oscillate left and right off the road’s center. The higher the coefficient of the proportional term, the faster the car oscillates. This behavior could also be seen in the simulator. The car drove smooth using a small coefficient and oscillated with a higher amplitude the larger the coefficient was set. On straight roads, a small value works fine. But when the car reaches a curve it might not steer enough to stay in the lane. This means, there is a tradeoff between driving smooth and staying on the road within a curve.

**I**: The integral term is used to compensate systematic bias. If the wheels are not exactly aligned or the wind pushes against the car, the car tends to move to one side. As soon as the coefficient does not equal zero, the car tends to drift to one side.

**D**: The derivative term helps the car to drive to the middle of the road smoothly by slowing down the convergence. It uses the difference between the current CET and the former CET to lower the steering angle while the car approaches the lane’s center. The higher the coefficient, the more the convergence is slowed down. In curves, a too high coefficient even leads the car back towards the ledge. I experienced this behavior during the project, when the car needed to take a sharp left turn. After the car entered the sharp curve and was too far on the right side of the lane, the proportional term let the car steer a lot to the left. Steering hard, the car approached the middle of the road quickly and the difference between the current CET and the former CET became so high that the derivative term pushed the car back to the ledge of the road.


### Student discusses how he chose the final hyperparameters (P, I, D coefficients). This could be done through manual tuning, twiddle, SGD, or something else, or a combination!

The first step I did was that I limited the steer value to a value between -1 and 1. Then I implemented a twiddle algorithm which could tune one of the three coefficients. I set all three coefficients to zero and tried to tune the P-coefficient first, the D-coefficient second and the I-coefficient third. At the beginning, the car left the road after some meters while lowering and raising the coefficient. The reason was that the step for lowering and raising the coefficient was too high. I lowered the step and deleted the part of the twiddle algorithm where the step was increased. This led to an algorithm which lowered the steps for the coefficient tuning after time. The P-coefficient got raised very slow while the D-coefficient increased faster. When it came to tuning the I-coefficient, the value oscillated around zero. That is why I decided to set the I-coefficient constantly to zero. The simulator does not simulate wind or other forces which influence the car’s driving behavior and the wheels are perfectly aligned. The twiddle-like algorithm produces some rough values but the fine tuning needed to be done manually. As soon as the car stayed on the road all of the time, I tried to increase the speed of the car. I decided to lower the speed of the car as soon as the car starts steering. I wanted the car to slow down exponentially to the steering angle in order to smoothly drive through sharp curves. So I implemented the formula (0.9-|steer_value|)^5 to calculate the car’s target speed. It turned out this formula lets the car drive with a speed between 34 and 48 miles per hour. After that, I tried to push the two coefficients low enough to let the car drive smoothly but needed to keep them high enough to stay on the road during curves. After manual tuning, I assigned the following values:

P-coefficient: 0.11

I-coefficient: 0.0

D-coefficient: 1.0

The car stays permanently on the road and drives kind of smooth most of the time. The result can be watched in this [video](https://drive.google.com/open?id=0B0agIiDyIPj1VC1DN1R1OUhtM28).
