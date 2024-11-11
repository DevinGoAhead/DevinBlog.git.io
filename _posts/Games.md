# Games101

## Chapter 4

如图所示，目的是将 f 面 投影至 n 面

<img src = "D:\Xuyang Wei\Documents\Typora\Games\Image\101-4-1 f project to n.png" style= "zoom: 40%">

- 这里将投影的过程分解为两个步骤

  1. 将 frustum “挤压” 为一个 “cuboid”

  <img src = "D:\Xuyang Wei\Documents\Typora\Games\Image\101-4-2 frustum squish into a cuboid.png" style= "zoom: 40%">

  2. 然后再将 f 面经正交投影， 投影至 n 面

  ==这里只讨论第一个步骤==

### 挤压

在挤压的过程中，以下 3 个结论是显然的，或者说是我们挤压的一种前置要求

n 面所有点坐标都不会变

f 面所有点的 z 坐标都不会变

f 面中心点的坐标不会变

#### m 面([f, n] 之间的面)任意点的

设 $(x_m, y_m, z_m) $是 m 面上一点, 挤压后为 $(x_m^\prime, y_m^\prime, z_m^\prime) $

挤压后的 m 面与 n 面的尺寸是完全相等的, 因此任意点的$(x_m^\prime, y_m^\prime) $ 与 n 面的对应点的 $(x_n, y_n) $ 是相等的, 所以这里不妨通过计算 n 面的  $(x_n, y_n) $  ,间接的计算 m 面的 $(x_m^\prime, y_m\prime) $

如图所示, 

<img src = "D:\Xuyang Wei\Documents\Typora\Games\Image\101-4-3 f similar triangle.png" style= "zoom: 35%">

由相似三角形可得
$$
x_m^\prime = x_n = \dfrac{n}{z_m} x_m \\
y_m^\prime = y_n = \dfrac{n}{z_m} y_m
$$
而 $z_m^\prime$ 目前无法求得, 可暂时保持未知

综上述条件可得
$$
M^{4 \times 4}_{persp \rightarrow ortho}
\begin{pmatrix}
x_m \\ y_m \\ z_m \\ 1
\end{pmatrix}
= 
\begin{pmatrix}
x_m^\prime \\ y_m^\prime \\ z_m^\prime \\ 1
\end{pmatrix}
=
\begin{pmatrix}
\dfrac{n}{z_m}x_m \\
\dfrac{n}{z_m}y_m \\
? \\
1
\end{pmatrix}
=
\begin{pmatrix}
nx_m \\ ny_m \\ ? \\ z_m
\end{pmatrix}
$$
"挤压"变换矩阵的形式如下
$$
M^{4 \times 4}_{persp \rightarrow ortho}=
\begin{pmatrix}
a_{11} & a_{12} & a_{13} & a_{14} \\
a_{21} & a_{22} & a_{23} & a_{24} \\
a_{31} & a_{32} & a_{33} & a_{34} \\
a_{41} & a_{42} & a_{43} & a_{44} \\
\end{pmatrix}
$$
则
$$
\begin{array}{l}
& (1) & a_{11}x_m + a_{12}y_m + a_{13}z_m + a_{14} = nx_m \\ 
& (2) &a_{21}x_m + a_{22}y_m + a_{23}z_m + a_{24} = ny_m \\
& (3) \\
& (4) & a_{41}x_m + a_{42}y_m + a_{43}z_m + a_{44} = z_m \\
&由\\
&  & (1) \Rightarrow nx_m 只与 x_m 相关且无常数项, \:\therefore a_{12} = a_{13} = a_{14} = 0, \; a_{11} = n \\
&  & (2) \Rightarrow ny_m 只与 y_m 相关且无常数项, \:\therefore a_{11} = a_{13} = a_{14} = 0, \; a_{12} = n \\
&  & (4) \Rightarrow z_m 只与 z_m 相关且无常数项, \:\therefore a_{11} = a_{12} = a_{14} = 0, \; a_{13} = 1
\end{array}
$$

$$
\therefore &
M^{4 \times 4}_{persp \rightarrow ortho}=
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
a_{31} & a_{32} & a_{33} & a_{34} \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
$$

#### n 面任意点

设 $(x_n, y_n, z_n) $是 n 面上一点, 挤压后为 $(x_n^\prime, y_n^\prime, z_n^\prime) $
显然 
$$
x_n^\prime = x_n \\
y_n^\prime = y_n \\
z_n^\prime = z_n =  n \\
$$

$$
\begin{array}{l}
& 则 \\
& M^{4 \times 4}_{persp \rightarrow ortho}
\begin{pmatrix}
x_n \\ y_n \\ n \\ 1
\end{pmatrix}
= 
\begin{pmatrix}
x_n^\prime \\ y_n^\prime \\ z_n^\prime \\ 1
\end{pmatrix}
=
\begin{pmatrix}
x_n \\ y_n \\ n \\ 1
\end{pmatrix}
=
\begin{pmatrix}
nx_n \\ ny_n \\ n^2 \\ n
\end{pmatrix}
\\
&  \therefore \;a_{31}x_n + a_{32}y_n + a_{33}n + a_{34} = n^2 \\
&  \therefore \;a_{31} = a_{32} = 0 \quad 且\:a_{33}n + a_{34} = n^2 \\
\end{array}
$$

#### f 面中心点

设 $(0, 0, z_f) $是 f 面中心点, 挤压后为 $(0, 0, z_f^\prime) $
显然 $z_f^\prime = z_f = f$
$$
\begin{array}{l}
& 则 \\
& M^{4 \times 4}_{persp \rightarrow ortho}
\begin{pmatrix}
0 \\ 0 \\ f \\ 1
\end{pmatrix}
= 
\begin{pmatrix}
0 \\ 0 \\ z_f^\prime \\ 1
\end{pmatrix}
=
\begin{pmatrix}
0 \\ 0\\ f \\ 1
\end{pmatrix}
=
\begin{pmatrix}
0 \\ 0 \\ f^2 \\ f
\end{pmatrix}
\\
&  \therefore \; a_{33}f + a_{34} = f^2 \\
\end{array}
$$

$$
\begin{array}{l}
& 由 \\
& & a_{33}n + a_{34} = n^2 \\
& & a_{33}f + a_{34} = f^2 \\
& 可计算得 \\
& & a_{33} = n + f \\
& & a_{34} = -nf \\
\end{array}
$$

**综上所述**
$$
M^{4 \times 4}_{persp \rightarrow ortho}=
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & n+f & -nf \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
$$
