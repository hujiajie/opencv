Contour Features {#tutorial_js_contour_features}
================

Goal
----

In this article, we will learn

-   To find the different features of contours, like area, perimeter, centroid, bounding box etc
-   You will see plenty of functions related to contours.

1. Moments
----------

Image moments help you to calculate some features like center of mass of the object, area of the
object etc. Check out the wikipedia page on [Image
Moments](http://en.wikipedia.org/wiki/Image_moment)

We use the function: **cv.moments (array, binaryImage = false)** 
@param array         raster image (single-channel, 8-bit or floating-point 2D array) or an array ( 1×N or N×1 ) of 2D points.
@param binaryImage   if it is true, all non-zero image pixels are treated as 1's. The parameter is used for images only. 

Try it
------

Here is a demo. Canvas elements named momentsCanvasInput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
.err {
    color: red;
}
</style>
</head>
<body>
<div id="momentsCodeArea">
<h2>Input your code</h2>
<button id="momentsTryIt" disabled="true" onclick="momentsExecuteCode()">Try it</button><br>
<textarea rows="12" cols="90" id="momentsTestCode" spellcheck="false">
var src = cv.imread("momentsCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var M = cv.moments(contours.get(0), false);
momentsOutput.innerHTML = M.m00;
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); 
M.delete();
</textarea>
<p class="err" id="momentsErr"></p>
</div>
<div id="momentsShowcase">
    <div>
        <canvas id="momentsCanvasInput"></canvas>
    </div>
    <input type="file" id="momentsInput" name="file" />
    <p><strong>The m00 is: </strong><span id="momentsOutput"></span></p>
</div>
<script src="utils.js"></script>
<script async src="opencv.js" id="opencvjs"></script>
<script>
var momentsOutput = document.getElementById("momentsOutput");
function momentsExecuteCode() {
    var momentsText = document.getElementById("momentsTestCode").value;
    try {
        eval(momentsText);
        document.getElementById("momentsErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("momentsErr").innerHTML = err;
    }
}

loadImageToCanvas("lena.jpg", "momentsCanvasInput");
var momentsInputElement = document.getElementById("momentsInput");
momentsInputElement.addEventListener("change", momentsHandleFiles, false);
function momentsHandleFiles(e) {
    var momentsUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(momentsUrl, "momentsCanvasInput");
}
</script>
</body>
\endhtmlonly

From this moments, you can extract useful data like area, centroid etc. Centroid is given by the
relations, \f$C_x = \frac{M_{10}}{M_{00}}\f$ and \f$C_y = \frac{M_{01}}{M_{00}}\f$. This can be done as
follows:
@code{.js}
var cx = M.m10/M.m00
var cy = M.m01/M.m00
@endcode

2. Contour Area
---------------

Contour area is given by the function **cv.contourArea()** or from moments, **M['m00']**.

We use the function: **cv.contourArea (contour, oriented = false)** 
@param contour    input vector of 2D points (contour vertices)
@param oriented   oriented area flag. If it is true, the function returns a signed area value, depending on the contour orientation (clockwise or counter-clockwise). Using this feature you can determine orientation of a contour by taking the sign of an area. By default, the parameter is false, which means that the absolute value is returned.

Try it
------

Here is a demo. Canvas elements named areaCanvasInput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="areaCodeArea">
<h2>Input your code</h2>
<button id="areaTryIt" disabled="true" onclick="areaExecuteCode()">Try it</button><br>
<textarea rows="12" cols="90" id="areaTestCode" spellcheck="false">
var src = cv.imread("areaCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var area = cv.contourArea(contours.get(20), false);
areaOutput.innerHTML = area;
src.delete(); dst.delete(); contours.delete(); hierarchy.delete();
</textarea>
<p class="err" id="areaErr"></p>
</div>
<div id="areaShowcase">
    <div>
        <canvas id="areaCanvasInput"></canvas>
    </div>
    <input type="file" id="areaInput" name="file" />
    <p><strong>The area is: </strong><span id="areaOutput"></span></p>
</div>
<script>
var areaOutput = document.getElementById("areaOutput");
function areaExecuteCode() {
    var areaText = document.getElementById("areaTestCode").value;
    try {
        eval(areaText);
        document.getElementById("areaErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("areaErr").innerHTML = err;
    }
}

loadImageToCanvas("lena.jpg", "areaCanvasInput");
var areaInputElement = document.getElementById("areaInput");
areaInputElement.addEventListener("change", areaHandleFiles, false);
function areaHandleFiles(e) {
    var areaUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(areaUrl, "areaCanvasInput");
}
</script>
</body>
\endhtmlonly

3. Contour Perimeter
--------------------

It is also called arc length. It can be found out using **cv.arcLength()** function.

We use the function: **cv.arcLength (curve, closed)** 
@param curve    input vector of 2D points.
@param closed   flag indicating whether the curve is closed or not.

Try it
------

Here is a demo. Canvas elements named perimeterCanvasInput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="perimeterCodeArea">
<h2>Input your code</h2>
<button id="perimeterTryIt" disabled="true" onclick="perimeterExecuteCode()">Try it</button><br>
<textarea rows="12" cols="90" id="perimeterTestCode" spellcheck="false">
var src = cv.imread("perimeterCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var perimeter = cv.arcLength(contours.get(20), true);
perimeterOutput.innerHTML = perimeter;
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); 
</textarea>
<p class="err" id="perimeterErr"></p>
</div>
<div id="perimeterShowcase">
    <div>
        <canvas id="perimeterCanvasInput"></canvas>
    </div>
    <input type="file" id="perimeterInput" name="file" />
    <p><strong>The perimeter is: </strong><span id="perimeterOutput"></span></p>
</div>
<script>
var perimeterOutput = document.getElementById("perimeterOutput");
function perimeterExecuteCode() {
    var perimeterText = document.getElementById("perimeterTestCode").value;
    try {
        eval(perimeterText);
        document.getElementById("perimeterErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("perimeterErr").innerHTML = err;
    }
}

loadImageToCanvas("lena.jpg", "perimeterCanvasInput");
var perimeterInputElement = document.getElementById("perimeterInput");
perimeterInputElement.addEventListener("change", perimeterHandleFiles, false);
function perimeterHandleFiles(e) {
    var perimeterUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(perimeterUrl, "perimeterCanvasInput");
}
</script>
</body>
\endhtmlonly

4. Contour Approximation
------------------------

It approximates a contour shape to another shape with less number of vertices depending upon the
precision we specify. It is an implementation of [Douglas-Peucker
algorithm](http://en.wikipedia.org/wiki/Ramer-Douglas-Peucker_algorithm). Check the wikipedia page
for algorithm and demonstration.

We use the function: **cv.approxPolyDP (curve, approxCurve, epsilon, closed)** 
@param curve        input vector of 2D points stored in cv.Mat.
@param approxCurve  result of the approximation. The type should match the type of the input curve.
@param epsilon      parameter specifying the approximation accuracy. This is the maximum distance between the original curve and its approximation.
@param closed       If true, the approximated curve is closed (its first and last vertices are connected). Otherwise, it is not closed.

Try it
------

Here is a demo. Canvas elements named approxPolyDPCanvasInput and approxPolyDPCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="approxPolyDPCodeArea">
<h2>Input your code</h2>
<button id="approxPolyDPTryIt" disabled="true" onclick="approxPolyDPExecuteCode()">Try it</button><br>
<textarea rows="20" cols="90" id="approxPolyDPTestCode" spellcheck="false">
var src = cv.imread("approxPolyDPCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 100, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
var poly = new cv.MatVector();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// approximates each contour to polygon
for(var i = 0 ; i < contours.size(); ++i)
{
    let tmp = new cv.Mat();
    // You can try more different conversion
    cv.approxPolyDP(contours.get(i), tmp, 3, true);
    poly.push_back(tmp);
}
// draw contours with random Scalar
for(var i = 0 ; i < contours.size(); ++i)
{
    var color = new cv.Scalar(Math.round(Math.random() * 255), Math.round(Math.random() * 255), Math.round(Math.random() * 255));
    cv.drawContours( dst, poly, i, color, 1, 8, hierarchy, 0, [0,0]);
    color.delete();
}
cv.imshow("approxPolyDPCanvasOutput", dst);
src.delete(); dst.delete(); hierarchy.delete(); contours.delete(); poly.delete();
</textarea>
<p class="err" id="approxPolyDPErr"></p>
</div>
<div id="approxPolyDPShowcase">
    <div>
        <canvas id="approxPolyDPCanvasInput"></canvas>
        <canvas id="approxPolyDPCanvasOutput"></canvas>
    </div>
    <input type="file" id="approxPolyDPInput" name="file" />
</div>
<script>
function approxPolyDPExecuteCode() {
    var approxPolyDPText = document.getElementById("approxPolyDPTestCode").value;
    try {
        eval(approxPolyDPText);
        document.getElementById("approxPolyDPErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("approxPolyDPErr").innerHTML = err;
    }
}

loadImageToCanvas("lena.jpg", "approxPolyDPCanvasInput");
var approxPolyDPInputElement = document.getElementById("approxPolyDPInput");
approxPolyDPInputElement.addEventListener("change", approxPolyDPHandleFiles, false);
function approxPolyDPHandleFiles(e) {
    var approxPolyDPUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(approxPolyDPUrl, "approxPolyDPCanvasInput");
}
</script>
</body>
\endhtmlonly

5. Convex Hull
--------------

Convex Hull will look similar to contour approximation, but it is not (Both may provide same results
in some cases). Here, **cv.convexHull()** function checks a curve for convexity defects and
corrects it. Generally speaking, convex curves are the curves which are always bulged out, or
at-least flat. And if it is bulged inside, it is called convexity defects. For example, check the
below image of hand. Red line shows the convex hull of hand. The double-sided arrow marks shows the
convexity defects, which are the local maximum deviations of hull from contours.

![image](images/convexitydefects.jpg)

We use the function: **cv.convexHull (points, hull, clockwise = false, returnPoints = true)** 
@param points        input 2D point set.
@param hull          output convex hull. 
@param clockwise     orientation flag. If it is true, the output convex hull is oriented clockwise. Otherwise, it is oriented counter-clockwise. The assumed coordinate system has its X axis pointing to the right, and its Y axis pointing upwards.
@param returnPoints  operation flag. In case of a matrix, when the flag is true, the function returns convex hull points. Otherwise, it returns indices of the convex hull points.

Try it
------

Here is a demo. Canvas elements named convexHullCanvasInput and convexHullCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="convexHullCodeArea">
<h2>Input your code</h2>
<button id="convexHullTryIt" disabled="true" onclick="convexHullExecuteCode()">Try it</button><br>
<textarea rows="24" cols="90" id="convexHullTestCode" spellcheck="false">
var src = cv.imread("convexHullCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 100, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
var hull = new cv.MatVector();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// approximates each contour to convex hull
for(var i = 0 ; i < contours.size(); ++i)
{
    // You can try more different conversion
    let tmp = new cv.Mat();
    cv.convexHull(contours.get(i), tmp, false, true);
    hull.push_back(tmp);
}
// draw contours with random Scalar
for(var i = 0 ; i < contours.size(); ++i)
{
    var colorHull = new cv.Scalar(Math.round(Math.random() * 255), Math.round(Math.random() * 255), Math.round(Math.random() * 255));
    cv.drawContours(dst, hull, i, colorHull, 1, 8, hierarchy, 0, [0,0]);
    var colorContours = new cv.Scalar(Math.round(Math.random() * 255), Math.round(Math.random() * 255), Math.round(Math.random() * 255));
    colorHull.delete(); colorContours.delete();
}
cv.imshow("convexHullCanvasOutput", dst);
src.delete(); dst.delete(); hierarchy.delete(); contours.delete(); hull.delete();
</textarea>
<p class="err" id="convexHullErr"></p>
</div>
<div id="convexHullShowcase">
    <div>
        <canvas id="convexHullCanvasInput"></canvas>
        <canvas id="convexHullCanvasOutput"></canvas>
    </div>
    <input type="file" id="convexHullInput" name="file" />
</div>
<script>
function convexHullExecuteCode() {
    var convexHullText = document.getElementById("convexHullTestCode").value;
    try {
        eval(convexHullText);
        document.getElementById("convexHullErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("convexHullErr").innerHTML = err;
    }
}

loadImageToCanvas("lena.jpg", "convexHullCanvasInput");
var convexHullInputElement = document.getElementById("convexHullInput");
convexHullInputElement.addEventListener("change", convexHullHandleFiles, false);
function convexHullHandleFiles(e) {
    var convexHullUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(convexHullUrl, "convexHullCanvasInput");
}
</script>
</body>
\endhtmlonly

6. Checking Convexity
---------------------

There is a function to check if a curve is convex or not, **cv.isContourConvex()**. It just return
whether True or False. Not a big deal.

@code{.js}
cv.isContourConvex(contours.get(i));
@endcode

7. Bounding Rectangle
---------------------

There are two types of bounding rectangles.

### 7.a. Straight Bounding Rectangle

It is a straight rectangle, it doesn't consider the rotation of the object. So area of the bounding
rectangle won't be minimum.

We use the function: **cv.boundingRect (points)** 
@param points        input 2D point set.

Try it
------

Here is a demo. Canvas elements named boundingRectCanvasInput and boundingRectCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="boundingRectCodeArea">
<h2>Input your code</h2>
<button id="boundingRectTryIt" disabled="true" onclick="boundingRectExecuteCode()">Try it</button><br>
<textarea rows="15" cols="90" id="boundingRectTestCode" spellcheck="false">
var src = cv.imread("boundingRectCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var rect = cv.boundingRect(contours.get(0));
var contoursColor = new cv.Scalar(255, 255, 255);
var rectangleColor = new cv.Scalar(255, 0, 0);
cv.drawContours(dst, contours, 0, contoursColor, 1, 8, hierarchy, 100, [0,0]);
cv.rectangle(dst, [rect.x,rect.y], [rect.x+rect.width,rect.y+rect.height], rectangleColor, 2, cv.LINE_AA, 0);
cv.imshow("boundingRectCanvasOutput", dst);
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); rect.delete(); contoursColor.delete(); rectangleColor.delete();
</textarea>
<p class="err" id="boundingRectErr"></p>
</div>
<div id="boundingRectShowcase">
    <div>
        <canvas id="boundingRectCanvasInput"></canvas>
        <canvas id="boundingRectCanvasOutput"></canvas>
    </div>
    <input type="file" id="boundingRectInput" name="file" />
</div>
<script>
function boundingRectExecuteCode() {
    var boundingRectText = document.getElementById("boundingRectTestCode").value;
    try {
        eval(boundingRectText);
        document.getElementById("boundingRectErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("boundingRectErr").innerHTML = err;
    }
}

loadImageToCanvas("LinuxLogo.jpg", "boundingRectCanvasInput");
var boundingRectInputElement = document.getElementById("boundingRectInput");
boundingRectInputElement.addEventListener("change", boundingRectHandleFiles, false);
function boundingRectHandleFiles(e) {
    var boundingRectUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(boundingRectUrl, "boundingRectCanvasInput");
}
</script>
</body>
\endhtmlonly

### 7.b. Rotated Rectangle

Here, bounding rectangle is drawn with minimum area, so it considers the rotation also. 

We use the function: **cv.minAreaRect (points)** 
@param points        input 2D point set.

Try it
------

Here is a demo. Canvas elements named minAreaRectCanvasInput and minAreaRectCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="minAreaRectCodeArea">
<h2>Input your code</h2>
<button id="minAreaRectTryIt" disabled="true" onclick="minAreaRectExecuteCode()">Try it</button><br>
<textarea rows="18" cols="90" id="minAreaRectTestCode" spellcheck="false">
var src = cv.imread("minAreaRectCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var rotatedRect = cv.minAreaRect(contours.get(0));
var vertices = new cv.Point2fVector()
rotatedRect.points(vertices);
var contoursColor = new cv.Scalar(255, 255, 255);
var rectangleColor = new cv.Scalar(255, 0, 0);
cv.drawContours(dst, contours, 0, contoursColor, 1, 8, hierarchy, 100, [0,0]);
// draw rotatedRect
for (var i = 0; i < 4; i++)
    cv.line(dst, vertices.get(i), vertices.get((i + 1) % 4), rectangleColor, 2, cv.LINE_AA, 0);
cv.imshow("minAreaRectCanvasOutput", dst);
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); rotatedRect.delete(); contoursColor.delete(); rectangleColor.delete();
</textarea>
<p class="err" id="minAreaRectErr"></p>
</div>
<div id="minAreaRectShowcase">
    <div>
        <canvas id="minAreaRectCanvasInput"></canvas>
        <canvas id="minAreaRectCanvasOutput"></canvas>
    </div>
    <input type="file" id="minAreaRectInput" name="file" />
</div>
<script>
function minAreaRectExecuteCode() {
    var minAreaRectText = document.getElementById("minAreaRectTestCode").value;
    try {
        eval(minAreaRectText);
        document.getElementById("minAreaRectErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("minAreaRectErr").innerHTML = err;
    }
}

loadImageToCanvas("LinuxLogo.jpg", "minAreaRectCanvasInput");
var minAreaRectInputElement = document.getElementById("minAreaRectInput");
minAreaRectInputElement.addEventListener("change", minAreaRectHandleFiles, false);
function minAreaRectHandleFiles(e) {
    var minAreaRectUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(minAreaRectUrl, "minAreaRectCanvasInput");
}
</script>
</body>
\endhtmlonly

8. Minimum Enclosing Circle
---------------------------

Next we find the circumcircle of an object using the function **cv.minEnclosingCircle()**. It is a
circle which completely covers the object with minimum area.

We use the functions: **cv.minEnclosingCircle (points, circle)** 
@param points        input 2D point set.
@param circle        output circle.

**cv.circle (img, center, radius, color, thickness = 1, lineType = cv.LINE_8, shift = 0)** 
@param img          image where the circle is drawn.
@param center       center of the circle.
@param radius       radius of the circle.
@param color        circle color.
@param thickness    thickness of the circle outline, if positive. Negative thickness means that a filled circle is to be drawn.
@param lineType     type of the circle boundary. 
@param shift        number of fractional bits in the coordinates of the center and in the radius value.

Try it
------

Here is a demo. Canvas elements named minEnclosingCircleCanvasInput and minEnclosingCircleCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="minEnclosingCircleCodeArea">
<h2>Input your code</h2>
<button id="minEnclosingCircleTryIt" disabled="true" onclick="minEnclosingCircleExecuteCode()">Try it</button><br>
<textarea rows="18" cols="90" id="minEnclosingCircleTestCode" spellcheck="false">
var src = cv.imread("minEnclosingCircleCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var circle = new cv.Circle;
cv.minEnclosingCircle (contours.get(0), circle);
var contoursColor = new cv.Scalar(255, 255, 255);
var circleColor = new cv.Scalar(255, 0, 0);
cv.drawContours(dst, contours, 0, contoursColor, 1, 8, hierarchy, 100, [0,0]);
cv.circle(dst, circle.center, circle.radius, circleColor)
cv.imshow("minEnclosingCircleCanvasOutput", dst);
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); circle.delete(); contoursColor.delete(); circleColor.delete();
</textarea>
<p class="err" id="minEnclosingCircleErr"></p>
</div>
<div id="minEnclosingCircleShowcase">
    <div>
        <canvas id="minEnclosingCircleCanvasInput"></canvas>
        <canvas id="minEnclosingCircleCanvasOutput"></canvas>
    </div>
    <input type="file" id="minEnclosingCircleInput" name="file" />
</div>
<script>
function minEnclosingCircleExecuteCode() {
    var minEnclosingCircleText = document.getElementById("minEnclosingCircleTestCode").value;
    try {
        eval(minEnclosingCircleText);
        document.getElementById("minEnclosingCircleErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("minEnclosingCircleErr").innerHTML = err;
    }
}

loadImageToCanvas("LinuxLogo.jpg", "minEnclosingCircleCanvasInput");
var minEnclosingCircleInputElement = document.getElementById("minEnclosingCircleInput");
minEnclosingCircleInputElement.addEventListener("change", minEnclosingCircleHandleFiles, false);
function minEnclosingCircleHandleFiles(e) {
    var minEnclosingCircleUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(minEnclosingCircleUrl, "minEnclosingCircleCanvasInput");
}
</script>
</body>
\endhtmlonly

9. Fitting an Ellipse
---------------------

Next one is to fit an ellipse to an object. It returns the rotated rectangle in which the ellipse is
inscribed.
We use the functions: **cv.fitEllipse (points)** 
@param points        input 2D point set.

**cv.ellipse1 (img, box, color, thickness = 1, lineType = cv.LINE_8)** 
@param img        image.
@param box        alternative ellipse representation via RotatedRect. This means that the function draws an ellipse inscribed in the rotated rectangle.
@param color      ellipse color.
@param thickness  thickness of the ellipse arc outline, if positive. Otherwise, this indicates that a filled ellipse sector is to be drawn.
@param lineType   type of the ellipse boundary.

Try it
------

Here is a demo. Canvas elements named fitEllipseCanvasInput and fitEllipseCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="fitEllipseCodeArea">
<h2>Input your code</h2>
<button id="fitEllipseTryIt" disabled="true" onclick="fitEllipseExecuteCode()">Try it</button><br>
<textarea rows="15" cols="90" id="fitEllipseTestCode" spellcheck="false">
var src = cv.imread("fitEllipseCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
var rotatedRect = cv.fitEllipse(contours.get(0));
var contoursColor = new cv.Scalar(255, 255, 255);
var ellipseColor = new cv.Scalar(255, 0, 0);
cv.drawContours(dst, contours, 0, contoursColor, 1, 8, hierarchy, 100, [0,0]);
cv.ellipse1(dst, rotatedRect, ellipseColor, 1, cv.LINE_8);
cv.imshow("fitEllipseCanvasOutput", dst);
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); rotatedRect.delete(); contoursColor.delete(); ellipseColor.delete();
</textarea>
<p class="err" id="fitEllipseErr"></p>
</div>
<div id="fitEllipseShowcase">
    <div>
        <canvas id="fitEllipseCanvasInput"></canvas>
        <canvas id="fitEllipseCanvasOutput"></canvas>
    </div>
    <input type="file" id="fitEllipseInput" name="file" />
</div>
<script>
function fitEllipseExecuteCode() {
    var fitEllipseText = document.getElementById("fitEllipseTestCode").value;
    try {
        eval(fitEllipseText);
        document.getElementById("fitEllipseErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("fitEllipseErr").innerHTML = err;
    }
}

loadImageToCanvas("LinuxLogo.jpg", "fitEllipseCanvasInput");
var fitEllipseInputElement = document.getElementById("fitEllipseInput");
fitEllipseInputElement.addEventListener("change", fitEllipseHandleFiles, false);
function fitEllipseHandleFiles(e) {
    var fitEllipseUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(fitEllipseUrl, "fitEllipseCanvasInput");
}
</script>
</body>
\endhtmlonly

10. Fitting a Line
------------------

Similarly we can fit a line to a set of points. We can approximate a straight line to it.

We use the functions: **cv.fitLine (points, line, distType, param, reps, aeps)** 
@param points     input 2D point set.
@param line       output line parameters. It should be a Mat of 4 elements[vx, vy, x0, y0], where [vx, vy] is a normalized vector collinear to the line and [x0, y0] is a point on the line.
@param distType   distance used by the M-estimator(see cv.DistanceTypes).
@param param      numerical parameter ( C ) for some types of distances. If it is 0, an optimal value is chosen.
@param reps       sufficient accuracy for the radius (distance between the coordinate origin and the line).
@param aeps       sufficient accuracy for the angle. 0.01 would be a good default value for reps and aeps.

**cv.line (img, pt1, pt2, color, thickness = 1, lineType = cv.LINE_8, shift = 0)** 
@param img          image.
@param pt1          first point of the line segment.
@param pt2          second point of the line segment.
@param color        line color.
@param thickness    line thickness.
@param lineType     type of the line,. 
@param shift        number of fractional bits in the point coordinates.

Try it
------

Here is a demo. Canvas elements named fitLineCanvasInput and fitLineCanvasOutput have been prepared. Choose an image and
click `Try it` to see the result. And you can change the code in the textbox to investigate more.

\htmlonly
<!DOCTYPE html>
<head>
<style>
canvas {
    border: 1px solid black;
}
</style>
</head>
<body>
<div id="fitLineCodeArea">
<h2>Input your code</h2>
<button id="fitLineTryIt" disabled="true" onclick="fitLineExecuteCode()">Try it</button><br>
<textarea rows="19" cols="90" id="fitLineTestCode" spellcheck="false">
var src = cv.imread("fitLineCanvasInput");
var dst = cv.Mat.zeros(src.rows, src.cols, cv.CV_8UC3);
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.threshold(src, src, 177, 200, cv.THRESH_BINARY);
var contours  = new cv.MatVector();
var hierarchy = new cv.Mat();
var line = new cv.Mat();
cv.findContours(src, contours, hierarchy, cv.RETR_CCOMP, cv.CHAIN_APPROX_SIMPLE, [0,0]);
// You can try more different conversion
cv.fitLine(contours.get(0), line, cv.DIST_L2, 0, 0.01, 0.01);
var contoursColor = new cv.Scalar(255, 255, 255);
var lineColor = new cv.Scalar(255, 0, 0);
cv.drawContours(dst, contours, 0, contoursColor, 1, 8, hierarchy, 100, [0,0]);
var vx= line.data32f()[0], vy = line.data32f()[1]; x = line.data32f()[2], y = line.data32f()[3];
var lefty = Math.round((-x * vy / vx) + y);
var righty = Math.round(((src.cols - x) * vy / vx) + y);
cv.line(dst, [src.cols - 1, righty], [0, lefty], lineColor, 2, cv.LINE_AA, 0);
cv.imshow("fitLineCanvasOutput", dst);
src.delete(); dst.delete(); contours.delete(); hierarchy.delete(); line.delete(); lineColor.delete(); contoursColor.delete();
</textarea>
<p class="err" id="fitLineErr"></p>
</div>
<div id="fitLineShowcase">
    <div>
        <canvas id="fitLineCanvasInput"></canvas>
        <canvas id="fitLineCanvasOutput"></canvas>
    </div>
    <input type="file" id="fitLineInput" name="file" />
</div>
<script>
function fitLineExecuteCode() {
    var fitLineText = document.getElementById("fitLineTestCode").value;
    try {
        eval(fitLineText);
        document.getElementById("fitLineErr").innerHTML = " ";
    } catch(err) {
        document.getElementById("fitLineErr").innerHTML = err;
    }
}

loadImageToCanvas("LinuxLogo.jpg", "fitLineCanvasInput");
var fitLineInputElement = document.getElementById("fitLineInput");
fitLineInputElement.addEventListener("change", fitLineHandleFiles, false);
function fitLineHandleFiles(e) {
    var fitLineUrl = URL.createObjectURL(e.target.files[0]);
    loadImageToCanvas(fitLineUrl, "fitLineCanvasInput");
}
function onReady() {
    document.getElementById("momentsTryIt").disabled = false;
    document.getElementById("areaTryIt").disabled = false;
    document.getElementById("perimeterTryIt").disabled = false;
    document.getElementById("approxPolyDPTryIt").disabled = false;
    document.getElementById("convexHullTryIt").disabled = false;
    document.getElementById("boundingRectTryIt").disabled = false;
    document.getElementById("fitEllipseTryIt").disabled = false;
    document.getElementById("minAreaRectTryIt").disabled = false;
    document.getElementById("fitLineTryIt").disabled = false;
    document.getElementById("minEnclosingCircleTryIt").disabled = false;
}
if (typeof cv !== 'undefined') {
    onReady();
} else {
    document.getElementById("opencvjs").onload = onReady;
}
</script>
</body>
\endhtmlonly