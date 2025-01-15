## livpx: Finding
* If no temporal layering is used, image quality is not good compared with images with temporal layering.
* With spatial_layer = 4 and temporal_layering_mode = 2, 3, the decoded image quality is good.
* Any parameter with a ```vpx_codec_control()``` macro (e.g., VP9E_SET_*) is run-time configurable.

## Combinations of spatial layers and temporal layers
(5 spatial layers + 3 temporal layers)
* Resolution: 1920x1080, 1280x720, 960x540, 640x360, 320x180

| Spatial layers  | Temporal layers  | 
|:-------------:|:-------------:|
| 1 | 0 | 
| 1 | 1 | 
| 1 | 2 | 
| 1 | 3 |
| 2 | 0 |
| 2 | 2 |
| 2 | 3 |
| 3 | 0 | 
| 3 | 2 | 
| 3 | 3 |
| 4 | 0 |
| 4 | 2 |
| 4 | 3 |
| 5 | 0 |
| 5 | 2 |

## Experiments
Note: BW -> Bandwidth (Kbps), ET -> Encode time (Ms), R -> Resolution

### Effects of number of spatial and temporal layers on ```Bandwidth``` and ```Encode time```.
## Device 01
**Device Specifications**<br>
Processor:	Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz   2.30 GHz<br>
Installed RAM:	16.0 GB (15.8 GB usable)<br>
GPU: Yes (2)<br>
Number of threads: 8
Status: High-end device

| Spatial layer | Temporal layer | Encode Time | R4 (BW) | R3 (BW) | R2 (BW) | R1 (BW) | R0 (BW) |
|:-------------:|:---------------:|:------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| 5 | 0 | 116.139 | 207.395 | 97.378 | 45.558 | 18.166 | 5.354 |
| 5 | 2 | 119.783 | 490.51 | 225.167 | 102.817 | 36.836 | 12.9 |
| 4 | 0 | 94.933 | | 202.222 | 92.862 | 42.7 | 14.435 |
| 4 | 2 | 95.124 | | 185.903 | 86.538 | 41.448 | 14.084 |
| 4 | 3 | 79.727 | | 288.83 | 139.087 | 58.351 | 13.539 |
| 3 | 0 | 62.481 | |  | 197.299 | 86.272 | 34.61 |
| 3 | 2 | 55.370 | |  | 181.621 | 82.413 | 33.212 |
| 3 | 3 | 53.577 | |  | 285.261 | 127.452 | 33.672 |
| 2 | 0 | 50.651 | |  |  | 197.512 | 65.41 |
| 2 | 2 | 50.054 | |  |  | 180.421 | 63.393 |
| 2 | 3 | 47.507 | |  |  | 285.696 | 74.809 |
| 1 | 0 | 27.741 | |  |  |  | 220.384 |
| 1 | 1 | 34.601 | |  |  |  | 199.916 |
| 1 | 2 | 34.206 | |  |  |  | 158.044 |
| 1 | 3 | 38.408 | |  |  |  | 267.404 |


## Device 02
**Device Specifications**<br>
Processor:	Intel(R) Core(TM) i5-2450M CPU @ 2.50GHz   2.50 GHz<br>
Installed RAM:	8.0 GB (7.82 GB usable)<br>
GPU: No<br>
Number of threads: 4<br>
Status: Moderate device

| Spatial layer | Temporal layer  | Encode | R4 (BW) | R3 (BW) | R2 (BW) | R1 (BW) | R0 (BW) |
|:-------------:|:---------------:|:------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| 5             | 0               | 221.763| 207.395 | 97.378  | 45.558  | 18.166  | 5.354   |
| 5             | 2               | 254.078| 490.51  | 225.167 | 102.817 | 36.836  | 12.9    |
| 4             | 0               | 195.479|         | 202.222 | 92.862  | 42.7    | 14.435  |
| 4             | 2               | 187.877|         | 185.903 | 86.538  | 41.448  | 14.084  |
| 4             | 3               | 159.951|         | 288.83  | 139.087 | 58.351  | 13.539  |
| 3             | 0               | 132.575|         |         | 197.299 | 86.272  | 34.61   |
| 3             | 2               | 130.928|         |         | 181.621 | 82.413  | 33.212  |
| 3             | 3               | 116.951|         |         | 285.261 | 127.452 | 33.672  |
| 2             | 0               | 92.1   |         |         |         | 197.512 | 65.41   |
| 2             | 2               | 104.981|         |         |         | 180.421 | 63.393  |
| 2             | 3               | 107.04 |         |         |         | 285.696 | 74.809  |
| 1             | 0               | 140.230|         |         |         |         | 167.404 |
| 1             | 1               | 110.343|         |         |         |         | 199.916 |
| 1             | 2               | 76.715 |         |         |         |         | 158.044 |
| 1             | 3               | 122.831|         |         |         |         |  267.404|

**Findings**
If spatial layers are increased encode time increases. The effect of increasing encode time is visible in moderate and low-end devices where encode time exceeds 200ms for higher spatial layers.

## Encode time of first I-frame for varying resolutions and spatial layers
Note: The Number of temporal layers is 3.

**4 spatial layers:**
| Resolution | Width | Height | Encode Time (ms) |
| :--------: | :----:| :-----:| :-------------:  |
| 0          |   240 |   136  |      140         |
| 1          |   480 |   270  |      139         |
| 2          |   960 |   540  |      146         |
| 3          |   1920|   1080 |      138         |

**3 spatial layers:**
| Resolution | Width | Height | Encode Time (ms) |
| :--------: | :----:| :-----:| :-------------:  |
| 0          |   480 |   270  |      112         |
| 1          |   960 |   540  |      113         |
| 2          |  1920 |   1080 |      126         |

**1 spatial layer:**
| Resolution | Width | Height | Encode Time (ms) |
| :--------: | :----:| :-----:| :-------------:  |
| 0          |  1920 |  1080  |      74          |

**Findings:** Increased number of spatial layers results in larger encode time. For the first frame encode time is always larger.

## Encode time analysis for 4 spatial layers and 3 temporal layers for consecutive frames
* ET -> Encode Time (ms)
  
| Frame No. | Spatial Layer 1 (ET) | Spatial Layer 2 (ET) | Spatial Layer 3 (ET) | Spatial Layer 4 (ET) |
|:---------:|:---------------:|:---------------:|:---------------:|:---------------:|
| 1         | 78              | 94              | 112             | 141             |
| 2         | 9               | 19              | 30              | 39              |
| 3         | 30              | 26              | 30              | 52              |
| 4         | 19              | 21              | 31              | 45              |
| 5         | 52              | 26              | 37              | 45              |
| 6         | 43              | 52              | 78              | 95              |
| 7         | 7               | 18              | 42              | 42              |
| 8         | 13              | 21              | 31              | 43              |
| 9         | 7               | 15              | 29              | 40              |
| 10        | 24              | 31              | 33              | 52              |
| 11        | 44              | 54              | 71              | 100             |
| 12        | 6               | 14              | 37              | 49              |
| 13        | 11              | 17              | 32              | 40              |
| 14        | 6               | 18              | 36              | 49              |
| 15        | 24              | 22              | 37              | 48              |
| 16        | 42              | 54              | 76              | 95              |
| 17        | 7               | 14              | 30              | 46              |
| 18        | 12              | 16              | 33              | 45              |
| 19        | 8               | 15              | 37              | 43              |
| 20        | 22              | 26              | 36              | 50              |
| Average   | 23.2            | 28.65           | 44              | 58              |

**Findings:** Encode time for the first frame is always larger compared to the subsequent frames. Increasing the number of spatial layers resulted in increased encode time.

