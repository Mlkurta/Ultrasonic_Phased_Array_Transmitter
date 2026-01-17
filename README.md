# Ultrasonic Phased Array Transmitter
This is a project receives audio via 3.5mm jack and transmits modulated ultrasound at 40 khz.
Power can be supplied from ~9V - 18V DC, I've found the best results and cleanest sound at around 12 volts, or roughly 3 3.7V lithium ion batteries.
The sound radiates directionally like a flashlight, and a joystick controls the direction in 5 degree increments from 
30 degrees left to 30 degrees right.  Pressing the BTN 0 button on the CMOD will focus the sound at a point 1 meter away.

I initially found the idea from Mean Gene Hacks' implementation of a parametric speaker:
https://www.youtube.com/watch?v=9hD5FPVSsV0&t=635s&ab_channel=MeanGeneHacks

I also wanted to learn about FPGAs; so I wondered if I could individually control rows of tranducers to "beamform" the sound. It works quite well, though 
admittedly the sound lacks the bass response of a decent speaker.

I used a CMOD A7 35T FPGA board, though many other FPGA boards with >1800 Flip flops, >1800 LUTs and 2 or more ADC inputs should
be capable of running it.  The project is current for Vivado 2021.2.

CAUTION! There's a false sense of safety with audible modulated ultrasound. From reading a few scientific papers on the subject, the best rule of thumb I could
extract for 40 khz is to assume you're being exposed to levels 60 dB higher than what it actually sounds like. This is another reason why I like 11-12VDC.
![Front_Image](https://user-images.githubusercontent.com/78199728/176317332-d1329611-d8d8-4863-ae41-bb6e8244707a.jpeg)<img src="176317332-d1329611-d8d8-4863-ae41-bb6e8244707a.jpg" alt="Description" width="300" height="200">
![Back_Image](https://user-images.githubusercontent.com/78199728/176317351-4105629f-5049-4989-8cf0-8f61a7647e7e.jpeg)

This is basically a project of novelty and not much use as a high-gain, directional transmitter. Because of the Shannon-Nyquist theorem:

For a plane arriving at angle $\theta$, the phase progression between adjacent elements spaced by distance d is: $\Delta\phi = kd\sin (\theta) = \frac{2\pi}{\lambda}$

The array cannot distinguish two different angles is they produce the same phase progression at a reapeat (modulo) of $2\pi$.  This ambiguity occurs when:  
$kd(\sin(\theta_1) - \sin(\theta_2)) = 2\pi m$  ....... for any integer $m \neq 0$.

Solving for element spacing: 
$d = \frac{m\lambda}{\sin(\theta_1) - \sin(\theta_2)}$

To prevent any ambiguity over all physically possible angles $(|\sin(\theta)| \leq 1)$, the spacing must satisfy:

$d = \frac{\lambda}{2}$

Given that the formula for wavelength:  $\lambda = \frac{v}{f}$,  where v is the wave velocity (sound) and f is the frequency, the wavelength of 40 KHz sound is:

$\lambda = \frac{343 m/s}{40 KHz} =$ 8.575 m. And taking into consideration the spacing between elements is 16mm, our $\frac{d}{\lambda} = $ 1.87.  In this case, at 0 degrees steering angle, rather than us getting a nice plot like this:

<img width="392" height="587" alt="Screenshot 2026-01-17 143053" src="https://github.com/user-attachments/assets/033ffd38-ef1c-49e6-9d8f-4b7fc28fa5fb" />

Our actual plot should be similar to this:

<img width="396" height="592" alt="Screenshot 2026-01-17 135426" src="https://github.com/user-attachments/assets/8a9e41a8-81b1-40f7-844a-889ec6b612dd" />




