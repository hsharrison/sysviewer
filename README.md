sysViewer
=====

*Matlab Dynamical system graphical interface*

`h = sysViewer(oPar, cPar, fcn, typ)` builds a GUI for viewing the effects of parameter changes on a one- or two-dimensional dynamical system. 
  
`oPar` and `cPar` are two-column cell arrays that define the order parameter and control parameter(s), respectively. The first column contains parameter names (strings to be typeset in latex). The second column contains 1x2 vectors of parameter's range. This vector defines plot axes limits for the order parameter and slider limits for the control parameters. 
  
The system is defined by the function handle `fcn`. For one-dimensional systems, this should be a function with two inputs and one output. The first input is system state, which will be a scalar in a one-dimensional system. The second input is a vector of control parameter values. The output of `fcn` can be either the rate of change of the order parameter (in which case `fcn` is the flow equation), or the system  potential (in which case fcn is the potential function). Which function is supplied can be indicated by the last input typ, one of the strings  `'flow'` or `'potential'`. If `typ` is empty or omitted, `'flow'` is the default. 
  
For two-dimensional systems defined with a flow, `fcn` should be a cell array containing two function handles, each outputting the rate of change along one dimension. Each function should take three inputs; the first two are scalar values of the order parameters, the third is a vector of control parameters. For two-dimensional systems defined with a potential, `fcn` should take inputs as above and output a scalar. 
  
The output `h` is a struct containing handles to the graphics objects in the GUI. Intended only for advanced tweaking or troubleshooting. 
  
`sysViewer(..., defs)` initializes the parameter sliders with the default values in vector `defs`. 
  
`sysViewer(..., 'vectorize')` speeds up computation for functions that cannot be evaluated with a vector. In fact, any number of trailing arguments will be passed to `chebfun`. See `help chebfun` for more options (`chebfun2` and `chebfun2v` for two-dimensional options). 
  
Examples
------ 

*One-dimensional*

```matlab
sysViewer({'\phi' [-pi pi]}, ... 
          {'b/a' [0 1]; '\Delta\omega' [-2 2]}, ... 
          @(x,c) c(2)*x-cos(x)-c(1)*cos(2*x), ... 
          'potential'); 
  
sysViewer({'\phi' [-pi pi]}, ... 
          {'b/a' [0 1]; '\Delta\omega' [-2 2]}, ... 
          @(x,c) c(2)-sin(x)-2*c(1)*sin(2*x), ... 
          'flow'); 
  
sysViewer({'\phi' [-pi pi]}, ... 
          {'b/a' [0 1]; '\Delta\omega' [-2 2]; 'c' [0 .5]; 'd' [0 .5]}, ... 
          @(x,c) c(2)*x-cos(x)-c(1)*cos(2*x)+c(3)*sin(x)+c(4)*sin(2*x), ... 
          'potential'); 
  
sysViewer({'\phi' [-pi pi]}, ... 
          {'b/a' [0 1]; '\Delta\omega' [-2 2]; 'c' [0 .5]; 'd' [0 .5]}, ... 
            @(x,c) c(2)-sin(x)-2*c(1)*sin(2*x)+c(3)*cos(x)+2*c(4)*cos(2*x), ... 
          'flow'); 
```

*Two-dimensional*

```matlab
sysViewer('x' [-5 5]; '\dot{x}' [-5 5]}, ... 
          [{'\alpha'; '\beta'; '\delta'} repmat({[-5 5]}, 3, 1)], ... 
          {@(x,dx,r) dx, @(x,dx,r) -r(1).*x.^3 - r(2).*x - r(3).*dx}, ... 
          'flow') 
```

Dependencies
-----

*(if missing, user will be prompted to download and install)*

  * [GUI Layout Toolbox](http://www.mathworks.com/matlabcentral/fileexchange/27758-gui-layout-toolbox)
  * [sliderPanel](http://www.mathworks.com/matlabcentral/fileexchange/13845-sliderpanel)
  * [uibutton](http://www.mathworks.com/matlabcentral/fileexchange/10743-uibutton-gui-pushbuttons-with-better-labels)
  * [Chebfun](http://www2.maths.ox.ac.uk/chebfun/)
