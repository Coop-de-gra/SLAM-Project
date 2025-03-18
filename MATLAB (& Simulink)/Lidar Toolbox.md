### Where
##### https://www.mathworks.com/help/lidar/index.html

---

### What does it do?
* design, test, analyze lidar processing systems (by provided algorithms, functions, applications)
* object detection, tracking, [semantic segmentation](https://github.com/Coop-de-gra/SLAM-Project/blob/main/Ref/Vocabulary_&_Terms/semantic_segmentation.md), [shape fitting](https://github.com/Coop-de-gra/SLAM-Project/blob/main/Ref/Vocabulary_&_Terms/shape_fitting.md), lidar registartion, and obstacle detection
* provides workflows and an application for lidar-camera cross-calibraion
* set up to stream data from specific lidar manufactures but I can't imagine its that difficult to stream data from other sources
* train detection, semantic segmentation, and classification using ML and DL algos.
* provides lidar processing reference examples for perception and navigation workflows.
* supports C/C++ generation for intergrating with existing code, desktop prototypes, and deployment

### Resources?
* almost all of the resources it provides are for 3D Lidar and [cloud data](https://github.com/Coop-de-gra/SLAM-Project/blob/main/Ref/Vocabulary_&_Terms/cloud_data.md).
* however, one of the resources is for generating a [build map from 2-D lidar scan using SLAM](https://www.mathworks.com/help/lidar/ug/build-map-from-2d-lidar-scans-using-slam.html), which is right smack in the middle of our scope.

### Build Map from 2D lidar scans using SLAM
* shows how to implement SLAM algo on a series of 2D lidar scans
* uses scan processing and pose graph optimization (PGO)
* example uses offline SLAM algo
* incrementally processes recorded lidar scans and builds pose graph to map environment
* to overcome drift in robot trajectory, example uses scan matching to recognize objects and loop closure to optimize poses
* sources pose graph optimization function from the [navigation toolbox](https://www.mathworks.com/help/nav/index.html)
* repo deconstructing this example found [here](https://github.com/Coop-de-gra/SLAM-Project/blob/main/MATLAB%20(&%20Simulink)/Examples_Sandbox/Map_from_2D_Lidar_SLAM.md)

