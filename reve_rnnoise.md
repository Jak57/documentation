# Instructions for running the RNNoise project

Link to the GitHub repository of the original project: 
``https://github.com/xiph/rnnoise``<br>
Link to the GitLab repository of the original project: 
``https://gitlab.xiph.org/xiph/rnnoise``<br>

# Set up steps:
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

# Generating the .BIN format of the model
1. From the **rnnoise** directory type the command: ``./dump_weights_blob``<br>
2. ``weights_blob.bin`` will be generated in the ``rnnoise`` directory.<br>
   
# Creating .lib file 
* Tool used: Microsoft Visual Studio 2022
## Steps
1. Create a new project
2. Search for .lib in the search bar
3. Select **Static Library**
4. Provide project name: ``rnnoiselib``
5. Select ``Create``.
  After this step, a new project will be created.

   
