[Collision Warning Using 2-D Lidar](https://www.mathworks.com/help/releases/R2024b/lidar/ug/collision-warning-using-2d-lidar.html)<p>

---

okay so before we start, a little new thing I learned<p>
In MATLAB: Add-Ons > Manage Add-Ons > Toolbox > (3-dots to the right) > View in... Explorer > Examples<p>
and on the right side, there is a whole entire log of examples AND THEY ARE CATEGORIZED<p>
ex. Getting Started, I/O, PreProcessing, Labeling Segmentation & Detection, Detection and Tracking, Calibration and Sensor Fusion, Navigation and Mapping<p>
like what! this is everything I need and exactly what I was touching on at the end of my last [log](https://github.com/Coop-de-gra/SLAM-Project/blob/2258c9603e8a6871cad05ce08ceb69c0f7c05d5d/MATLAB%20(%26%20Simulink)/Sandbox/Logs%40Lidar_Toolbox_GettingStarted.md) when I was shadowing someone use MATLAB for real for the first time in College @ research <p>
I'm really enjoying this<p>

---

quick note, `clc` clears Command Window<p>

---

2D collision warning use case: AGV (Automatic Guided Vehicle), etc.<p>
found in: <p>
  * Resturaunts (kitchen to table delivery)
  * Hospitals (delivering medication, linens, etc.)
  * Airports (baggage handling)
  * Distribution Centers (think Amazon)
  * Warehouses (think Amazon)
  * Manufacturing (material resuply, product lines, etc.)
  * etc.

---

goal: represent a robot in a 2D workspace with Lidar, detect obstacles, provide warning before potential impact)

---

use a Binary Occupancy Map to define the warehouse with a grid, <p>
0s or 1s represent walls or navigatable space<p>

---

new section because this is **really cool**
we can create our own warehouse layout by creating one of these occupancy maps<p>
and then, we can simulate 2D lidar sensor using a `rangeSensor` object AND COLLECT SIMULATED RANGE AND ANGLE READINGS<p>
then we use the readings and angles to generate a lidarScan object that contains the 2D scan<p>
LIKE WHAT<p>

---

before we go further, let me try to give this a shot<p>

---

starting with the [Binary Occupancy Map](https://www.mathworks.com/help/releases/R2024b/nav/ref/binaryoccupancymap.html)<p>
we can already see how to create a map<p>
```
map = binaryOccupancyMap
map = binaryOccupancyMap(width,height)
map = binaryOccupancyMap(width,height,resolution)
map = binaryOccupancyMap(rows,cols,resolution,"grid")
map = binaryOccupancyMap(p)
map = binaryOccupancyMap(p,resolution)
map = binaryOccupancyMap(sourcemap)
map = binaryOccupancyMap(sourcemap,resolution)
map = binaryOccupancyMap(___,Name=Value)
```
reading through this and trying to tie something back to what were trying to accomplish...<p>
THEY EVEN DEFINE WHAT EACH CREATION COMMAND DOES, THIS IS GOLD<p>
reading the definitions, this is what we want: `map = binaryOccupancyMap(p)` because it creates a grid from the values in matrix `p`<p>
so now we need to find how to make a matrix and define it as `p`<p>

---

looking into an example for `map = binaryOccupancyMap(p)`, we can also use a `.png` image and convert it to a map<p>
and considering defining each single spot on the map with 1 or 0, i might give that a shot.

---

okay, I couldnt find a paint app on mac so im using the factory example from the binary occupancy map example<p>

---

SICK, WE HAVE A BOM<p>

using these:<p>
```
image = imread('imageMap.png');
grayimage = rgb2gray(image);
bwimage = grayimage < 0.5;
grid = binaryOccupancyMap(bwimage);
show(grid)
```
it locates the png (make sure to put in in the current folder)<p>
converts it to grayscale, and then black and white<p>
and then creates a grid out of it, WHICH SO HAPPENS TO BE IN THE EXACT FORMAT FOR A BINARY OCCUPANCY MAP<p>

---

quick side thought<p>
back in capstone when Jordan was using a camera to capture the thickness of the filament<p>
that could have been done in a heartbeat with MATLAB, and he might have done it like that honestly<p>
everything is making so much sense<p>

---

OKAY SO, back on track<p>
the binary occupancy map it created is about 700x700 and im not sure if that will work with the example here <p>
for reference, the one in the example is about 70x70 <p>
so we have to down scale the image and re convert it to a binary occupancy map and its pretty easy<p>
`grid2 = imresize(bwimage, .125`
That converted the original matrix from 700x700 to about 80x80 which is probably good enough, but also I don't know if there was an issue in the first place<p>
alright, lets keep going<p>

---

the `rangeSensor` is pretty much a simulated Lidar<p>
it outputs range and angle measurements based on given sensor pose and occupancy map<p>
looks like creation is pretty straight foward<p>
```
rbsensor = rangeSensor
rbsensor = rangeSensor(Name,Value)
```
but... it says "Load a MAT-file containing the predefined waypoints of the AGV into the workspace" and I'm not completely sure what that means<p>
so, research time on how to make one<p>

---

okay so making a predefined waypoint deal is pretty straight foward<p>
all you have to do ts make a simple matrix<p>
```
waypoints = [
10,10;
70,10;
80,30;
]
```
and now we can create the range sensor<p>

---

first we need to create the range sensor and set its properties<p>
`rbsensor = rangeSensor`
and then we can call the object with arguments as if it were a function<p>
and here is the usage:<p>
```
[ranges,angles] = rbsensor(pose,map)
```

---
well actually this example does a way better job at explaining it<p>
you have to create the seperate values as arguments<p>
and the pass them in<p>
```
rbsensor = rangeSensor;

// Specify the pose of the sensor and the ground-truth map.
truePose = [0 0 pi/4];
trueMap = binaryOccupancyMap(eye(10));

// Generate the sensor readings.
[ranges, angles] = rbsensor(truePose, trueMap);

// Visualize the results using lidarScan.
scan = lidarScan(ranges, angles);
figure
plot(scan)
```
so lets try it!<p>

---

okay, this is what im working with:<p>
<img width="405" alt="image" src="https://github.com/user-attachments/assets/df6a6470-6f54-4cca-88f9-98f2a90b3f8e" /><p>
and I'm setting the scanner to sit at (50,10) and look East<p>
```
truePose = [50 10 0]
```
but after the output, this is what im seeing<p>
<img width="200" alt="image" src="https://github.com/user-attachments/assets/75061050-1c26-45a6-abb8-b83373213a11" /><p>
and I'm not sure its orientation or where it's located and looking at so I verify its true pose<p>
will continue investigating<p>

---

OKAY so, I ran an easier location to verify, like the flat wall at (50, 10) and this is the output:<p>
<img width="300" alt="image" src="https://github.com/user-attachments/assets/670f8fd1-9eba-4190-836c-4e5fcfe5f839" /><p>
and this means that its looking to the East but on the map its plotting it to the South<p>
which also means! that the earlier scan is correct<p>
I just had no clue that the look reach was that far<p>

---

another ex. <p>
```
truePose = [20 55 pi]
[ranges, angles] = rbsensor(truePose, grid80x80);
scan = lidarScan(ranges, angles);
figure
plot(scan)
```
and here is the output:<p>
<img width="200" alt="image" src="https://github.com/user-attachments/assets/0b735a1c-b2fb-4433-99b6-24727fe8b88f" /><p>
the only issue is that I have to rotate it 90deg counter clockwise<p>
