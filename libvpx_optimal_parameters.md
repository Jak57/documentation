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
**Device Specifications**<br>
Processor:	Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz   2.30 GHz<br>
Installed RAM:	16.0 GB (15.8 GB usable)<br>
GPU: Yes (2)<br>
Status: High-end device

| Spatial layer | Temporal layer | R4 (ET) | R4 (BW) | R3 (BW) | R2 (BW) | R1 (BW) | R0 (BW) |
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
