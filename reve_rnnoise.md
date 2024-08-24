# Instructions for running the RNNoise project

Link to the GitHub repository of the original project: 
``https://github.com/xiph/rnnoise``<br>
Link to the GitLab repository of the original project: 
``https://gitlab.xiph.org/xiph/rnnoise``<br>

# Set up steps:
1. Clone the repository from GitLab.<br>
   ``https://gitlab.xiph.org/xiph/rnnoise.git``<br>
2. Type Windows PowerShell in the search bar.<br>
3. Setting up WSL (Windows Subsystem for Linux).<br>
   3.1. Right-click on Windows PowerShell and select **Run as administrator**.<br>
   3.2. ``wsl --install``<br>
   3.3. Restart your PC and provide a username and password.<br>
   3.4. ``sudo apt-get update``<br>
   3.5. Go to the directory where you cloned the project: ``cd /mnt/d/projects/rnnoise``<br>
4. Compile the project:<br>
   4.1. ``sudo apt-get install autoconf automake libtool build-essential``<br>
   
