---
layout: post
title: Homogeneous coordinate 
tags: [Computer graphics]
categories: Technical
---


## Matrix
Before diving into the homogeneous coordinate, it's important to understand why matrix is preferred in transformation calculation. 
Let's begin the discussion with a simple scenario, If we want to rotate a point _**p**_ with respect to origin _**o**_ anti-clock wisely 90&deg;  
![](/assets/img/post/rotation.png)

Applying the linear algebra equation **Ax=b**, it's easy to write the new position of p<sub>1</sub>:

$$
p_1=R_1 \cdot p=
\begin{bmatrix}
\cos(90^{\circ})&-\sin(90^{\circ})\\
\sin(90^{\circ})&\cos(90^{\circ})\\
\end{bmatrix}\ \begin{bmatrix}
3\\3\end{bmatrix}=\begin{bmatrix}-3\\3\end{bmatrix}
$$

If we rotate p1 to p2 by 45 degree, then it can be written as:

$$
p_2=R_2 \cdot p_1=
\begin{bmatrix}
\cos(45^{\circ})&-\sin(45^{\circ})\\
\sin(45^{\circ})&\cos(45^{\circ})\\
\end{bmatrix} \begin{bmatrix}
-3\\3\end{bmatrix}=\begin{bmatrix}-4.24\\0.0\end{bmatrix}
$$

For matrix, the above two steps can be combined into one formula:

$$
p=R_2 \cdot R_1 \cdot p_1=
\begin{bmatrix}
\cos(45^{\circ})&-\sin(45^{\circ})\\
\sin(45^{\circ})&\cos(45^{\circ})\\
\end{bmatrix}
\begin{bmatrix}
\cos(90^{\circ})&-\sin(90^{\circ})\\
\sin(90^{\circ})&\cos(90^{\circ})\\
\end{bmatrix}\ \begin{bmatrix}
3\\3\end{bmatrix}=\begin{bmatrix}-4.24\\0.0\end{bmatrix}
$$

**Yes, you may observe some pattern from it - rotate this points to any angle and regardless of how many time you rotate it, you will get the correct position from this chain of matrix.**

$$
p_n=R_n \ldots R_4 \cdot R_3 \cdot R_2 \cdot R_1 \cdot p
$$

So, the rotation in matrix form is very compact and indeed computational friendly considering the fact that modern PC is very good at matrix computation. 

### How about translation? 
If we wish to translate the point p to a new location p3,
![](/assets/img/post/translation.png)

The position of a point can be changed by adding a vector.

$$
p_3=p+\vec{t}=\begin{bmatrix}
3\\3\end
{bmatrix}+\begin{bmatrix}
-6\\-1\end
{bmatrix}=\begin{bmatrix}
-3\\2\end
{bmatrix}
$$
It looks ok, because adding up numbers seems easier comparing with matrix multiplication. However, it's very common that we want to rotate and translate an object. **Translation** breaks the chain of matrix multiplication as you'll have to express the relation as:
$$
B=R \cdot (P+\vec{t}) \quad or \quad B=R \cdot P +\vec{t}
$$
Either rotation first or translation first will make the form difficult to expand.

### Homogeneous coordinate

That's why homogeneous coordinates is used everywhere in computer graphics. It  plays a vital role in solving this problem. What it does is to introduce an addional dimension in the vector. A 2D vector will be expanded to 3D, likewise, a 3D vectoer will bring a 4th component with it.

In a game or a 3D graphical application, a point is normally represented by a 4-component column(row) vector.  
$$
p=\begin{bmatrix}
a\\b\\c\\1\end
{bmatrix}
$$


A vector is obtained by subtraction of two points. It has a zero fourth-component, 
$$
\vec{v}=p_{1}-p_{2}=\begin{bmatrix}
a\\b\\c\\1\end
{bmatrix}-\begin{bmatrix}
d\\e\\f\\1\end
{bmatrix}=\begin{bmatrix}
a-d\\b-e\\c-f\\0\end
{bmatrix}
$$

For better illustration, let's still use the above __2D translation__ case.
The postion of p will be $$ \begin{bmatrix}3\\3\\1\end{bmatrix}$$.


$$
p_3=p+\vec{t}=\begin{bmatrix}
3\\3\\1\end
{bmatrix}+\begin{bmatrix}
-6\\-1\\0\end
{bmatrix}=\begin{bmatrix}
-3\\2\\1\end
{bmatrix}
$$


You may wonder what's the difference by adding one more row of number. It makes no sense as the plus __"+"__ is still there. Wait a moment, you'll see the difference. 

With the additional row, the vector addition can be written as a multiplication of Matrix and a vector with the help of [**Identity Matrix**](https://en.wikipedia.org/wiki/Identity_matrix):

$$
p_3=T \cdot p=\begin{bmatrix}
1&0&-6\\0&1&-1\\0&0&1\end
{bmatrix}\begin{bmatrix}
3\\3\\1\end
{bmatrix}=\begin{bmatrix}
-3\\2\\1\end
{bmatrix}
$$

Previously, the rotation matrix is a 2 by 2 matrix. How will it change to fit in the 3 by 3 case? 

**Simple!**

$$
p_2=R_2 \cdot R_1 \cdot p_1=
\begin{bmatrix}
\cos(45^{\circ})&-\sin(45^{\circ})&0\\
\sin(45^{\circ})&\cos(45^{\circ})&0\\
0&0&1
\end{bmatrix}
\begin{bmatrix}
\cos(90^{\circ})&-\sin(90^{\circ})&0\\
\sin(90^{\circ})&\cos(90^{\circ})&0\\
0&0&1
\end{bmatrix}\ \begin{bmatrix}
3\\3\\1\end{bmatrix}=\begin{bmatrix}-4.24\\0\\1\end{bmatrix}
$$

The 3 by 3 matrix is written like this because the object rotates along the z-axis.

> **_voilà!!_** you can chain the rotation and translation operation again. 

$$
p_3=T \cdot R_2 \cdot R_1 \cdot p_1=\begin{bmatrix}
1&0&-6\\0&1&-1\\0&0&1\end
{bmatrix}
\begin{bmatrix}
\cos(45^{\circ})&-\sin(45^{\circ})&0\\
\sin(45^{\circ})&\cos(45^{\circ})&0\\
0&0&1
\end{bmatrix}
\begin{bmatrix}
\cos(90^{\circ})&-\sin(90^{\circ})&0\\
\sin(90^{\circ})&\cos(90^{\circ})&0\\
0&0&1
\end{bmatrix}\ \begin{bmatrix}
3\\3\\1\end{bmatrix}=\begin{bmatrix}-3\\2\\1\end{bmatrix}
$$

Moreover, scaling can also be cascaded into the matrix operation. The scale matrix is usually represented as $$S=\begin{bmatrix}
s_x&0&0\\0&x_y&0\\0&0&1\end
{bmatrix}$$.

In conclusion, the transformation matrix has the following general form:

$$
p=S \cdot R \cdot T \cdot p_0=\begin{bmatrix}
s_x&0&0\\0&x_y&0\\0&0&1\end
{bmatrix} 
\begin{bmatrix}
\cos(\theta^{\circ})&-\sin(\theta^{\circ})&0\\
\sin(\theta^{\circ})&\cos(\theta^{\circ})&0\\
0&0&1
\end{bmatrix}\begin{bmatrix}
1&0&t_x\\0&1&t_y\\0&0&1\end
{bmatrix}
\begin{bmatrix}
x\\y\\1\end{bmatrix}
$$


