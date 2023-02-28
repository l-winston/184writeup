
https://l-winston.github.io/184writeup/proj2/index.html

# Task 1
de Casteljau's algorithm 

![](screenshot_2-14_18-39-16.png)

# Task 2
Since we now are increasing the number of samples by the factor `sample_rate`, we need to increase the size of the buffer from `width*height` to `width*height*sample_rate`. This is reflected in `set_sample_rate` and `set_framebuffer_target`.

First, we compute `sqrt(sample_rate)` to get the supersampling factor in each dimension. Then, we iterate over the indicies of the buffer corresponding to samples in the bounding box of the triangle. We convert the indicies `(i, j)` to real world coordinates `((i*1.0/rate)+0.5/rate, (j*1.0/rate)+0.5/rate)`. 

Using these real world coordinates, we can then perform the same 3 line tests to determine if the sampled point is in the triangle or not. 

Finally, we fill the appropriate indicies with the given color.

Also, we modified `resolve_to_framebuffer` to average over supersamples before sending them to the framebuffer.

Supersampling is useful because it simulates removing frequencies above the Nyquist frequency before sampling. It approxmiates box pre-filter antialiasing.

![](screenshot_2-14_18-59-48.png)
![](screenshot_2-14_18-59-51.png)
![](screenshot_2-14_18-59-53.png)

# Task 3 

![](screenshot_2-14_19-17-8.png)

We made cubeman do jumping jacks!

# Task 4

Barycentric coordinates express a point inside of a triangle as a weighted sum of each of its vertices. Barycentric coordinates are expressed as a tuple `(a, b, c)` where `0 <= a, b, c <= 1` and `a+b+c=1`. `a` indicates how close the point is to the first vertex, `b` the second vertex, and `c` the third.

![](screenshot_2-14_22-14-23.png)

This image illustrates a triangle shaded using barycentric interpolation. As you can see, points closer to a vertex have a color more similar to that vertex, and it blends smoothly as we move towards the middle section of the triangle.

![](screenshot_2-14_19-42-54.png)

# Task 5

Pixel sampling is the process of figuring out what RGB value a pixel in the triangle should be given the triangle, the coordinates of the pixel, and the corresponding triangle in the texture. Both nearest sampling and bilenear sampling use barycentric interpolation to get from a point (x, y) to texture coordinates (u, v) given the 3 vertices of the triangle in both xy space and uv space.

Nearest pixel sampling uses the interpolated `(u, v)` coordinates and takes the color of the closest texture pixel to it.

Bilinear sampling looks at the 4 texture pixels around `(u, v)` and does bilinear interpolation between the color values of them to get the sample.

|       | Nearest | Bilinear |
| ----------- | ----------- | -|
| 1x      | ![](near1.png) | ![](bi1.png) |
| 16x   | ![](near16.png) | ![](bi16.png) |

# Task 6

A mipmap is a sequence of different representations of pixels/textures with different resolution levels. Level sampling is the process of creating mipmaps and choosing which level would best suit each individual area of the texture. To decide which level should be used on a specific area of the texture, the algorithm measures the gradient by looking at the texture coordinates `(u, v)` around a sample. Closer uv coordinates indicate that the gradient is smaller and farther uv coordinates indicate that the gradient is larger. A smaller gradient means that a higher level of the mipmap will be selected because more detail is required, and vice versa for a larger gradient. If we do this level selection across the whole texture, different levels of the mipmap will be chosen corresponding to how much detail is needed in different parts of the texture.

Pixel sampling either nearest neighbor or bilinear. Bilinear is slower because we need to sample all 4 neighbors and bilinearly interpolate between them. Since bilinear interpolates over nearby texels, points sampled near the middle of the 4 texels will have smoother values and thus more antialiasing. 

Level sampling is either zero, nearest, or linear. Zero is the fastest becase we don't need to compute the level. It also uses the least memory because we don't need to store any mipmap levels, just the texture. Nearest is faster than linear because linear needs to sample twice and interpolate between them, while nearest just samples at `floor(level`). They both use the same amount of memory because they can use the same number of mipmap levels. Finally, linear has more antialiasing power than nearest. 

Supersampling is slower because you need to sample more points. It also costs more memory because it increases the size of the buffer by a factor equal to the sample rate. However, it has more antialiasing power because it simulates removing high frequency signals before sampling.


|       | P_NEAREST | P_LINEAR |
| ----------- | ----------- | -|
| L_ZERO      | ![](zero.png) | ![](bilinear.png) |
| L_NEAREST   | ![](nearest.png) | ![](both.png) |