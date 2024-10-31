## Audio Mixing

In real-time audio conferencing when multiple participants speak simultaneously, their voices need to be mixed and played to the receiver. Thus the received voice file contains only voices from all participants except the receiver's voice. Efficient mixing of participant voices is crucial to ensure clarity, loudness as well as smoothness of voice level. There are many algorithms for mixing audio such as:
* Summation
* Align-to-Average weighted
* Align-to-Greatest weighted
* Align-to-Weakest weighted
* Align-to-Self weighted
* Algin-to-RMS weighted

Experimentations with different audio files reveal that Align-to-RMS weighted is the optimal choice for mixing audio when working with audio chunks of duration 60ms (960 samples).

## Align-to-RMS weighted algorithm
1. All the audios are summed to generate a true mixer output. The figure below shows the formula of a true mixer output.
<p align="center">
    <img src="images/true_mixer.png" alt="Project Logo" width="220" height="80">
</p>

2. For every chunk of 960 samples the root mean square (RMS) value is calculated and stored.

3. Every audio chunk of 960 samples from each participant is collected and their RMS value is calculated and stored.

4. The ratio of RMS value from steps 3 and 2 is measured and multiplied by the true mixer output for the particular audio chunk. This generates the mixed audio.

The formula of the Align-to-RMS weighted audio mixing algorithm is provided below:

<div align="center">
    Y<sub>rms</sub> = Y<sub>true_mixer</sub> * (input<sub>rms</sub> / mixed<sub>rms</sub>)
</div>


## Pseudocode of True Mixer
    
```
def findMaxAudioLen(audios):
    maxLen = 0
    for audio in audios:
        maxLen = MAX(maxLen, LEN(audio))
    return maxLen

def trueMixer(audios):
    N = findMaxAudioLen(audios)
    M = LEN(audios)

    for i = 0 to N-1:
        sum = 0
        for j = 0 to M-1:
            sum += audios[j][i]

        mixer[i] = sum
    return mixer
```

## Pseudocode of calculating mixed<sub>rms</sub> and input<sub>rms</sub>

```
def findMixedRMS(mixer):
    N = LEN(mixer)
    M = 960
    k = 0

    for idx = 0, idx < N-1, idx += M:
        s = 0
        for i = 0 to N-1:
            s += (mixer[i + idx] * mixer[i + idx])
        denominator[k++] = SQRT(s / N)
    return denominator

def findInputRMS(audios):
    N = findMaxAudioLen(audios)
    M = LEN(audios)
    
    for i = 0 to N-1:
        s = 0
        for j = 0 to M-1:
            s += (audios[j][i] * audios[j][i])
        sumOfSqr[i] = s

    nominator = findMixedRMS(sumOfSqr)
    return nominator


```

## Pseudocode of Align-to-RMS weighted algorithm

```
def alignToRMS(audios):
    mixer = trueMixer(audios)
    nominator = findInputRMS(audios)
    denominator = findMixedRMS(mixer)

    N = LEN(mixer)
    M = 960
    k = 0

    for i = 0, i < N, i += 960:
        lambda = nominator[k] / denominator[k]
        for j = 0 to M - 1:
            mixer[i + j] *= lambda
        k++;
    return mixer

```

## References
1. A new weighted audio mixing algorithm for a multipoint processor in a VoIP conferencing system (Paper - 2014)
      * [https://ccrma.stanford.edu/~eberdahl/Papers/DAFx2010BerdahlHarris.pdf](https://ieeexplore.ieee.org/document/6968330)

2. Research on fast real-time adaptive audio mixing in multimedia conference (Paper - 2005)
      * [https://ieeexplore.ieee.org/document/5675660 (Review paper on different methodologies for AFC)](https://www.researchgate.net/publication/225398617_Research_on_fast_real-time_adaptive_audio_mixing_in_multimedia_conference) 

---
**Prepared by**<br>
*Jakir Hasan (Reve Systems'24)*<br>
*Date (creation) - 31/10/24*<br>
*Date (last modification) - 31/10/24*<br>


**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Nafiul Alam Fuji*<br>


