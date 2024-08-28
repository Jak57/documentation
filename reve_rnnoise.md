# Instructions for running the RNNoise project

* [Running the RNNoise](#set-up-steps)
* [Generating the .BIN format of the model](#bin-format-of-the-model)
* [Creating .lib file](#creating-lib-file)
* [Creating .dll file](#creating-dll-file)

Link to the GitHub repository of the original project: 
``https://github.com/xiph/rnnoise``<br>
Link to the GitLab repository of the original project: 
``https://gitlab.xiph.org/xiph/rnnoise``<br>

---
## Set up steps

1. Setting up WSL (Windows Subsystem for Linux).<br>
2. Type Windows PowerShell in the search bar.<br>
   2.1. Right-click on Windows PowerShell and select **Run as administrator**.<br>
   2.2. ``wsl --install``<br>
   2.3. Restart your PC and provide a username and password.<br>
   2.4. ``sudo apt-get update``<br>
3. Go to the directory where you want to clone the project: ``cd /mnt/d/projects/``<br>
4. Clone the project from GitLab: ``git clone https://gitlab.xiph.org/xiph/rnnoise.git``<br>
5. Compile the project:<br>
   5.1. ``sudo apt-get install autoconf automake libtool build-essential``<br>
   5.2. ``./autogen.sh``<br>
   5.3. ``./configure``<br>
   5.4. ``make``<br>
   5.5. In the **rnnoise** directory place a audio file (.raw, 48khz)<br>
   5.6. ``./examples/rnnoise_demo fuji1_48k.raw fuji1_48k_output.raw``<br>
   5.7. Output audio file (.raw, 48khz) will be generated in the **rnnoise** folder.<br>

---
## bin format of the model
1. From the **rnnoise** directory type the command: ``./dump_weights_blob``<br>
2. ``weights_blob.bin`` will be generated in the ``rnnoise`` directory.<br>

---
## Creating lib file 
* Tool used: Microsoft Visual Studio 2022
## Steps
1. Create a new project
2. Search for .lib in the search bar
3. Select **Static Library**
4. Provide project name: ``rnnoiselib``
5. Select ``Create``.
  After this step, a new project will be created.
## Setting up project:
1. Remove all the existing files from the ``Header Files`` and ``Source Files`` directories.<br>
   1.1. Expand the folder.<br>
   1.2. Right-click on the file name which you want to remove.<br>
   1.3. Select ``Remove``.<br>
2. Select ``Release`` in ``Solution Configurations`` and ``x64`` in ``Solution Platforms``.
3. Right-click on ``rnnoiselib`` and select ``Properties`` -> ``Configuration Properties`` -> ``C/C++`` -> ``Precompiled Headers`` -> ``Precompiled Header`` -> ``Not Using Precompiled Headers`` -> ``Apply`` -> ``Ok``.
4. From the ``rnnoise/src`` folder copy all the ``.c``, ``.h`` and ``x64`` folder into ``rnnoiselib/rnnoiselib`` folder.
5. From the ``rnnoise/include`` directory copy the ``.h`` file into ``rnnoiselib/rnnoiselib`` directory.
6. Right-click on the ``Source Files`` directory: ``Add`` -> ``Existing Items``.
7. Select the ``.c`` files: ``celt_lpc.c denoise.c kiss_fft.c nnet.c nnet_default.c parse_lpcnet_weights.c pitch.c rnn.c rnnoise_data.c rnnoise_tables.c``
8. Right-click on the ``Header Files`` directory: ``Add`` -> ``Existing Items``.
9. Select the ``.h`` files:<br>
   9.1. ``_kiss_fft_guts.h``<br>
   9.2. ``arch.h ``<br>
   9.3. ``celt_lpc.h``<br>
   9.4. ``common.h``<br>
   9.5. ``cpu_support.h``<br>
   9.6. ``denoise.h``<br>
   9.7. ``kiss_fft.h``<br>
   9.8. ``nnet.h``<br>
   9.9. ``nnet_arch.h``<br>
   9.10. ``opus_types.h``<br>
   9.11. ``pitch.h``<br>
   9.12. ``rnn.h``<br>
   9.13. ``rnnoise_data.h``<br>
   9.14. ``rnnoise_data_little.h``<br>
   9.15. ``vec.h``<br>
   9.16. ``vec_avx.h``<br>
   9.17. ``vec_neon.h``<br>
   9.18. ``rnnoise.h``<br>
10. ``Build`` -> ``Clean Solution`` -> ``Build Solution``.
11. ``rnnoiselib.lib`` file will be generated in ``rnnoiselib/x64/Release``.

---
## Creating dll file
* Tool: Microsoft Visual Studio 2022
# Steps
1. ``Create a new project`` -> ``Dynamic-Link Library (DLL)``
2. Provide name: ``rnnoisedll`` -> ``Create``<br>
   A new project will be created. Choose ``Release`` and ``x64`` as ``Solution Configurations and Platforms``.<br>
   
   2.1. Remove all the existing files from the ``Header Files`` and ``Source Files`` directories.<br>
   2.2. Expand the folder.<br>
   2.3. Right-click on the file name which you want to remove.<br>
   2.4. Select ``Remove``.<br>
   2.5. Right-click on ``rnnoiselib`` and select ``Properties`` -> ``Configuration Properties`` -> ``C/C++`` -> ``Precompiled Headers`` -> ``Precompiled Header`` -> ``Not Using Precompiled Headers`` -> ``Apply`` -> ``Ok``.<br>
   
3. Creating ``.h`` file from the ``java`` code<br>
  3.1. Write the ``native methods``<br>
  3.2. Comment out all the ``import`` statements and ``dll`` loading statements from the ``java`` file.<br>
  3.3. In the command line terminal go to the exact location of the ``java`` file.<br>
  3.4. Run ``javac -h . RNNoise.Java``<br>
      The header file will be created in the same directory as the Java file.<br>
      
5. Copy the header file ``com_reve_audio_libraries_dlls_audio_filters_rnnoise_RNNoiseNS.h`` in the ``/rnnoisedll/rnnoisedll`` directory.
6. Right-click on the ``Header Files`` folder of ``rnnoisedll``: ``Add`` -> ``Existing Item`` -> then choose the header file.
7. Copy ``rnnoise`` folder into ``rnnoisedll/rnnoisedll``.
8. Right-click on ``rnnoisedll`` -> ``Properties`` -> ``Configuration Properties`` -> ``C/C++`` -> ``General`` -> ``Additional Include Directories`` -> ``Edit``.
   Add the following items here:<br>
   7.1. ``C:\Users\Reve_207\.jdks\corretto-17.0.11\include``<br>
   7.2. ``C:\Users\Reve_207\.jdks\corretto-17.0.11\include\win32``<br>
   7.3. ``rnnoise\include``<br>
   7.4. ``C:\Users\Reve_207\source\repos\rnnoisedll\rnnoisedll\rnnoise\src``<br>
   
9. ``Ok`` -> ``Apply`` -> ``Ok``
10. Write the C/C++ file and add it to the ``Source Files`` directory.
11. Place the ``rnnoiselib.lib`` file in ``rnnoisedll\rnnoisedll\rnnoise\x64\release``.
  
   
   
   

   
