---
permalink: /projects/fftocean/
author_profile: true
layout: splash
---

# Simulating Ocean water
<video width="640" height="640" controls>
  <source src="/assets/files/fftocean.mp4" type="video/mp4">
</video>


### Model of waves
In the FFT-based model, the ocean's height at some position $x$ is viewed as the sum of sin & cos wave functions with some amplitudes and different wave length $k$ here :

$$h(x,t) = \sum_{k}\tilde h(k,t)exp(ik \cdot x)$$

where `t` is the time and $k = (2 \pi n / L_x, 2 \pi m / L_y), x = (nL_x/N, mL_z/M)$. $M,N$ are the resolution of the grid, $L_x, L_y$ are the physical size (patch size) of the ocean. 

The key idea in this model is that statistical analysis showed that the wave amplitude $\tilde h(k, t)$ have a close releationship with some spatial specturms:

$$P_h(k) = \big \langle |\tilde h^*(k, t)|^2  \big \rangle$$



A common used spectrum is called the Phillips spectrum:

$$P_h(k) = A\frac{exp(-k^2L^2)}{k^4}|Normalize(k) \cdot \omega| ,$$

where $L = V^2 / g$, the largest possible waves generate by a wind with speed $V$. And $g$ is the gravitational constant, $w$ is the direction of the wind.

We can also calculate the normal using the height function, and it will also be in a form can be calculated using FFT.

### Simulation
The pipeline for the simulation is:
1. Generate random variables for the wave amplitudes
2. Use the random variables to calculate the specturm for each wave lengths
3. For each frame, we use the specturm to get the wave amplitudes
4. Use fast Fourier transform to calculate the height of the ocean
5. Put the height into a displacement texture and read it in the vertex shader

And the way to calculate the noramls and displacement in x & y direction is similar to the calculation of height.


## Rendering
There are four parts in the rendering:
1. Base color
3. Reflection
4. Scattering
5. Foam
### Base color
The base color is for water without any special effects. It's is for an approximation of deep water's color.
### Reflection
For the reflection of the ocean, I used a reflection map to include the reflection of the sky and a specular hight the same as the normal Phong shading.
### Scattering
The scattering effect is the most important part for water rendering. In order to make the ocean looks good, we need to approximate the scattering effect.

According to some talks in GDC & siggraph, here are two property of scattering:
- If you view directly to the waves, i.e. the angle of view direction and the normal is small, there will be more scattering.
- The thiner the wave, the more the lights will scatter.

So my rendering of water is a composition of the Jacobian, height and view angle.

# Other
I also used a sphere with an hdr texture I found online for the reflection mapping and background color.

# Reference 
Introduction to algorithms, 3rd edition

[Simulating ocean water](http://evasion.imag.fr/~Fabrice.Neyret/images/fluids-nuages/waves/Jonathan/articlesCG/waterslides2001.pdf)

[Nvidia's talk about FFT ocean](developer.download.nvidia.com/assets/gamedev/files/sdk/11/OceanCS_Slides.pdf)

[The Technical Art of Sea of Thieves](https://dl.acm.org/doi/10.1145/3214745.3214820)

[Water Technology of Uncharted](https://www.gdcvault.com/play/1015309/Water-Technology-of)

[Water rendering in Crest](http://advances.realtimerendering.com/s2019/index.htm)

[Summary of realistic water rendering and simulation](https://zhuanlan.zhihu.com/p/95917609)


