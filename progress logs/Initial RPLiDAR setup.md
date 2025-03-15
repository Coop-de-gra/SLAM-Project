Starting fresh because XBOX 360 drivers and linux are a shit show <p>
gone for not but not forgotten <p>

---

installed drivers for the RPLiDAR (CP210x USB to UART Bridge) <p>
plugged in RPLiDAR and it was pulse spinning but now it is constant RPM spinning <p>
now I need to check that Windows is recognizing it as a COM port <p>

---

its recognized as a COM port! <p>
Under "Ports (COM & LPT)", it shows "Silicon Labs CP210x USB to UART Bridge (COM4) <p>
usually, you would use the vendors software to test the LiDAR, but it being recognized as a COM port is enough <p>

---

now I have to install a RPLIDAR SDK so I can stream data into MATLAB <p>
this [github](https://github.com/mathworks/Streamer-Slamtec-RPLIDAR-Sensor-in-MATLAB) is for just that <p>

---

Now I have to install Microsoft Visual Studio C++ 2019 for the RPLIDAR SDK, noted in this [git](https://github.com/Slamtec/rplidar_sdk) <p>

---

now following the following instruction "If you have Microsoft Visual Studio 2019 installed, just open workspaces/vc14/sdk_and_demo.sln, and compile" <p>

---

been a little while since the last entry, just trouble shooting out the ass <p>
lesson: dont follow matlab instructions closely in these circumstances, they will often lead astray <p>
better direction: read between the lines on the github README's and terminal outputs - they usually tell you exatly what you need to know <p>

---

Almost forgot that MATLAB is a walking command window <p>

---

currently getting stuck on this command:
```
>> % Select appropriate include and library paths:
includePath = '.....\rplidar_sdk-master\sdk\include';
libraryPath = '.....\rplidar_sdk-master\output\x64\Release';
compile(includePath, libraryPath)
```
because its outputting:
```
Error using mex
MEX cannot find library 'rplidar_driver', specified with the -l option.
 MEX searched for a file with one of the following names:
 librplidar_driver.lib
rplidar_driver.lib
 Verify the library name is correct. If the library is not
 on the existing path, specify the path with the -L option.
```
but like I said earlier, I just need to read between the lines<p>
...and in this case I need to point MEX to rplidar_driver<p>

---

okay so, read between the lines with chatGPT deep reasearch function becuase thats the only one that works now, and it came up with this:
```
includePath = 'C:\Users\pooc\Documents\RPLIDAR MATLAB\RPLIDAR drivers\rplidar_sdk-master\sdk\include';
libraryPath = 'C:\Users\pooc\Documents\RPLIDAR MATLAB\RPLIDAR drivers\rplidar_sdk-master\output\x64\Release';
compile(includePath, libraryPath);
```
there is an error because I dont specify the full path<p>
and it compiled with no errors because of it... <p>
note to self, try the easiest things first. One of them being, file paths <p>

---

now with this command ```help slamteclidar```, it gives the the API to connect to the RPLIDAR<p>
at the end of it, there is some  example code I can use but I have to adjust the COM port and the BAUD rate<p>
I then went into Device Manager in Windows and went down to COM Ports<p>
there i can find the RPLIDAR, see that its in COM4, and then go into it properties<p>
in the Port Settings, i can see that the bits per second (BAUD rate) is 9600<p>

---

okay so, the bits per second is not baud rate and I was led astray<p>
if we do a quick google search of "RPLIDAR A1M8 Baud Rate" the first link is [this](https://www.waveshare.com/wiki/RPLIDAR_A1#:~:text=How%20to%20Work,-The%20standard%20configuration&text=Note:%20When%20using%20the%20radar,be%20lower%20than%201.6V.)<p>
and then we go to its Specifications and see its communication interface is UART @ 115200<p>
so we can change that in the example code and we have this
```
slobj = slamteclidar("COM4", 115200);
% Start scanning
slobj.start();
% Read 10 scans
for i = 1 : 10
scan = slobj.read();
plot(scan);
pause(0.1);
end
% Stop scanning
slobj.stop();
% Close connection and destroy resources
slobj.delete();
clear slobj;
```
and it works!<p>
here is the first output!<p>
![image](https://github.com/user-attachments/assets/3c1222be-19bd-4393-8adb-2e8f18d8bad7)

---

ALSO, just found this out<p>
the file extension .m is MATLAB... DUH<p>

---

guess what else works!<p>
the 2D cloud data!!
I send the command ```demo_slamteclidar```
and I get this output!<p>
![image](https://github.com/user-attachments/assets/e4edfd8b-46ee-45e0-8aac-69ae3395e99e)


