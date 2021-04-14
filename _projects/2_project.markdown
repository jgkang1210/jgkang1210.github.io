---
layout: page
title: Robotics using matlab
description: robotics using matlab
img: /assets/img/proj2_prof.jpg
importance: 1
category: work
---

[git repo](https://github.com/jgkang1210/ME439)

## Robotics using matlab library

Based on : 
[http://hades.mech.northwestern.edu/index.php/Modern_Robotics](http://hades.mech.northwestern.edu/index.php/Modern_Robotics)


Matlab library를 활용하여 inverse kinematics, forward kinematics 등등 구현.


## 기능
1. coppelia sim scene for controlling the Indy 7 manipulator
2. MATLAB assignments for solving problems

### 2R open chain manipulator sim
Using Matlab

Can calculate the 2R open chane robot's configuration with manipulibility elipsoid and 1st, 2nd, 3rd measure of the manipulability.

ex) when L1 = 1, L2 = 1, theta 1 = -10, theta 2 = 10
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-8 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/-1020.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>

ex) when L1 = 1, L2 = 1, theta 1 = 60, theta 2 = 60
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-8 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/6060.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>

ex) when L1 = 1, L2 = 1, theta 1 = 135, theta 2 = 90
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-8 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/13590.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>

ex) when L1 = 1, L2 = 1, theta 1 = 190, theta 2 = 160
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-8 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/190160.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>

ex) complex case
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-8 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/complex.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>