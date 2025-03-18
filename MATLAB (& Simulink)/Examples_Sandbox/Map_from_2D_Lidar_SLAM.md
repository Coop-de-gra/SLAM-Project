### What
* estimate robot trajectory from a series of scans. optimize drift in trajectory. visualize map using scans and pose optimization
* example uses data previously collected from a Jackal robot with a SICK TiM511 Lidar

### Code Deconstruction

---

```
data = load("wareHouse.mat");
scans = data.wareHouseScans;
```
  * `warehouse.mat` is a Matlab file 
