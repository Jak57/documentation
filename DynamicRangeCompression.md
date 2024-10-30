## Dynamic Range Compression (DRC)

In real-time audio or video conferencing, a sudden increase in the participant’s voice can degrade the perceptual audio quality. To maintain a consistent voice level during audio conferencing, it is necessary to attenuate the loud voices without causing additional artifacts. The Dynamic Range Compression algorithm accomplishes this goal by compressing the audio amplitudes where the decibel level exceeds a certain threshold. Amplitudes below this threshold are left unchanged which ensures a smooth transition of voice level. <br>

In DRC the following algorithm is used for compressing the high amplitudes of audio.

<p align="center">
    <img src="images/drc_formula.png" alt="Project Logo" width="400" height="80">
</p>

Here, 
* x<sub>G</sub> indicates the decibel value corresponding to the audio sample.
  
* T indicates the threshold in the decibel full-scale (dbFS) unit. In digital audio systems, the highest dbFS value is 0 and all other values are negative. For example, -10 dbFS indicates that the decibel value is 10 units lower than the highest dbFS level (0 dbFS).
  
* R indicates the ratio (``R:1``) for performing the compression. For example, if the compression ratio is ``2:1``, then for every 2-decibel increase from the threshold value the output will be only 1-decibel increase from the threshold. Let's say that the threshold value is set to -10 dbFS, and the amplitude of an audio sample corresponds to -8 dbFS which is a 2-decibel increase from the threshold (``-8 - (-10) = 2``). So, as the ratio of compression is 2:1, the output will be increased by 1 decibel from the threshold (``-10 + 1 = -9``). In this way, all the audio samples will be compressed where decibel values exceed the thresholds resulting in a smooth transition of voice level.

* y<sub>G</sub> contains the decibel value corresponding to the compressed audio sample when the criteria of compression are met otherwise the decibel value remains unchanged.

## Amplitude to decibel and decibel to amplitude

In our case, we are working with raw audio with the following properties:

<div align="center">
    
| Property | Value | 
|:----------:|:----------:|
| Sample Rate (Hz) | 16000 | 
| Sample Size (in bits) | 16  | 
| Signed | True | 
| Big Endian | False |

</div>

As we are working with ``16-bit`` signed numbers, the highest number we can represent is ``32786``. For measuring decibel full-scale value (dbFS) from the amplitudes, they must be normalized by this upped limit at first. The formula for measuring decibel full-scale value from the amplitude value of audio sample is provided below.

