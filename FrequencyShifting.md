# Frequency Shifting

Audio signals are composed of sinusoids with different frequencies. In frequency shifting, the entire frequency spectrum of an audio signal is shifted upward or downward by a specified frequency shift amount. It is used in the Acoustic Feedback Loop Suppression problem. 

In the acoustic feedback loop, the sound that is played by the speaker is picked up by the microphone and reamplified again and again causing a loop. This feedback loop causes an annoying sound called howling. If it remains unhandled this howling sound can severely affect the voice quality. Frequency shifting disrupts the formation of this loop by shifting the frequencies up or down by a small amount.


## Frequency Shifting Process
1. At first the Hilbert Transformer is applied to the real-valued signal which creates a complex-valued analytical signal. The real part of the analytical signal contains the original signal and the imaginary part contains the Hilbert Transform of the original signal. The formula of the Hilbert Transform is provided below.

<p align="center">
    <img src="images/hilbert_transform.png" alt="Project Logo" width="220" height="30">
</p>

**Hilbert Transformation**<br>
* At first Fast Fourier Transformation (FFT) is applied to the original signal.
* Only positive frequencies are kept. The real and imaginary parts of the positive frequencies are multiplied by 2.
* Negative frequencies are zeroed out.
* Then the inverse FFT is applied.
* The resulting signal is called the analytical signal.

2. The analytical signal is multiplied by **e^i(2 * PI * f<sub>s</sub> * (t / f))**<br>
   Here,<br>
   * ``i`` = imaginary unit.
   * ``PI`` = 3.14159
   * **f<sub>s</sub>** = frequency shift amount
   * ``t`` = time
   * ``f`` = frequency
3. The above multiplication resulted in the shifted frequency spectrum by f<sub>s</sub> amount.
4. The real part of this signal is the final shifted version in the time domain.

**Pseudocode of Hilbert Transformation**
```
def hilbertTransformation(audio):
    N = LEN(audio)
    audioFFT[] = FFT(audio)
    for i = 0 to N/2 - 1:
        audioFFT[i] *= 2

    for i = N/2 to N - 1:
        audioFFT[i] = 0

    analyticalSignal = IFFT(audioFFT)
    return analyticalSignal
    
```

**Observations**
* When applying the frequency shifting on an audio chunk of duration 60ms (``960 samples``), some artifacts are added in the frequency-shifted audio.
* For audio chunks of duration multiples of 100ms (``1600 samples``), frequency shifting does not introduce any artifacts.
* As we process audio chunks of 60ms, at first using Sinc interpolation we generate 1600 samples from 960 samples.
* Then frequency shifting is applied to this interpolated signal and finally, the resulting signal is downsampled to 960 samples.

## Sinc Filter
For resampling Sinc filter is used which has a maximum value of 1 at index 0 and decades gradually on both ends. The formula of the Sinc filter is provided below.

<p align="center">
    <img src="images/sinc.png" alt="Project Logo" width="180" height="60">
</p>

The below figure shows the shape of the Sinc filter. 

<p align="center">
    <img src="images/sinc_shape.png" alt="Project Logo" width="500" height="200">
</p>

**Pseudocode for Sinc filter, Up and Down sampling**
```
def sinc(x):
    if x == 0:
        return 1
    return sin(PI * x) / (PI * x)


def sincUpSample(audio, targetSize):
    originalSize = LEN(audio)
    ratio = targetSize / originalSize

    for i = 0 to targetSize - 1:
        sum = 0
        position = i / ratio
        leftIndex = FLOOR(position)

        for j = leftIndex - 64 to leftIndex + 64:
            if j >= 0 and j < originalSize:
                sum += (audio[j] * sinc(position - j))
        upsample[i] = sum
    return upsample


def sincDownSample(upsample, originalSize):
    targetSize = LEN(upsample)
    ratio = targetSize / originalSize

    for i = 0 to originalSize - 1:
        sum = 0
        position = i * ratio
        leftIndex = FLOOR(position)

        for j = leftIndex - 64 to leftIndex + 64:
            if j >= 0 and j < targetSize:
                sum += (upsample[j] * sinc(position - j))
        audio[i] = sum
    return audio

```

## Pseudocode of Frequency Shifting
```
timer = 0

def frequencyShifting(audio):
    targetSize = 1600
    originalSize = LEN(audio)
    upsample = sincUpSample(audio, targetSize)
    analyticalSignal[] = hilbertTransformation(upsample)

    N = LEN(analyticalSignal)
    for i = 0 to N-1:
        analyticalSignal[i] *= (e ^ (2 * PI * fs * (timer / f))
        timer += 1

    audio = sincDownSample(upsample, originalSize)
    return audio

```
Note: For better audio quality the Dynamic Range Compression algorithm can be applied after applying frequency shifting.

## Simulation and Optimal Values of Parameters
We applied Frequency Shifting to the raw audio file sampled at 16000 Hz. Optimal values of parameters are provided below:
* f<sub>s</sub> = ``5``
* target<sub>chunk_size</sub> = ``1600``
* window<sub>size</sub>: ``129`` (-64 to +64)

## Real-time Example
With the optimal configuration, we applied the Frequency Shifting algorithm to the [input audio](audio/fs_input.raw) and generated the [output audio](audio/fs_output.raw). 

## References
1. FREQUENCY SHIFTING FOR ACOUSTIC HOWLING SUPPRESSION (Paper - 2010)
      * https://ccrma.stanford.edu/~eberdahl/Papers/DAFx2010BerdahlHarris.pdf
      * https://ccrma.stanford.edu/~eberdahl/Projects/FreqShift/ (Demonstration - Stanford University)

2. 50 Years of Acoustic Feedback Control: State of the Art and Future Challenges (Paper - 2010)
      * https://ieeexplore.ieee.org/document/5675660 (Review paper on different methodologies for AFC)

---
**Prepared by**<br>
*Jakir Hasan (Reve Systems'24)*<br>
*Date (creation) - 31/10/24*<br>
*Date (last modification) - 31/10/24*<br>


**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Nafiul Alam Fuji*<br>


