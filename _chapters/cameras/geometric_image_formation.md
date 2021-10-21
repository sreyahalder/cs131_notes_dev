---
title: Geometric Image Formation
keywords: (insert comma-separated keywords here)
order: 17 # Lecture number for 2020
---

# Lecture 8: Geometric Image Formation

## Why Geometric Vision Matters
All images are 2D projections of a 3D world. To understand this 3D world around us, we can only work from the 2D information we have (images). It's important to be able to make sense of the geometry of this 2D information and extrapolate to 3 dimensions. 

## Geometric Primitives in 2D and 3D

### Points
A 2D point can be represented as $\textbf{x} = (x, y) \in R^2$, but also as a column vector where $$\textbf{x} =  \begin{bmatrix}
x \\
y
\end{bmatrix}$$.
Similarly, 3D points can be represented as $x=\textbf{x} = (x, y, z) \in R^3$, but also as a column vector where $$\textbf{x} = \begin{bmatrix}
x \\
y \\
z
\end{bmatrix}$$.
We can also represent points in 2D and 3D using homogeneous coordinates, where we append a 1. Thus, a 2D points becomes $(x, y, 1)$ and a 3D point becomes $(x, y, z, 1)$.

### Everything is easier in Projective Space
In projective space, it is easier to manipulate homogeneous coordinates. A 2D line can be represented as $l = (a, b, c)$ or as the equation $ax + by + c = 0$. In homogeneous coordinates, we see that $x^{-T}l = 0$.

### Homogeneous coordinates in 2D
Using homogeneous coordinates puts us in projective space. 2D projective space can be seen as something between $R^2$ and $R^3$ and can be represented as $P^2 = R^3 - (0, 0, 0)$. In this space, two lines will always meet at a point (which can sometimes be at infinity).
To go from heterogeneous to homogeneous coordinates, we just append a 1. To go from homogeneous to heterogeneous coordinates, we divide by the third component.

$$\begin{bmatrix}
x \\
y \\
w
\end{bmatrix} 

\rightarrow 

\begin{bmatrix}
$x/w$ \\
$y/w$
\end{bmatrix} $$.

This is because points that differ only by scale are equivalent in this space, so $\tilde{\textbf{x}} = (\tilde{x}, \tilde{y}, \tilde{z}) = \tilde{w}(x, y, 1) = \tilde{w}\bar{\textbf{x}}$.

### Everything is easier in Projective Space
We can now represent 2D and 3D lines in many ways. We know that for a line $l$, $\tilde{x}^Tl = 0$, for all points $\tilde{x}$ on the line $l$. The line $l$ can be represented using the normal to the line and the distance from the origin. Here, $\textbf{l} = (\hat{n}_x, \hat{n}_y, d) = (\hat{\textbf{n}}, d)$ where the magnitude of $\hat{\textbf{n}}$ is 1.

Similarly, a 3D plane $m$ can be represented where $\tilde{x}^Tm = 0$, or $\textbf{m} = (\hat{n}_x, \hat{n}_y, \hat{n}_z, d) = (\hat{\textbf{n}}, d)$

### Lines in 3D
Lines in 3D are a bit more complicated than planes. We can use two-point parametrization to represent them. Say we have two places at $z = 0, 1$ and two coordinates $\textbf{p} = (x_0, y_0)$ and $\textbf{q} = (x_1, y_1)$, the line going through them is $\textbf{r} = (1 - \lambda)\textbf{p} + \lambda\textbf{q}$. Then $\tilde{\textbf{r}} = \mu\tilde{\textbf{p}} + \lambda\tilde{\textbf{q}}$. This is useful for matters such as light fields.

### Cross Product
As a reminder, cross products (or vector products) is a vector operation in $\mathbb{R}^3$ denoted by the symbol $\times$. The cross product of two linearly independent vectors is a vector that is perpendicular to both vectors. If the two vectors are linearly dependent or either vector is the zero vector, the cross product is zero. 

The cross product can be evaluated as: 

$$\mathbf{a} \times \mathbf{b} = ||\mathbf{a}|| \, ||\mathbf{b}|| \sin{\theta}$$

where $\theta$ is the angle between the two vectors and || || denotes vector magnitude.

# 2D and 3D Transformations

So far we have imagined all sorts of geometries in 2D and 3D thanks to homogeneous coordinates -- all thanks to the fact that we are in perspective space. 

In respect to computer vision, this is important because a camera is mapping a 3D space to a 2D space. This mapping of real world object to photo is especially important when thinking about the third dimension that is not captured in two dimension representations. From different angles and distances, the same 3D object can have many different 2D representations. It is essential to be able to stitch together these different perspectives to have a comprehensive understanding of a 3D object even with exclusively 2D data. 

To solve this problem, one can induce a  planar homography. Consider a point in three dimensional space, it has two different two dimensional representations from  two different camera angles. To reconcile these two representations, we can consider an arbitrary plane in the three dimensional space. By looking at all the points lying on this plane from both perspectives, we are able to induce a planar homography and stitch these two 2D representations together. This aspect will be discussed later in this course. 

## 2D Transformations

There are many 2D transformations. It is really important to note that all transformations is just matrix multiplication. This is really important as we understand matrix multiplication and linear algebra very well and because these are computationally quick. The following table demonstrates the different transformations and their corresponding matrices. 

### Identity
The simplest transformation is simply doing nothing: multiplying by the identity matrix. The following is a $2\times 2$ identity matrix:


$$\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
$$

### Scaling
The next simplest transformation is scaling -- multiplying by the identity matrix where each element is scaled by some constant value. The effect of scaling is simply changing the size of an image. The result of this matrix multiplication is essentially just an element-wise multiplication by a set of constant values. 

Start with a point $p  = (x, y)$ and consider scaling this point by $s_x$ in the x direction and $s_y$ in the y direction. This scaling can be captured by the matrix:

$$\begin{bmatrix}
s_x & 0 \\
0 & s_y
\end{bmatrix}$$

### Rotation
Start with a point $p  = (x, y)$ and consider rotating this point by $\theta$ around the origin and let this rotated point be $(x', y')$.
(Add a figure here)

The rotation by $\theta$ can be captured via the rotation matrix $R$ where: 

$R =
\left[\begin{array}{cc}
\cos{\theta} & -\sin{\theta} \\
\sin{\theta} & \cos{\theta} \\
\end{array}\right]$

We show the derivation as follows:
$x = r \cos{\alpha}, y = r \sin{\alpha}$
$x' = r \cos{(\alpha + \theta)}, y' = r \sin{(\alpha + \theta)}$
Using the sum of two angles trigonometry properties, 
$x' = r (\cos{\alpha} \cos{\theta} - \sin{\alpha} \sin{\theta}), y' = r(\sin{\alpha}\cos{\theta} + \cos{\alpha} \sin{\theta})$ .
$x' = \cos{\theta} x - \sin{\theta} y, y' = \cos{\theta} y + \sin{\theta} x$.
$\left[\begin{array}{cc}
x'\\
y'
\end{array}\right]=
\left[\begin{array}{cc}
\cos{\theta} & -\sin{\theta} \\
\sin{\theta} & \cos{\theta} \\
\end{array}\right]
\left[\begin{array}{cc}
x\\
y\\
\end{array}\right]$ gives the rotation matrix $R$.

$R$ satisfies the following two properties:

 - Inverse is $R$ transpose:

To undo the rotation by $\theta$, we can rotate $(x', y')$ by $\theta$ counterclockwise, or by $-\theta$, as follows, 
$\left[\begin{array}{cc}
x\\
y
\end{array}\right]=
\left[\begin{array}{cc}
\cos{(-\theta)} & -\sin{(-\theta)} \\
\sin{(-\theta)} & \cos{(-\theta)} \\
\end{array}\right]
\left[\begin{array}{cc}
x'\\
y'\\
\end{array}\right]$. 
Notice the matrix being multiplied,
$\left[\begin{array}{cc}
\cos{\theta} & -\sin{\theta} \\
\sin{\theta} & \cos{\theta} \\
\end{array}\right] = \left[\begin{array}{cc}
\cos{\theta} & \sin{\theta} \\
-\sin{\theta} & \cos{\theta} \\
\end{array}\right] = R^T$

 - Is orthonormal:
 This follows immediately from the first property that the inverse is $R^T$. Rotating by $\theta$ clockwise, then rotating again by $\theta$ counter-clockwise is equivalent to multiplying by $R R^T$ which returns the original coordinates $(x, y)$ and is therefore an identity matrix. Note this property is invariant to the order of operations, so $RR^T = R^TR = I$. 

### Translation
(Add a diagram)
Translating $(x, y)$ by $t_x, t_y$ is equivalent to element-wise addition $(x + t_x, y + t_y)$.  In order to represent the addition as a matrix operation, we convert to homogeneous coordinates, so we consider $(x, y, 1)$ and $(t_x, t_y, 1)$. The resulting coordinates are $(x + t_x, y + t_y, 1)$, which can be obtained by the following matrix operation:

 $\left[\begin{array}{c}
x + t_x \\
y + t_y \\
1 \\
\end{array}\right] = \left[\begin{array}{ccc}
1 & 0 & t_x \\
0 & 1 &  t_y \\
0 & 0 & 1 \\
\end{array}\right]  \left[\begin{array}{c}
x  \\
y \\
1 \\
\end{array}\right]$.

$T = \left[\begin{array}{ccc}
1 & 0 & t_x \\
0 & 1 &  t_y \\
0 & 0 & 1 \\
\end{array}\right]$ represents a translation matrix for homogeneous coordinates. 

### Transformations Using Homogeneous Coordinates
As we saw with translations, there are notational benefits to using homogeneous coordinates for 2d transformations. Representing transformations with matrix operations will be useful for compositions since any compositions can be represented by a product of individual matrices. First, we introduce each transformation matrix with homogeneous coordinates and in the next section, we show how these matrices can be combined to generate more complex transformations.

#### Translation
Translating by $t_x, t_y$ along the x and y axes. 

$\left[\begin{array}{c}
x' \\
y'  \\
1  \\
\end{array}\right] = \left[\begin{array}{ccc}
1 & 0 & t_x \\
0 & 1 &  t_y \\
0 & 0 & 1 \\
\end{array}\right] \left[\begin{array}{c}
x \\
y  \\
1  \\
\end{array}\right] = \left[\begin{array}{c}
x + t_x \\
y + t_y \\
1  \\
\end{array}\right]$

#### Scaling 
Scaling the $x$ coordinate by $a$ and the $y$ coordinate by $b$, i.e., $(x, y) \rightarrow (ax, by)$. 

$\left[\begin{array}{c}
x' \\
y'  \\
1  \\
\end{array}\right] = \left[\begin{array}{ccc}
a & 0 & 0 \\
0 & b &  0 \\
0 & 0 & 1 \\
\end{array}\right] \left[\begin{array}{c}
x \\
y  \\
1  \\
\end{array}\right] = \left[\begin{array}{c}
ax \\
by \\
1  \\
\end{array}\right]$

#### Rotation
Rotating by $\theta$ around the origin.

$\left[\begin{array}{c}
x' \\
y'  \\
1  \\
\end{array}\right] = \left[\begin{array}{ccc}
\cos{\theta} & -\sin{\theta} & 0 \\
\sin{\theta} & \cos{\theta} &  0 \\
0 & 0 & 1 \\
\end{array}\right] \left[\begin{array}{c}
x \\
y  \\
1  \\
\end{array}\right] = \left[\begin{array}{c}
x \cos{\theta} - y \sin{\theta} \\
x \sin{\theta} + y  \cos{\theta} \\
1  \\
\end{array}\right]$

#### Shearing

Shearing pushes either the $x$ or the $y$ coordinate by $\phi$ along the respective axis, so shearing in $x$ direction means keeping the $y$ position the same and sliding $x$ by $\phi$ to the right, as shown in the figure. 

(Add figure)

$\left[\begin{array}{c}
x' \\
y'  \\
1  \\
\end{array}\right] = \left[\begin{array}{ccc}
1 & \tan{\phi} & 0 \\
0 & 1 &  0 \\
0 & 0 & 1 \\
\end{array}\right] \left[\begin{array}{c}
x \\
y  \\
1  \\
\end{array}\right] = \left[\begin{array}{c}
x + y \tan{\phi} \\
y \\
1  \\
\end{array}\right]$

Shearing in $y$ direction means keeping the same $x$ position and sliding $y$ by $\phi$ in the upward direction, as shown here. 

(Add figure)

$\left[\begin{array}{c}
x' \\
y'  \\
1  \\
\end{array}\right] = \left[\begin{array}{ccc}
1 & 0 & 0 \\
\tan{\phi} & 1 &  0 \\
0 & 0 & 1 \\
\end{array}\right] \left[\begin{array}{c}
x \\
y  \\
1  \\
\end{array}\right] = \left[\begin{array}{c}
x \\
x \tan{\phi} + y \\
1  \\
\end{array}\right]$

## Compositions of Transformations
Next, we will be combining different types of 2d transformations to apply different compositions. 

### Euclidean/Rigid
The Euclidean, or rigid, transformation is a combination of applying rotation and translation. In matrix form, we can compose $F_{\text{rotation}}$ with $F_{\text{translation}}$ as follows: 
$F_{\text{rotation}} F_{\text{translation}} =  \left[\begin{array}{ccc}
\cos{\theta} & -\sin{\theta} & 0 \\
\sin{\theta} & \cos{\theta} & 0  \\
0 & 0 & 1  \\
\end{array}\right] \left[\begin{array}{ccc}
1 & 0 & t_x \\
0 & 1 & t_y  \\
0 & 0 & 1  \\
\end{array}\right] = \left[\begin{array}{ccc}
\cos{\theta} & -\sin{\theta} & t_x \\
\sin{\theta} & \cos{\theta} & t_y  \\
0 & 0 & 1  \\
\end{array}\right]$, which we obtain as the result of matrix product. 

This transformation has **three** parameters: the rotation angle $\theta$, the change to $x$ coordinate, $t_x$ and the change to $y$ coordinate, $t_y$. Therefore, this transformation has **three degrees of freedom**.  Euclidean transform preserves the shape of an object (e.g., angles, lengths, parallelism).

### Similarity
The similarity transformation is a combination of doing scaling, rotation, and translation. In matrix form, we can obtain the transformed point after a similarity transformation by the following multiplication:

$$\begin{bmatrix}
x'\\
y'\\
w'
\end{bmatrix} = 
\begin{bmatrix}
a & -b & t_x\\
b & a & t_y\\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
w
\end{bmatrix} $$

This transformation has **four** parameters: the rotation angle $\theta$, the scaling factor $s$, and $t_x$ and $t_y$ (Note that $a = scos(\theta)$ and $b = ssin(\theta)$). Therefore this transformation also has **four degrees of freedom** (where the number of degrees of freedom is the number of independent quantities necessary to express the values of all the variable properties of a system).

Similarity transformations preserve lines, parallelism, and angles.

Sources: 
[https://nvision-user-guide.readthedocs.io/en/latest/geometric_transformations.html](https://nvision-user-guide.readthedocs.io/en/latest/geometric_transformations.html)

[https://www.britannica.com/science/degree-of-freedom-mathematics-and-statistics](https://www.britannica.com/science/degree-of-freedom-mathematics-and-statistics)

### Affine Transformations
Affine transformations are combinations of linear transformations and translations. In matrix form, we can obtain the transformed point after an affine transformation by the following multiplication:

$$\begin{bmatrix}
x'\\
y'\\
w'
\end{bmatrix} = 
\begin{bmatrix}
a & b & c\\
d & e & f\\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
w
\end{bmatrix} $$

This transformation has **six** parameters: four ($a, b, d, e$) from the arbitrary linear transformations and $c$ and $f$ from the translations. Therefore this transformation also has **six degrees of freedom**.

Affine transformations preserve lines, parallelism, and ratios, but because of the translations, does not necessarily map the origin to the origin.

### Projective Transformations (aka Homography)
Projective transformations are combinations of affine transformations and projective warps. In matrix form, we can obtain the transformed point after a projective transformation by the following multiplication:

$$\begin{bmatrix}
x'\\
y'\\
w'
\end{bmatrix} = 
\begin{bmatrix}
a & b & c\\
d & e & f\\
g & h & i
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
w
\end{bmatrix} $$

Note that for homogenous coordinates, points that differ by a constant nonzero scale factor are equivalent. If we pick $i$ to be the scale factor, we can divide each element by $i$ to get the equivalent equation:

$$\begin{bmatrix}
x'\\
y'\\
w'
\end{bmatrix} = 
\begin{bmatrix}
j & k & l\\
m & n & o\\
p & q & 1
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
w
\end{bmatrix} $$

This shows us that although we have **nine** parameters, there are only **eight degrees of freedom**. 

Projective transformations only preserve lines, and do not necessarily preserve parallelism, ratios, or mapping from the origin to the origin.

### Composing Transformations
We can represent compositions of transformations with matrix multiplication. For example, if we wanted to transform a point $p$ first by scaling, then by rotation, then by a second rotation, the matrix representation would be:

$p' = R_2(R_1(S(p)))$.

But instead of using three matrices for the transformations, we can multiply the matrices instead to get a single matrix that represents the composition of the three transformations:

$p' = (R_2R_1S)p$.

Note that the order of the matrix multiplication matters. The matrices are in order from right to left--the matrix associated with the first transformation should be the leftmost matrix in the multiplication, and subsequent transformation matrices should be to the right. 
