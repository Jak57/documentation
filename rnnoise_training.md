## Useful Linux commands
* untar operation: ``tar -xvzf musan.tar.gz``
* Size of files/directoires: ``du -ah --max-depth=1``
* Conversion from .sw to .pcm: ``ffmpeg -f s16le -ar 48000 -ac 1 -i foreground_noise.sw -f s16le foreground_noise.pcm``
* GPU information: ``nvidia-smi --query-gpu=index,name,memory.free --format=csv,noheader``
* Seeing GPU: ``echo $CUDA_VISIBLE_DEVICES``
* Setting up GPU: ``echo $CUDA_VISIBLE_DEVICES=0``
* Creating log file and redirecting stdout & stderr to the log file and console: ``LOGFILE="rnnoise_training.log"``, ``exec > >(tee -a "$LOGFILE") 2>&1``
* Finding all ``.flac`` files in a directory and moving them to a new directory: ``find sentence+word -type f -name "*.flac" -exec mv {} clean_libraries/ \;``
* Finding the number of ``.flac`` files in a folder: ``find sentence+word -type f -name "*.flac" | wc -l``
* Removing nested folders: ``rm -rf folder``
* Concatenating audio files: ``cat file1.sw file2.sw file3.sw > output.sw``
* Changing audio format: ``mv noise.sw noise.pcm``

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
* Microsoft: https://github.com/microsoft/MS-SNSD
* Kaggle: https://www.kaggle.com/datasets/mmoreaux/environmental-sound-classification-50
* Public Domain Sound: https://pdsounds.tuxfamily.org/
* Google (MUSAN): https://www.openslr.org/17/
* Google (RIRS): https://www.openslr.org/28/

## Git Commands
* Check remote branch: ``git remote -v``
* Setting up remote origin: ``git remote set-url origin **url**``

## RNNoise training instructions
* Cloning the Git repository for RNNoise: ``git clone https://github.com/xiph/rnnoise.git``
* ``cd rnnoise``
* ``./autogen.sh``
* ``./configure``
* ``make``

## Resources for training RNNoise
* RNNoise wrapper: https://github.com/dbklim/RNNoise_Wrapper
* Training instructions for RNNoise_Wrapper: https://github.com/dbklim/RNNoise_Wrapper/blob/master/TRAINING.md
* RNNoise trained on MS-SNSD dataset training statistics visualization: https://github.com/xiph/rnnoise/issues/189
* Comments for training: https://github.com/xiph/rnnoise/issues/189
* 

## Useful software
* Observing memory usage: TreeSize (https://treesize.net/)
* 
   
