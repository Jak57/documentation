## Sampling

In digital signal processing (DSP), the process of converting a continuous analog signal to a digital signal is called **sampling**. On the other hand, the **sampling rate / frequency** refers to the number of samples per second taken from a continuous signal to create a digital signal. A higher sampling rate corresponds to more fine-grained audio at the expense of more memory usage for storing more samples per second.

In the Reve Conference project, we are working with a sample rate of 16000 Hz (16000 samples per second). However, the RNNoise audio processing module which deals with noise cancellation from audio, requires a sampling rate of 48000 Hz. To overcome this issue, we first perform upsampling from 16 kHz to 48 kHz. Then perform noise cancellation. Later we down-sampled the audio from 48 kHz to 16 kHz.

**Upsampling**: In upsampling, the sample rate is increased (Ex. 16 kHz to 48 kHz). There are some simple algorithms for performing upsampling:
* Repetition of sample value.
* Linear interpolation technique.

## Upsampling Using Linear Interpolation
Let's say the heights of two adjacent points are h<sub>1</sub> and h<sub>2</sub>. We want to convert from frequency f<sub>1</sub> to f<sub>2</sub>. So the formulas of linear interpolation are:

<div align="center">
  ratio = f<sub>2</sub> / f<sub>1</sub><br>
  delta = (h<sub>2</sub> - h<sub>1</sub>) / ratio<br>
  h<sub>pred1</sub> = h<sub>1</sub> + 1 * delta<br>
  h<sub>pred2</sub> = h<sub>1</sub> + 2 * delta<br>
  .....<br>
</div>

## Pseudocode of Upsampling Using Linear Interpolation

```
def upsample(audio, f1, f2):
  ratio = f2 / f1
  prev_height = 0
  N = LEN(audio)

  k = 0
  for i = 0 to N-1:
    cur_height = audio[i]
    delta = (cur_height - prev_height) / ratio

    for j = 0 to RATIO - 1:
      audio_upsample[k++] = audio[i] + j * delta

    prev_height = cur_height
  return audio_upsample

```

## Downsampling Using Skipping
In downsampling, the sampling rate is reduced (Ex. 48 kHz to 16 kHz). The process of downsampling is much simpler with respect to upsampling. Let's say, we want to downsample from 48 kHz to 16 kHz (Ratio = 3). Then using the skipping method, among every 3 points only the 1st point is kept, and the remaining 2 points are omitted. 

## Pseudocode of Downsampling Using Skipping
```

def downsample(upsample, f1, f2):
  ratio = f2 / f1
  N = LEN(upsample)

  k = 0
  for i = 0, i <= N-ratio, i += ratio:
    audio_downsample[k++] = upsample[i]

  return audio_downsample

```

## Real-time Example
We applied the upsampling algorithm to an [input audio of 16 kHz](audio/sampling_input.raw) and generated an [output audio fo 48 kHz](audio/sampling_output_upsample.raw). Then we performed downsampling and created a [downsampled audio of 16 kHz](audio/sampling_output_downsample.raw).










