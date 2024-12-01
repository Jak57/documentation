# Moving Average Filter
**Goal:** Reducing random white noise from the audio signal.

## Pseudocode
```
prev1 = 0
prev2 = 0

def applyMovingAverageFilter(inputData):
  N = LEN(inputData)
  for i = 0 to N-1:
    cur = inputData[i]
    avg = (prev1 + prev2 + cur) / 3
    inputData[i] = (short) avg
```

## References
* https://www.analog.com/media/en/technical-documentation/dsp-book/dsp_book_ch15.pdf

  
**Prepared by**<br>
*Jakir Hasan (Reve Systems'24)*<br>
*Date (creation) - 01/12/24*<br>
*Date (last modification) - 1/12/24*<br>

**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Nafiul Alam Fuji*<br>
