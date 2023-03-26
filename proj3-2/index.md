
https://l-winston.github.io/184writeup/proj3-2/index.html

# Overview
For this project, we decided to do Task 1 (Mirror and Glass Materials) and Task 4 (Depth of Field). In Task 1, we implemented reflection and refractions, and used them to create a realistic glass material. In Task 4, we modified `generate_ray` to take into account the effects of a thin lens.

# Task 1: Mirror and Glass Materials 
|`max_ray_depth`||description|
-|-|-|
0|![](spheres0.png)|only direct lighting
1|![](spheres1.png)|light bounces off walls to camera
2|![](spheres2.png)|light reflects off left ball to camera
3|![](spheres3.png)|light refracts through the right ball to camera
4|![](spheres4.png)|light refracts through right ball and reflects off left ball to camera
5|![](spheres5.png)|from ray 4+ all features appear and just more paths make it to camera
100|![](spheres100.png)|from ray 4+ all features appear and just more paths make it to camera

# Task 4: Depth of Field

The pinhole camera model assumes that light travels through a single point in the camera to form an inverted image on a flat image plane. This model uses simple geometry and does not take into account the effects of lens distortion, which can cause image distortion or blur, particularly around the edges of the image.

The thin-lens camera model assumes that light passes through a thin lens that bends the light to form an inverted image on a flat image plane. It considers the lens as a thin optical element and uses the lens equation to capture the effects of lens distortion and can produce more accurate images than the pinhole camera model


|focal depth||
-|-|
3.5|![](dragond35.png)
4|![](dragond4.png)
4.5|![](dragond45.png)
5|![](dragond5.png)


|focal depth||
-|-|
.21|![](dragona21.png)
.25|![](dragona25.png)
.30|![](dragona30.png)
.40|![](dragona40.png)

