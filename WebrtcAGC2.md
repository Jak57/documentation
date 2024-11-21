## Building Library from WebRTC Audio Processing Module (APM)

**Goal:** In this project, we generate header files (.h), a lib file (.lib), and a dll file (.dll) using Meson and Ninja from WebRTC's audio processing module.<br>
**Origin of the code base:** https://git.iptelephony.revesoft.com/mobileapps/OtherProjects/webrtc-audio-processing-1.3<br>
**Author of the original codebase:** Dhiman Paul<br>
**Tested commit:** defb4f12487c24c30acb8d32fb78f6a3f501e8c1<br>

## Tools
* Microsoft Visual Studio 2022
## Prerequisites
* C++ version: >= 17
* Architecture: x64
* Meson
* Ninja
  
  ```
  python --version
  python -m pip install --upgrade pip
  pip install meson
  pip install ninja
  ```

## Setting up the project
* clone this repository
* Open the project using Microsoft Visual Studio.
* Open the developer command prompt from the Tools section.
* Set up the environment for the x64 architecture.
  ```
  cl 
  vcvars64
  "C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
  cl
  ```
* In the windows-x86.cross file specifies the location where you want to save the library.
  ```
  prefix = 'F:/dhiman_vai_resources/webrtc-audio-processing-1.3/webrtc_apm_libs'
  ```
* Type the following commands to build the project.
  ```
  meson setup --wipe build --cross-file=cross-files/windows-x86.cross
  ninja -C build
  ninja -C build install
  ```
* Build files will be generated the specified location.
