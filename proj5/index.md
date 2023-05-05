# 5.2A Final Report Webpage (CS184 Students)

## Abstract
Ray marching is a technique that uses a signed distance function to vastly speed up the process of tracing rays. This distance function tells us how far the closest point in the scene is from any point. Using this, we can quickly extend our ray from the ray and compute intersections without overshooting. In this project, we want to exploit the recursive nature of fractals to speed up this process and create an interactive real-time rendering of interesting fractals.

# Technical approach
Our first attempt at implementing ray marching started with Project 3-1, where we replaced the code for raytracing with code for basic raymarching. I created a class `RayMarch` that contained various different functions defining the distance estimators of unique geometries, including primitives, fields, and fractals. `RayMarch` also had a function `trace` that took in a camera ray and returned the color of that ray. Finally, we modified `est_radiance_global_illumination` to use this `trace` function instead of performing raytracing. 

Since ray marching is deterministic, a render with one sample per pixel worked great. However, we ran into some bugs. One of these bugs was that rays would converge to the surface and a color would be loaded into `out_color`, but the ray would continue marching. This created many issues with performance, but also with coloring artifacts. We also had another floating point bug that caused the effective minimum distance to be zero, making it almost impossible for rays to converge even on extremely simple geometry. This caused spheres to appear speckly and more complex fractals to not even appear.

Our code used `generate_camera_rays` to generate the rays and then set t=0. Then, it would find the point on the ray at the given t value to evaluate the distance estimator at. If the distance is below the minimum distance, we mark it as a hit, else we advance t by that amount. This worked great, but the performance wasn’t good enough for real-time (~1/fps). To solve this issue, we had to look elsewhere.

We ended up settling on using project 4 ClothSim as our new starting point. We took the code for camera controls and the boilerplate for shaders and gutted everything else. This made it relatively easy to quickly move the code from the CPU onto the GPU, and it worked great! We were able to upgrade our framerate from <1fps to buttery smooth realtime (>60fps).

However, doing this came with its own set of unique and challenging bugs. Figuring out how to get the shader to render on the entire screen instead of just the cloth or collision objects was a annoying. I ended up just replacing the code in the phong shader, setting it up to be the default shader, and surrounding the camera with 6 giant plane objects so that no matter which way the camera looked you would still be rendering the scene. However, this wasn’t as easy as it seems since it involved modifying the input .json and also rewriting some of the object loader code to allow multiple planes. I also couldn’t move the position of the cloth in the .json so I had to shift the positions of all the point masses in the mesh initializer. After getting this all working, the rest of the features were much less of a headache.

We implemented ambient occlusion by using the number of steps required to converge as a heuristic for surface complexity and darkening the pixel accordingly. We implemented glow by mixing in a color (cyan) depending on the number of steps to converge. We also were able to estimate surface normals with numerical differentiation for coloring. This also allowed us to implement phong shading on fractals. We also implemented fog by mixing in a color depending on the distance from the camera.
## Results
How many licks does it take to get to the center of a Menger sponge?

![](image1.gif)

In a sky full of stars, I think I see you!

![](image2.gif)

Think of the bigger picture… literally.

![](image3.gif)

Appreciate natural beauty. (Mandelbulb with power Euler’s number.)

![](image4.gif)

## References
http://blog.hvidtfeldts.net/index.php/2011/06/distance-estimated-3d-fractals-part-i/
https://www.fractalforums.com/sierpinski-gasket/kaleidoscopic-(escape-time-ifs)/?PHPSESSID=b097968bb06fb1db1caf27855251b1e3
https://iquilezles.org/articles/


## Contributions from each team member

All three members contributed to the shader code. The transformation functions were written by Winston and David. The distance estimators were written by Winston and Matthew. The color tuning and output were written by David and Matthew. Optimization and refactoring were done mostly by Winston, and extensive debugging was done by David and Matthew.

All three members contributed to the write-up. The abstract was written by Winston. The technical approach section was written by Winston and David. The results and contributions section was written by Matthew, who also gathered the video clips.

These video clips were used for the team video, which Matthew edited and voiced over. The script was co-authored by all three members of the team.

## Random Notes
Started CPU -> ended GPU using shader


Our starting point was ClothSim, but the only part we utilized was the shader rendering and camera controls. (boilerplate)

Ray-marching
Alternative to ray-tracing, uses distance estimators
Distance estimators give us heuristic for how close we are to the nearest point in scene
Allows rays to exponentially converge most of the time
Fractals are interesting and fast because we can exploit their symmetry
fractals are recursive substructures built using reflections
easily be defined with reflections, rotations, translations, and scaling
Ex. sierpenski tetrahedron is made of one tetrahedron that’s reflected and rotated to create 4 tetrahedra (that’s step 1)
Repeat this process until desired complexity
Reflections, rotations, translations, and scaling are easily expressed in distance estimators
Ex. infinite field of spheres using modulo
folds infinite space into a small box
by modulo, every point in R^3 will be mapped into a box 
placing geometry into the box will cause infinite repetition
Our actual implementation
First try: start from proj 3-1 pathtracer and replace raytracing code with raymarching
Way too slow! Runs at ~1fps
Second try: build off of proj 4 clothsim and move code to gpu
Lots of challenges setting up the scene and getting the shaders to render to the right pixels
Eventually got it to work, runs buttery smooth even for much more complex fractals (>60fps)
What else did we do? 
Use time to convergence as heuristic for surface complexity, giving us ambient occlusion for free!
Calculate surface normals using numerical differentiation
Used for coloring 
Map normals to color values
Use normals to compute phong shading
In future could use surface normals to allowing interactions with other objects like physics collisions
Added fog for sense of depth


