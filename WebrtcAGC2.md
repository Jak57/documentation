# Building Library from WebRTC Audio Processing Module (APM)

**Goal:** In this project, we generate header files (.h), a lib file (.lib), and a dll file (.dll) using Meson and Ninja from WebRTC's audio processing module.<br>
**Origin of the code base:** https://git.iptelephony.revesoft.com/mobileapps/OtherProjects/webrtc-audio-processing-1.3<br>
**Author of the original codebase:** Dhiman Paul<br>
**Tested commit:** defb4f12487c24c30acb8d32fb78f6a3f501e8c1<br>

## Tools
* Microsoft Visual Studio 2022
## Prerequisites
* C++ version: 17
* Architecture: x64
* Meson
* Ninja
* Installing Ninja and Meson:
  ```
  python --version
  python -m pip install --upgrade pip
  pip install meson
  pip install ninja
  ```

## Setting up the project
1. Clone this repository
2. Open the project using Microsoft Visual Studio.
3. Open the developer command prompt from the Tools section.
4. Set up the environment for the x64 architecture.
  ```
  cl 
  vcvars64
  "C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
  cl
  ```
5. In the windows-x86.cross file specifies the location where you want to save the library.
  ```
  prefix = 'F:/dhiman_vai_resources/webrtc-audio-processing-1.3/webrtc_apm_libs'
  ```
6. Type the following commands to build the project.
  ```
  meson setup --wipe build --cross-file=cross-files/windows-x86.cross
  ninja -C build
  ninja -C build install
  ```
7. Build files will be generated at the specified location.

**Notes**
* To build a shared library for Windows (DLL), the ``shared_library()`` statement must be provided.

---
# Testing the AGC2 form WebRTC APM using a console application
**Goal:** In this project, we test the functionality of WebRTC's AGC2 using a console application of Microsoft's Visual Studio.

## Tools
* Microsoft Visual Studio 2022

## Prerequisites
* C++ version: 17
* Architecture: x64

## Setting up the project
1. Open Microsoft Visual Studio and create a console application (ex. ```webrtc_apm_updated64```).
2. Select ```Release``` as solution configuration and ```x64``` as solution platform.
3. Add the c++ program ```webrtc_apm_updated64.cpp``` to the ```Source Files```.
4. Copy the library folder ```webrtc_apm_libs``` to the location ```\webrtc_apm_updated64\webrtc_apm_updated64```.
5. Move all the folders from ```webrtc_apm_libs\include\webrtc-audio-processing``` to ```webrtc_apm_libs\include```.
6. Put the path of header files ```C:\Users\Reve_207\source\repos\webrtc_apm_updated64\webrtc_apm_updated64\webrtc_apm_libs\include``` to the below location.<br>
   6.1. ```Properties -> Configuration Properties -> C/C++ -> Additional Include Directories```.
7. Put the path of the folder of the lib file ```C:\Users\Reve_207\source\repos\webrtc_apm_updated64\webrtc_apm_updated64\webrtc_apm_libs\lib``` to the below location.<br>
  7.1. ```Properties -> Configuration Properties -> Linker -> General -> Additional Library Directories.```
8. Put the name of the lib file ```webrtc-audio-processing.lib``` in the below location.<br>
  8.1. ```Properties -> Configuration Properties -> Linker -> Input -> Additional Dependencies```
9. Remove pre-compiled headers by going to the location below.<br>
  9.1. ```Properties -> Configuration Properties -> C/C++ -> Precompiled Headers -> Precompiled Header -> Not Using Precompiled Headers.```
10. Clean and build the solution.
11. .exe file will be generated at ```webrtc_apm_updated64\x64\Release```.
12. Place the ```input.raw``` file and the ```webrtc-audio-processing-3.dll``` from the location ```webrtc_apm_updated64\webrtc_apm_updated64\webrtc_apm_libs\bin``` to the location ```webrtc_apm_updated64\x64\Release```.
13. Execute the .exe file by running ```webrtc_apm_updated64.exe```.
14. ```ouput.raw``` file will be generated which if the AGC2 performed version of ```input.raw``` file.
