For shifting frequency by a small amount, we use the Hilbert transformer.

## Resources
1. FREQUENCY SHIFTING FOR ACOUSTIC HOWLING SUPPRESSION (Paper - 2010)
      * https://ccrma.stanford.edu/~eberdahl/Papers/DAFx2010BerdahlHarris.pdf
      * https://ccrma.stanford.edu/~eberdahl/Projects/FreqShift/ (Demonstration - Stanford University)

2. 50 Years of Acoustic Feedback Control: State of the Art and Future Challenges (Paper - 2010)
      * https://ieeexplore.ieee.org/document/5675660 (Review paper on different methodologies for AFC)


## Finding
1. Frequency shifting works perfectly for processing time of multiples of 100ms.

2. For 60ms processing, a click sound is added after the chunks. To solve this problem, resampling is performed from 60ms to 100ms. After the frequency shifting, again resampling is performed from 100ms to 60ms using the Sinc interpolation technique, which yielded better results.
