## 16-05-2026 - Phase 0 - Session 5

What I did:

- Watched video on essence of calculus

What clicked:

- $\frac{dA}{dx} \approx f(x)$, here
  - $f(x)$ is a given function
  - $A$ is the area under the graph of $f(x)$ or `Integral` of $f(x)$
  - $dx$ is the small change in the value of $x$
  - $dA$ change in area
  - The ratio between $dA$ & $dx$ is the `Derivatives` of $A$ or derivative is whatever this ratio approaches when $dx$ becomes smaller and smaller $dx \rightarrow 0$. Or loosely speaking, it is a measure of how sensitive a function $A(x)$ (area function) is to the change in its input $x$.
- Fundamental Theorem of Calculus
  - To compute the area under a function $f$ from $x = a$ to $x = b$, find any function $g$ whose derivative is $f$ (i.e. $g'(x) = f(x)$). The area is then $g(b) - g(a)$.
  - Going from $f \rightarrow g$: you **integrate**
  - Going from $g \rightarrow f$: you **differentiate**
- In a graph of distance($s$) vs time($t$), where $ds$ is the small change in the distance over a small time $dt$ passed, then the ratio $\frac{ds}{dt}$ is the derivative of **instantaneous velocity** i.e. the slope of tangent line to the curve at that point.
  - In terms of FTC, $f(t)$ is the velocity and $g(t)$ is the distance.
- Power rule $$\frac{dx^n}{dx} = nx^{n-1}$$
- Sum rule $$\frac{d}{dx}(g(x)+h(x))=\frac{dg}{dx}+\frac{dh}{dx}$$
- Product rule $$f(x)=g(x)h(x), \frac{df}{dx}=\frac{dg}{dx} \cdot h(x) + \frac{dh}{dx} \cdot g(x)$$
- Function composition rule $$\frac{d}{dx}g(h(x))=\frac{dg}{dh} \cdot \frac{dh}{dx} = \frac{dg}{dx}$$
- `Implicit differentiation` is a technique for finding $dy/dx$ when assuming $y$ is an implicit function of $x$, i.e. defined by an equation relating $x$ and $y$, without first solving the equation for y.

What is still fuzzy / questions to chase:

- Solve $\frac{df(x)}{dx} \text{ for, } f(x)=\frac{1}{x} \text{ and } f(x)=\sqrt{x}$

Time spent: ~4hrs

## 24-05-2026 - Phase 0 - Session 6

What I did:

- Read Khan academy articles for Multi-valued functions, partial & high order derivatives

What clicked:

- A function with multiple input and/or multiple output numbers is a `multi-variable` function, e.g. temperature of a location on map.
- With multi-variable calculus we can calculate rate of change of temperature if we move in a direction on map (**derivative**) or we can calculate average temperate of a location by using temperature from infinite points of time for that location (**integral**).
- With the help of multi-variable calculus we can analyze the behavior of a function.
- A function with output as a vector is called `vector-valued` function, and a function with single value output is a `scalar-valued` function.
- `Partial derivative` is calculated of a function having multiple variables by keep all other variables constant except one for which we want to calculate derivate. E.g. $$f(x,y)=x^2y, \frac{\partial f}{\partial x}=2xy, \frac{\partial f}{\partial y}=x^2$$
- We can also solve implicit differentiation with help of partial derivative using this formula, if $f(x,y)=0$ then $$\frac{dy}{dx}=-\frac{F_x}{F_y} \text{, where } F_x=\frac{\partial f}{\partial x}, F_y=\frac{\partial f}{\partial y}$$
- `Second partial derivative` is the derivative done one more time of already existing partial derivative, e.g. $F_{xx}$, $F_{xy}$, $F_{yx}$ or $F_{yy}$. Here $F_{xy}$ represents derivate of a function $f(x,y)$, when first derivative of $f(x,y)$ is done for $x$ & next derivative of first is done for $y$. `High order derivative` is the next derivate done for any variable of already existing second partial derivative or other high order derivatives.
- `Gradient` $\nabla{g}$ is a vector of partial derivatives of a multi-variable function, evaluated at a point. It points in the direction of steepest increase of the function, and its magnitude gives the rate of increase in that direction. $$\nabla f = \begin{bmatrix} \dfrac{\partial f}{\partial x} \\[1em] \dfrac{\partial f}{\partial y} \\[1em] \vdots \end{bmatrix}$$
