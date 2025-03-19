### What
progression log entries

---

### About
[Get Started with Lidar Toolbox](https://www.mathworks.com/help/lidar/getstarted.html)

---

Verify that I have the Lidar Toolbox and Navigation toolbox installed - and I do. (Matlab > Home > Environment > Add-Ons > My Products)<p>
Navigate to Lidar toolbox > Documentation > Build Map from 2-D lidar Scans Using SLAM > Open Live Script <p>
Copy command and enter into the MATLAB commans window<p>

---

command window located at the bottom of MATLAB window...

---

```
data = load("wareHouse.mat");
scans = data.wareHouseScans;
```
writes data and scans into the 'workspace' tab to the right

---

feels like I'm just copying and pasting code and not really learning anything - going to find the working file and sift through<p>
hopefully I can reverse engineer it or at least something close<p>

---

warehouse.mat would be a great file to find - doesn't look like its in my local files<p>
going to finish the tutorial and see if anything stands out<p>

---

welp that was short lived - tried to imput a legend on the lidarscanmap and it threw a warning about legend imputs<p>
then I pulled up the legend metadata, located it in my local files, and found the wareHouse.mat file off my M.2 from there<p>
however, nothing's jumping out at me screaming useful - gotta keep looking<p>

---

ran the following code and it's erroring at unrecognized variable 'gridResolution' - I need to find its file<p>
```
loopClosureThreshold = 110;
loopClosureSearchRadius = 2;
loopClosureNumMatches = 1;
mapObjLoop = lidarscanmap(gridResolution,maxLidarRange);
for i = 1:numel(scans)
    isScanAccepted = addScan(mapObjLoop,scans{i});
    % Detect loop closure if scan is accepted
     if isScanAccepted
         [relPose,matchScanId] = detectLoopClosure(mapObjLoop, ...
             MatchThreshold=loopClosureThreshold, ...
             SearchRadius=loopClosureSearchRadius, ...
             NumMatches=loopClosureNumMatches);
         % Add loop closure to map object if relPose is estimated
         if ~isempty(relPose)
            addLoopClosure(mapObjLoop,matchScanId,i,relPose);
         end
     end
end
```

---

