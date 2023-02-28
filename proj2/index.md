
https://l-winston.github.io/184writeup/proj2/index.html

# Task 1: Bezier Curves with 1D de Casteljau Subdivision
de Casteljau's algorithm is a way to recursively subdivide bézier curves by using successive linear interpolation. By subdividing each line segment of the control polygon, we create a new polygon with one fewer segment. Eventually we are left with a single point, which is the point on the curve at `t`.

We implemented this in `evaluateStep` by iterating through the control points and interpolating between `n` of them with paramater `t` to create a new list of `n-1` points.

![](1-1.png)
![](1-2.png)
![](1-3.png)
![](1-4.png)
![](1-5.png)
![](1-6.png)
![](1-diff.png)


# Task 2: Bezier Surfaces with Separable 1D de Casteljau
de Casteljau's algoritm very easily extends to Bézier surfaces. We iterate over every row of points in the surface and perform 1D de Casteljau's algorithm to evaluate points at parameter `t`. Using this list of points from each row.

I copied my implementation of `evaluateStep` from `BezierCurve`. In `evaluate1D`, I recursively call `evaluateStep` to reduce the number of control points until there is only one point left, which `evaluate1D` returns. In `evaluate`, I iterate over every row of control points and evaluate them using `evaluate1D` to get a new list of control points for each row. Then, I make a final call to `evaluate1D` to evaluate the point along this new list at `t`.

![](1-2-teapot.png)

# Task 3: Area-Weighted Vertex Normals

To implement area-weighted vertex normals, the first thing I did was to collect a list of `neighbors` of the given vertex by iterating through halfedges and vertices. 

Next, I iterated over every consecutive pair of `neighbors`, which each define a unique triangle with the given vertex. Then, I used these three points to get two vectors to calculate normals and area. 

Finally, I summed up the normalized cross products multiplied by triangle areas and normalized the final sum.

![](3-1.png)

![](3-2.png)

# Task 4: Edge Flip

I drew out a diagram that included all edges, halfedges, vertices, and faces before and after the edge flip. Then, I methodically extracted every edge/halfedge/vertex/face object from the mesh. Finally, I went through each one and reassigned the pointers so they matched the diagram of after the edge flip. 

![](4-1.png)
![](4-2.png)

# Task 5: Edge Split



# Task 6

