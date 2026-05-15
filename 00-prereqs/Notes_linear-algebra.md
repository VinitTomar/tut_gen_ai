## 07-05-2026 — Phase 0 - Session 1

What I did:

- Watched essence of liner algebra, linear combination, basis vector, span

What clicked:

- Vector is the building block of linear algebra
  - Column form: $\vec{v} = \begin{bmatrix} x \\ y \end{bmatrix}$
- Representation of vector in Physics, Mathematics & Computer science
- Linear algebra is based around sum of vectors and scaler & vector multiplication
  - Vector addition: $\vec{u} + \vec{v} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \end{bmatrix}$
  - Scalar multiplication: $c\,\vec{v} = \begin{bmatrix} c \cdot v_1 \\ c \cdot v_2 \end{bmatrix}$
- Unit vectors $\hat{i}$ for X axis & $\hat{j}$ for Y axis, aka, "basis of coordinate system"
  - $\hat{i} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \quad \hat{j} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$
  - Any 2D vector: $\vec{v} = x\hat{i} + y\hat{j}$
- `Liner combination` is the scaling & then addition of two vectors
  - $a\vec{v} + b\vec{w}, \quad a, b \in \mathbb{R}$
- `Span` is a set of all linear combination of two vectors
  - $\text{span}(\vec{v}, \vec{w}) = \{\, a\vec{v} + b\vec{w} : a, b \in \mathbb{R} \,\}$
- `Linearly dependent` vector is another vectors that can be generated from linear combination of other vectors.
  - $\vec{u} = a\vec{v} + b\vec{w}$ for some scalars $a, b$
- `Linearly independent` vector is reverse of linearly dependent vector. It adds a new dimension to the vector span.

What's still fuzzy / questions to chase:

- Mathematical representation of a vector
  - As a column: $\vec{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix} \in \mathbb{R}^n$
  - As a row: $\vec{v} = \begin{bmatrix} v_1 & v_2 & \cdots & v_n \end{bmatrix}$

Time spent: ~18mins

## 08-05-2026 — Phase 0 - Session 2

What I did:

- Watched videos on matrices as linear transformation, matrix multiplication, 3D linear transformation

What clicked:

- `Linear transformation` is a transformation where grid lines don't curve & remains parallel and origin doesn't move. It is represented in matrix form. Here $L$ is linear transformation & $A$ is it's matrix representation
  - Preserves addition: $L(\vec{v} + \vec{w}) = L(\vec{v}) + L(\vec{w})$
  - Preserves scaling: $L(c\,\vec{v}) = c\,L(\vec{v})$
  - As a matrix: $L(\vec{v}) = A\vec{v}$, where the columns of $A$ are where $\hat{i}$ and $\hat{j}$ land after the transformation
  - $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}, \quad v = \begin{bmatrix} x \\ y \end{bmatrix} \text{then } A \cdot v = x\begin{bmatrix} a \\ c \end{bmatrix} + y\begin{bmatrix} b \\ d \end{bmatrix} = \begin{bmatrix} ax + by \\ cx + dy \end{bmatrix}$
- $\vec{v} = 1\hat{i} + 2\hat{j} \\ Transformed \ \vec{v} = 1(Transformed\ \hat{i}) + 2(Transformed\ \hat{j})$
- `Shear transformation` where either one of $\hat{i}$ or $\hat{j}$ remains fixed
- `One dimensional span` is when transformation of basis vectors ends up in $linearly \ dependent \ vectors$
- The application of a transformation is done by it's matrix multiplication with matrix of our vector.
- Multiple transformation can be applied by first doing matrix multiplication of all the transformation (aka composition) and then applying it to our $\vec{v}$ vector.
- $M_1 \cdot M_2 \ne M_2 \cdot M_1$
- In a transformation, we do matrix-vector multiplication between the transformation matrix and the vector. Each entry of the resulting vector is the dot product of a row of the matrix with the input vector.
- In transformation what we do is **matrix-vector multiplication**. Technically the dot is between two vector that gives a scalar.

What's still fuzzy / questions to chase:

- When multiple transformation are applied, then they are written left to right but they are applied right to left
  - $L(G(H(\vec{v}))) = (ABC)\vec{v}$, where $H$ acts first (rightmost), then $G$, then $L$ (leftmost)

Time spent: ~1.5hr

## 09-05-2026 - Phase 0 - Session 3

What I did:

- Watched video on determinant, linear system of equations, non-square matrix, dot product, duality, cross product

What clicked:

- `Determinant` is a scaling factor by which a linear transformation changes any area in a 2D plane and in 3D it changes the volume.
  - $\det\begin{pmatrix} \begin{bmatrix} a & b \\ c & d \end{bmatrix} \end{pmatrix} = ad - bc$
- If determinant is zero in a 2D then it means transformation squeezes space into a line, and in a 3D it means transformation squeeze space into a plane. It also means columns/vectors in matrix are linearly dependent.
- `Inverse` of a matrix is that matrix which gives an Identity matrix when multiplied to it.
  - When $det(A) = 0$, then it means inverse of that matrix doesn't exist.
- `Column space` of $A$ is set of all possible output of $A \cdot \vec{v}$. It is the span of output where basis vector lands after transformation. Also, it is the span of the columns of our matrix.
- `Null space` is a set of vectors that land on the origin or kernel of matrix.
- `Rank` is the number of dimensions or linearly independent column in a matrix or the output of a transformation(Transformation output is also a matrix). It is the number of dimensions in the column space.
- `Full rank` is when a matrix has equal number of linearly independent columns / basis vector, to the dimension of a matrix in a $N$ x $N$ matrix.
- `Identity` transformation does nothing to the vector it is applied on
  - $A^{-1} \cdot A = I = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$
- `Dot product` is a helpful tool in the projection of one vector to the another. If product is negative then they are in opposite direction, if positive then they are in same direction or if zero then they are perpendicular to each other.
  - All transformation don't preserve dot product before and after transformation. The one who does, they are named `orthonormal` transformations.
- When a linear transformation is applied to a line having dots equally place on it in a higher dimension space, then after transformation those dots will be equally place in the lower output dimension space. Otherwise it is not a liner transformation.
- Dual of a vector is the linear transformation it encodes $\Longleftrightarrow$ a liner transformation from some space to one dimension is a certain vector in that space.
- `Cross product` is $\vec{p} = \vec{v} \times \vec{w}$ always a vector. Order matters.
  - area of a parallelogram created by adding a copy of these vector from tail to head on each other.
  - True cross product is the one when combining two 3D vectors give another 3D vector. This vector is perpendicular to the plain defined by those two vectors, direction is defined by **right hand rule** and length is equal to the area of parallelogram.
  - $\vec{v} \times \vec{w} = \begin{bmatrix} v_1 \\ v_2 \\ v_3 \end{bmatrix} \times \begin{bmatrix} w_1 \\ w_2 \\ w_3 \end{bmatrix} = \begin{bmatrix} v_2 w_3 - v_3 w_2 \\ v_3 w_1 - v_1 w_3 \\ v_1 w_2 - v_2 w_1 \end{bmatrix}$
  - In some sense it is related to determinant.

What is still fuzzy / questions to chase:

- Column space, how it helps us understand when a solution exist.
- Null space, why it is a kernel of a matrix. How it help us to understand how a set of all possible solution look like.
- Transformation between 2D to 3D to 1D or vice-versa. The thing I need to explore is where vector or points land after the transformation is applied.
- Why 2D cross product & 3D cross product differ? In 2D cross product we get a number, but in 3D we get a vector.

Time spent: ~ 3hrs

## 14-05-2026 - Phase 0 - Session 4

What I did:

- Watched video on "change of basis", EigenVector & EigenValues,

What clicked:

- Consider two plain $\mathcal{P_1} \text{ \& } \mathcal{_2P}$. $\mathcal{P_2}$ have basis vector of $\mathcal{P_1}$ written in the $\mathcal{P_2}$'s coordinate system by matrix $A$. Then we can translate any vector $\vec{VP_1}$ of $\mathcal{P_1}$ into $\mathcal{P_2}$ by vector multiplication of $A \cdot \vec{VP_1}$. Also, we can translate any vector $\vec{VP_2}$ of $\mathcal{P_2}$ into $\mathcal{P_1}$ by vector multiplication of $A^{-1}$ & $\vec{VP_2}$.
- An expression like $A^{-1}MA$ suggests a mathematical `empathy` where $M$ represent a transformation and outer two matrices represents `empathy`, the shift in perspective. It is used to convert a transformation $M$ from one plane to another.
- A vector $\vec{v}$ remains at it's span(the line it draws) after applying a transformation is called `EigenVector` and the factor by which it scales is called `EigenValue`. In 3D, EigenVector gives the axis of rotation.
  - $A\vec{v} = \lambda \vec{v}$, where $A$ is transformation, $\vec{v}$ is eigenvector & $\lambda$ is eigenvalue. Also can be written as $A\vec{v} = \lambda I \vec{v}$, where $I$ is identity matrix
- `Diagonal` matrices $D$ are the those who have zeros everywhere except the diagonal from left to right and top to bottom. These matrices are easy to work with for example calculating $D^{100}$.
- In a co-ordinate system where basis vector acts as eigenvectors with a transformation, then that transformation will be a Diagonal matrix with the eigenvalue at diagonal telling how each basis vector scale.
- If transformation $M$ is not a diagonal matrix & has multiple eigenvectors, then $M$ can be converted into a diagonal matrix by choose a set of eigenvectors as basis vector by $A^{-1}MA$ where $A$ is matrix that has eigenvectors as basis. All transformation doesn't have eigenvectors.
- A set of basis vector, where each vector is an eigenvector, they are called `Eigenbasis`.
- A $N \times N$ matrix can have up to $N$ eigenvalues.
- Eigenvalues tell how much an operation stretches space in a direction & Determinant tell how much areas & volumes as a whole.

What is still fuzzy / questions to chase:

- What is mathematical empathy.

Time spend: ~ 3hrs
