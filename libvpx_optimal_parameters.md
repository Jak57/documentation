## livpx: Finding
* If no temporal layering is used, image quality is not good compared with images with temporal layering.
* With spatial_layer = 4 and temporal_layering_mode = 2, 3, the decoded image quality is good.
* Any parameter with a ```vpx_codec_control()``` macro (e.g., VP9E_SET_*) is run-time configurable.

## Combinations of spatial layers and temporal layers
(5 spatial layers + 3 temporal layers)

| Spatial layers  | Temporal layers  | 
|:-------------:|:-------------:|
| 1 | 0 | 
| 1 | 1 | 
| 1 | 2 | 
| 1 | 3 |
| 2 | 0 |
| 2 | 1 |
| 2 | 2 |
| 2 | 3 |
| 3 | 0 | 
| 3 | 1 | 
| 3 | 2 | 
| 3 | 3 |
| 4 | 0 |
| 4 | 1 |
| 4 | 2 |
| 4 | 3 |
| 5 | 0 |
| 5 | 1 |
| 5 | 2 |

