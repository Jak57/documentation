## Useful Linux commands
* ``tar -xvzf musan.tar.gz``: untar operation
* ``du -ah --max-depth=1``: Size of files/directoires
* ``ffmpeg -f s16le -ar 48000 -ac 1 -i foreground_noise.sw -f s16le foreground_noise.pcm``: Conversion from .sw to .pcm
* ``nvidia-smi --query-gpu=index,name,memory.free --format=csv,noheader``, ``nvidia-smi``: GPU information
* ``echo $CUDA_VISIBLE_DEVICES``: Seeing GPU
* ``echo $CUDA_VISIBLE_DEVICES=0``: Setting up GPU
* ``LOGFILE="rnnoise_training.log"``, ``exec > >(tee -a "$LOGFILE") 2>&1``: Creating log file and redirecting stdout & stderr to the log file and console
* ``find sentence+word -type f -name "*.flac" -exec mv {} clean_libraries/ \;``: Finding all ``.flac`` files in a directory and moving them to a new directory
* ``find sentence+word -type f -name "*.flac" | wc -l``: Finding the number of ``.flac`` files in a folder
* ``rm -rf folder``: Removing nested folders
* ``cat file1.sw file2.sw file3.sw > output.sw``: Concatenating audio files
* ``mv noise.sw noise.pcm``: Changing he audio format
* ``cat *.pid``: Seeing process id
* ``tail -f logs/*.log``: Tailing log informations
* ``cat logs/*.log``: Seeing information of log files.

## Audio Format
* ``.pcm``: Pulse Code Modulation is a raw, uncompressed audio format.
* ``.sw``, ``.raw``, and ``.pcm`` audio formats are similar.

## Dataset from Reve Systems
**Clean Speech**
* wget --no-check-certificate https://dev.revesoft.com:27183/create_sentence+word.zip 
* wget --no-check-certificate https://dev.revesoft.com:27183/collect_sentence+word.zip 
* wget --no-check-certificate https://dev.revesoft.com:27183/create_sentence+word+phoneme.zip 
* wget --no-check-certificate https://dev.revesoft.com:27183/collect_sentence+word+phoneme.zip
* Total size: 192GB (48khz, raw/PCM format)

**Noise**
* wget --no-check-certificate https://dev.revesoft.com:27183/noise.tar.gz 
  
## Noise dataset link

| **Source**                 | **Dataset Link**                                                                                                        |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Microsoft                  | [MS-SNSD](https://github.com/microsoft/MS-SNSD)                                                                         |
| Kaggle                     | [Environmental Sound Classification 50](https://www.kaggle.com/datasets/mmoreaux/environmental-sound-classification-50) |
| Public Domain Sound        | [PDSounds](https://pdsounds.tuxfamily.org/)                                                                             |
| Google                     | [MUSAN](https://www.openslr.org/17/)                                                                                    |
| Google                     | [RIRS](https://www.openslr.org/28/)                                                                                     |

## Virtual Environment Creation
* ``apt-get install python3.10-venv`` (Virtual environment)
* ``cd /mnt/d/``
* ``mkdir rnn_dev``
* ``cd rnn_dev``
* ``python3 -m venv rnn_dev`` (Creating python virtual environment)
* ``source rnn_dev/bin/activate`` (Activating the virtual environment)

## Git Commands
* Check remote branch: ``git remote -v``
* Setting up remote origin: ``git remote set-url origin **url**``
* Changing to an already existing branch: ``git checkout rnn_train``

## Operating System
* Ubuntu (WSL)

## Prerequisite libraries for training RNNoise
* ``sudo apt-get update``
* ``sudo apt-get install autoconf automake libtool build-essential``
* ``pip3 install torch torchvision torchaudio`` (Install PyTorch with GPU suport)
* ``pip3 install tqdm``

## RNNoise training instructions
<!--* ``git clone https://github.com/xiph/rnnoise.git`` (Cloning the Git repository for RNNoise)
* ``git checkout 130913db902535a19eb477a9f4cdd5c922d63cf2``
* ``cd rnnoise``
* ``./autogen.sh``
* ``./configure``
* ``make``
* ``scripts/dump_features_parallel.sh ./dump_features speech.pcm background_noise.pcm foreground_noise.pcm features.f32 $feature_count rir_list.txt`` (Feature extraction - parallel)
* ``./dump_features -rir_list rir_list.txt speech.pcm background_noise.pcm foreground_noise.pcm features.f32 $feature_count`` (Feature extraction - not parallel)
* ``python3 torch/rnnoise/train_rnnoise.py features.f32 output_directory --batch-size $batch_size --epochs $epochs`` (Training)-->

| **Description**                                                  | **Command**                                                                                                                                         |
|------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Cloning the Git repository for RNNoise                           | ``git clone https://github.com/xiph/rnnoise.git``                                                                                                   |
| Changing directory to the RNNoise project                        | ``cd rnnoise``                                                                                                                                      |
| Checking out a specific commit of the repository                 | ``git checkout 130913db902535a19eb477a9f4cdd5c922d63cf2``                                                                                           |
| Generating the `configure` script and preparing the build system | ``./autogen.sh``                                                                                                                                    |
| Configuring the RNNoise build                                    | ``./configure``                                                                                                                                     |
| Compiling the RNNoise library                                    | ``make``                                                                                                                                            |
| Extracting features in parallel                                  | ``scripts/dump_features_parallel.sh ./dump_features speech.pcm background_noise.pcm foreground_noise.pcm features.f32 $feature_count rir_list.txt`` |
| Extracting features without parallel processing                  | ``./dump_features -rir_list rir_list.txt speech.pcm background_noise.pcm foreground_noise.pcm features.f32 $feature_count``                         |
| Training the RNNoise model using the extracted features          | ``python3 torch/rnnoise/train_rnnoise.py features.f32 output_directory --batch-size $batch_size --epochs $epochs``                                  |
| Generating the quantized version of the model                    | ``python3 torch/rnnoise/dump_rnnoise_weights.py --quantize output_directory/checkpoints/rnnoise_$epochs.pth rnnoise_c``                             |
| Moving .c file                                                   | ``mv rnnoise_c/rnnoise_data.c src/``                                                                                                                |
| Moving .h file                                                   | ``mv rnnoise_c/rnnoise_data.h src/``                                                                                                                |
| Configuring the model                                            | ``./configure``                                                                                                                                     |
| Cleaning                                                         | ``make clean``                                                                                                                                      |
| Compiling the library                                            | ``make``                                                                                                                                            |
| Performing inference                                             | ``./examples/rnnoise_demo input.raw output_new_$epochs.raw``                                                                                        |

## Resources for training RNNoise
* RNNoise wrapper: https://github.com/dbklim/RNNoise_Wrapper
* Training instructions for RNNoise_Wrapper: https://github.com/dbklim/RNNoise_Wrapper/blob/master/TRAINING.md
* RNNoise trained on MS-SNSD dataset training statistics visualization: https://github.com/xiph/rnnoise/issues/189
* Comments for training: https://github.com/xiph/rnnoise/issues/189
* 

## Useful software
* Observing memory usage: TreeSize (https://treesize.net/)
* 
   
