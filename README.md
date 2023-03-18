# Finite differences for the wave equation
[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=mathworks/Simple-Wave-Equation-solver)

Implementation of a simple numerical schemes for the wave equation.



<img src="https://latex.codecogs.com/gif.latex?\frac{\partial^2&space;u}{\partial&space;t^2&space;}=c^2&space;\frac{\partial^2&space;u}{\partial&space;x^2&space;}."/>



Applying the second-order centered differences to approximate the derivatives,



<img src="https://latex.codecogs.com/gif.latex?\frac{u_j^{n+1}&space;-2u_j^n&space;+u_j^{n-1}&space;}{\Delta&space;t^2&space;}=c^2&space;\frac{u_{j+1}^n&space;-2u_j^n&space;+u_{j-1}^n&space;}{\Delta&space;x^2&space;}."/>



It can be arrange to obtain



<img src="https://latex.codecogs.com/gif.latex?u_j^{n+1}&space;=s(u_{j+1}^n&space;+u_{j-1}^n&space;)+2(1-s)u_j^n&space;-u_j^{n-1}&space;,"/>



where



<img src="https://latex.codecogs.com/gif.latex?s=c^2&space;\frac{\Delta&space;t^2&space;}{\Delta&space;x^2&space;}."/>



It is known that the scheme needs to satisfy  <img src="https://latex.codecogs.com/gif.latex?\inline&space;s\le&space;1"/> for stability. Copyright 2015-2016 The MathWorks, Inc.




![image_0.png](SimpleWaveEquation_images/image_0.png)


# Proboem Setup
```matlab
N = 101;
L = 4*pi;
x = linspace(0,L,N);

% It has three data set; 1: past, 2: current, 3: future.
u = zeros(N,3);
s = 0.5;
```
# Initial Condition
```matlab
% Gaussian Pulse
y = 2*exp(-(x-L/2).^2);
u(:,1) = y;
u(:,2) = y;

% Plot the initial condition.
handle_line = plot(x,u(:,2),'LineWidth',2);
axis([0,L,-2,2]);
xlabel('x'); ylabel('u');
title('Wave equation');
```

![figure_0.png](SimpleWaveEquation_images/figure_0.png)

# Boundary condition
```matlab
% Dirichet Boundary conditions
u(1,:) = 0;
u(end,:) = 0;
```
# Simulation
```matlab
filename = 'wave.gif';
for ii=1:100
    disp(['at ii= ', num2str(ii)]);
    u(2:end-1,3) = s*(u(3:end,2)+u(1:end-2,2)) ...
        + 2*(1-s)*u(2:end-1,2) ...
        - u(2:end-1,1);
    u(:,1) = u(:,2);
    u(:,2) = u(:,3);
    handle_line.YData = u(:,2);
    drawnow;
    frame = getframe(gcf);
    im = frame2im(frame);
    [A,map] = rgb2ind(im,256);
    if ii==1
           imwrite(A,map,filename,'gif','LoopCount',Inf,'DelayTime',0.05);
    else
        imwrite(A,map,filename,'gif','WriteMode','append','DelayTime',0.05);
    end
end
```

![figure_1.png](SimpleWaveEquation_images/figure_1.png)

Copyright 2015-2016 The MathWorks, Inc.
[![View Simple Wave Equation solver on File Exchange](https://www.mathworks.com/matlabcentral/images/matlab-file-exchange.svg)](https://jp.mathworks.com/matlabcentral/fileexchange/59915-simple-wave-equation-solver)
