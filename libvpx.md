# Parameters of libvpx

**Reference commit: update_backend (886d652ed7312a0fb89ea55675e3bc847cb18ba6)**

## vpx_encoder.h

* Full Form:
  * cfg -> Configuration
  * svc_ctx -> Scalable Video Coding Context
---
**struct: vpx_codec_enc_cfg**               (Encoder configuration structure)<br>
**Parameter List** 
1. unsigned int g_usage
2. unsigned int g_threads
3. unsigned int g_profile
4. unsigned int g_w
5. unsigned int g_h
---
6. vpx_bit_depth_t g_bit_depth
7. unsigned int g_input_bit_depth
8. struct vpx_rational g_timebase
9. vpx_codec_er_flags_t g_error_resilient
10. enum vpx_enc_pass g_pass
---
11. unsigned int g_lag_in_frames
12. unsigned int rc_dropframe_thresh
13. unsigned int rc_resize_allowed
14. unsigned int rc_scaled_width
15. unsigned int rc_scaled_height
---
16. unsigned int rc_resize_up_thresh
17. unsigned int rc_resize_down_thresh
18. enum vpx_rc_mode rc_end_usage
19. vpx_fixed_buf_t rc_twopass_stats_in
20. vpx_fixed_buf_t rc_firstpass_mb_stats_in
---
21. unsigned int rc_target_bitrate
22. unsigned int rc_min_quantizer
23. unsigned int rc_max_quantizer
24. unsigned int rc_undershoot_pct
25. unsigned int rc_overshoot_pct
---
26. unsigned int rc_buf_sz
27. unsigned int rc_buf_initial_sz
28. unsigned int rc_buf_optimal_sz
29. unsigned int rc_2pass_vbr_bias_pct
30. unsigned int rc_2pass_vbr_minsection_pct
---
31. unsigned int rc_2pass_vbr_maxsection_pct
32. unsigned int rc_2pass_vbr_corpus_complexity
33. enum vpx_kf_mode kf_mode
34. unsigned int kf_min_dist
35. unsigned int kf_max_dist
---
36. unsigned int ss_number_layers
37. int ss_enable_auto_alt_ref[VPX_SS_MAX_LAYERS]
38. unsigned int ss_target_bitrate[VPX_SS_MAX_LAYERS]
39. unsigned int ts_number_layers
40. unsigned int ts_target_bitrate[VPX_TS_MAX_LAYERS]

41. unsigned int ts_rate_decimator[VPX_TS_MAX_LAYERS]
42. unsigned int ts_periodicity
43. unsigned int ts_layer_id[VPX_TS_MAX_PERIODICITY]
44. unsigned int layer_target_bitrate[VPX_MAX_LAYERS]
45. int temporal_layering_mode

46. int use_vizier_rc_params
47. 
























