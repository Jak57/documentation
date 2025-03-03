## Useful Linux commands
* untar operation: ``tar -xvzf musan.tar.gz``
* Size of files/directoires: ``du -ah --max-depth=1``
* Conversion from .sw to .pcm: ``ffmpeg -f s16le -ar 48000 -ac 1 -i foreground_noise.sw -f s16le foreground_noise.pcm``
* GPU information: ``nvidia-smi --query-gpu=index,name,memory.free --format=csv,noheader``
* Seeing GPU: ``echo $CUDA_VISIBLE_DEVICES``
* Setting up GPU: `` echo $CUDA_VISIBLE_DEVICES=0``


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
* ``./autogen.sh``
* ``./configure``
* ``make``

## Resources for training RNNoise
* RNNoise wrapper: https://github.com/dbklim/RNNoise_Wrapper
* Comments for training: https://github.com/xiph/rnnoise/issues/189

## Useful software
* Observing memory usage: TreeSize (https://treesize.net/)
* 
   
