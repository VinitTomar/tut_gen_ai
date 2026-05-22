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
- Solve Implicit differentiation with partial derivative technique

Time spent: ~4hrs
