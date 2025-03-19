## What
* estimate robot trajectory from a series of scans. optimize drift in trajectory. visualize map using scans and pose optimization
* example uses data previously collected from a Jackal robot with a SICK TiM511 Lidar

---

## Progression Logs
* [test]

---

## Code Deconstruction

Load lidar data

```
data = load("wareHouse.mat");
scans = data.wareHouseScans;
```
  * `warehouse.mat` is a Matlab data file used to store data in a structured, Binary format
    * in this lens, lidar scans contain large amounts of data, especially if they are 3D cloud point
    * it is assigned to the variable `data` and `load`ed in
  *  `data.wareHouseScans` is used to access the wareHouseScans from the data variable and assign it to `scans`
    *  I'm assuming it is "order of operations" to load the data first and then call it scan data for the workspace

---
Estimate Robot Trajectory

```
maxLidarRange = 8;
gridResolution = 20;
mapObj = lidarscanmap(gridResolution,maxLidarRange);
```
* 
