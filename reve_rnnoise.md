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
4. Go to the directory where you want to clone the project: ``cd /mnt/d/projects/``<br>
5. Clone the project from GitLab: ``git clone https://gitlab.xiph.org/xiph/rnnoise.git``<br>
6. Compile the project:<br>
   6.1. ``sudo apt-get install autoconf automake libtool build-essential``<br>
   6.2. ``./autogen.sh``<br>
   6.3. ``./configure``<br>
   6.4. ``make``<br>
   6.5. In the **rnnoise** directory place a audio file (.raw, 48khz)<br>
   6.6. ``./examples/rnnoise_demo fuji1_48k.raw fuji1_48k_output.raw``<br>
   6.7. Output audio file (.raw, 48khz) will be generated in the **rnnoise** folder.<br>

   
