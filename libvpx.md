# Parameters of libvpx

**Reference commit: update_backend (886d652ed7312a0fb89ea55675e3bc847cb18ba6)**

## vpx_encoder.h

* Full Form:
  * cfg -> Configuration
  * svc_ctx -> Scalable Video Coding Context
---
## struct: vpx_codec_enc_cfg               (Encoder configuration structure)<br>
**Parameter List (vpx_encoder.h)** 
1. unsigned int g_usage
2. **unsigned int g_threads**   // present in update_backend
3. **unsigned int g_profile**
4. **unsigned int g_w**
5. **unsigned int g_h**
---
6. vpx_bit_depth_t g_bit_depth
7. unsigned int g_input_bit_depth
8. **struct vpx_rational g_timebase**
9. **vpx_codec_er_flags_t g_error_resilient**
10. **enum vpx_enc_pass g_pass**
---
11. **unsigned int g_lag_in_frames**
12. **unsigned int rc_dropframe_thresh**
13. unsigned int rc_resize_allowed
14. unsigned int rc_scaled_width
15. unsigned int rc_scaled_height
---
16. unsigned int rc_resize_up_thresh
17. unsigned int rc_resize_down_thresh
18. **enum vpx_rc_mode rc_end_usage**
19. vpx_fixed_buf_t rc_twopass_stats_in
20. vpx_fixed_buf_t rc_firstpass_mb_stats_in
---
21. **unsigned int rc_target_bitrate**
22. **unsigned int rc_min_quantizer**
23. **unsigned int rc_max_quantizer**
24. unsigned int rc_undershoot_pct
25. unsigned int rc_overshoot_pct
---
26. unsigned int rc_buf_sz
27. unsigned int rc_buf_initial_sz
28. unsigned int rc_buf_optimal_sz
29. unsigned int rc_2pass_vbr_bias_pct
30. **unsigned int rc_2pass_vbr_minsection_pct**
---
31. **unsigned int rc_2pass_vbr_maxsection_pct**
32. unsigned int rc_2pass_vbr_corpus_complexity
33. enum vpx_kf_mode kf_mode
34. **unsigned int kf_min_dist**
35. **unsigned int kf_max_dist**
---
36. unsigned int ss_number_layers
37. int ss_enable_auto_alt_ref[VPX_SS_MAX_LAYERS]
38. unsigned int ss_target_bitrate[VPX_SS_MAX_LAYERS]
39. unsigned int ts_number_layers
40. unsigned int ts_target_bitrate[VPX_TS_MAX_LAYERS]
---
41. unsigned int ts_rate_decimator[VPX_TS_MAX_LAYERS]
42. unsigned int ts_periodicity
43. unsigned int ts_layer_id[VPX_TS_MAX_PERIODICITY]
44. unsigned int layer_target_bitrate[VPX_MAX_LAYERS]
45. **int temporal_layering_mode**
---
46. int use_vizier_rc_params
47. vpx_rational_t active_wq_factor
48. vpx_rational_t err_per_mb_factor
49. vpx_rational_t sr_default_decay_limit
50. vpx_rational_t sr_diff_factor
---
51. vpx_rational_t kf_err_per_mb_factor
52. vpx_rational_t kf_frame_min_boost_factor
53. vpx_rational_t kf_frame_max_boost_first_factor
54. vpx_rational_t kf_frame_max_boost_subs_factor
55. vpx_rational_t kf_max_total_boost_factor
---
56. vpx_rational_t gf_max_total_boost_factor
57. vpx_rational_t gf_frame_max_boost_factor
58. vpx_rational_t zm_factor
59. vpx_rational_t rd_mult_inter_qp_fac
60. vpx_rational_t rd_mult_arf_qp_fac
61. vpx_rational_t rd_mult_key_qp_fac
---


## struct: SvcContext
**Parameter List (svc_context.h)**
1. **int spatial_layers**
2. **int temporal_layers**
3. **int temporal_layering_mode**
4. SVC_LOG_LEVEL log_level
5. int output_rc_stat
---
6. **int speed**
7. **int threads**
8. **int aqmode**
9. void *internal
---

# Description of Parameters(vpx_encoder.h)
## struct: vpx_codec_enc_cfg               (Encoder configuration structure)<br>

## Generic settings (g)
1. unsigned int g_usage
   * Deprecated: Algorithm-specific "usage" value.
   * Must be zero.

2. **unsigned int g_threads** <br>
   * For multi-threaded implementations define the maximum number of threads to use for encoding. The codec can use fewer threads than the defined one.
   * Must be selected based on the number of logical processors/cores available on the system.
   * Currently used: cfg->g_threads = 4
   * Optimal: Measure the number of threads on the system and assign the value dynamically.
     ```
     #include<unistd.h>

     unsigned int num_threads = sysconf(_SC_NPROCESSORS_ONLN)
     cfg->g_threads = num_threads
     ```
   * High resolution (1080 px): More threads need to be used for distributing the workloads.
   * Low resolution (360/480 px): Fewer threads are needed.

3. **unsigned int g_profile**
   * Selects encoding profile. Some decoder only supports specific profiles.
   * Currently used: cfg->g_profile = 0
   * For VP9: Profile = 0
      * 8-bit color depth
      * 4:2:0 chroma subsampling
   * Profile 0 has broad device compatibility
   * Profile = 1
      * 8-bit color depth
      * 4:4:4 or 4:2:2 chroma subsampling (screen share or professional video)
   * Profile = 2
      * 10/12-bit color depth
      * 4:2:0 chroma subsampling (HDR)
   * Profile = 3
      * 10/12-bit color depth
      * 4:4:4 or 4:2:2 chroma subsampling (HDR)

4. **unsigned int g_w**
   * Width of the video frame in pixels.

5. **unsigned int g_h**
   * Height of the video frame in pixels.
---
6. vpx_bit_depth_t g_bit_depth
   * Bit-depth of the codec.
   * Determines the precision of each pixel's color component and directly impacts the quality and size of the video.
   * Values:
      * VPX_BITS_8: 8 bits per color component (standard precision for most video content).
          * Sufficient for most standard-definition (SD) and high-definition (HD) content.
          * File size is small and processing is faster.
      * VPX_BITS_10: 10 bits per color component (HDR video).
      * VPX_BITS_12: 12 bits per color component (Used for high-quality video and HDR content).

7. unsigned int g_input_bit_depth
   * Bit-depth of the raw input data provided to the encoder or decoder.
   * Indicates the precision of each color component (YUV) in the raw input.

8. **struct vpx_rational g_timebase**
   * Defines the smallest unit of time (in seconds) that corresponds to one frame in a video. It is expressed as num/den.
   * num = 1, and den = FPS (frame per second)

9. vpx_codec_er_flags_t g_error_resilient
   * Configure error-resilient feature.
   * Helps in mitigating the effect of packet loss or corruption during transmission.
   * Enables the codec to encode or decode video in a way that reduces the impact of error.
   * Flags:
      * VPX_ERROR_RESILIENT_DEFAULT
         * Enables the default error-resilience settings.
         * Usually ensures that frames are independently decodable (intra-coded) to reduce dependency between frames.
      * VPX_ERROR_RESILIENT_PARTITIONS
         * Makes partitions of a single frame independently decodable.
         * Helps mitigate the impact of packet loss on the entire frame.
    * Encoding:
      * The encoder may insert additional intra-coded blocks or independently decodable frame partitions.
    * Decoder:
      * The decoder can recover from corrupted data by skipping damaged partitions or reconstructing frames using intra-coded data.

10. **enum vpx_enc_pass g_pass**
    * Multi-pass encoding mode
    * For single pass: VPX_RC_ONE_PASS
    * For multi-pass: VPX_RC_FIRST_PASS -> VPX_RC_LAST_PASS
    * VPX_RC_ONE_PASS
       * The encoder processes the video in one step and no prior analysis is performed.
       * Encoding is fast and suitable for real-time low-latency applications.
       * In terms of bitrate control and overall quality it is less efficient.
     * VPX_RC_FIRST_PASS
        * Statistical analysis is performed.
     * VPX_RC_LAST_PASS
        * For optimizing the encoding decisions, data collected from the first pass is used.
        * Encoding is slow but provides better quality at a given bitrate.

---
11. **unsigned int g_lag_in_frames**   
     * Allow lagged encoding.
     * The encoder consumes a specified number of frames (<=) and encodes the current frame for better compression efficiency.
     * This introduces latency and is not suitable for real-time communications.
     * For real-time communication cfg->g_lag_in_frames = 0

## Rate control settings (rc)
12. **unsigned int rc_dropframe_thresh**   
    * Temporal resampling configuration.
    * To meet the target data rate, temporal resampling allows the codec to drop frames.
    * The threshold is described as a percentage of the target data buffer. When the data buffer falls below this percentage of fullness, a dropped frame is indicated.
    * During scenes of high complexity dropping the frame enables the encoder to stay within bandwidth or storage constraints.
    * A higher threshold value is used for real-time applications so that bandwidth stays within limits. cfg->rc_dropframe_thresh = 30
    * To disable frame dropping the threshold is set to 0.

13. unsigned int rc_resize_allowed
    * Enable or disable spatial resampling.
    * Spatial resampling allows the codec to compress a lower-resolution version of the frame, which is then upscaled by the encoder
      to the correct presentation resolution.
    * Increased quality at a low data rate, at the expense of CUP time on the encoder/decoder.
    * Frame resizing refers to the ability of the encoder to adjust the resolution of the video frame dynamically during encoding.
    * When network conditions deteriorate bitrate can be lowered with the help of frame resizing.
    * Value:
       * 0: Frame resizing is disabled.
       * 1: Frame resizing is enabled.
    * When the actual bitrate significantly exceeds the target bitrate, the encoder resizes frames to a smaller resolution.

14. unsigned int rc_scaled_width


15. unsigned int rc_scaled_height
---
16. unsigned int rc_resize_up_thresh
17. unsigned int rc_resize_down_thresh
18. **enum vpx_rc_mode rc_end_usage**
    * Rate control algorithm to use.
    * VPX_VBR: Variable Bit Rate
       * Bit rate is adjusted dynamically to achieve a target average bit rate over time.
       * Not suitable for real-time application.
    * VPX_CBR: Constant bit rate
       * Consistent bit rate is maintained and bandwidth usage is predicted.
       * Suitable for real-time video conferencing.
    * VPX_CQ: Constant Quality
       * Ignoring bit rate control, and maintaining a consistent quality is prioritized.
    * VPX_Q: Constant Quantizer
       * Ignoring the bitrate target, a fixed quantization parameter across all frames is used. 

19. vpx_fixed_buf_t rc_twopass_stats_in
20. vpx_fixed_buf_t rc_firstpass_mb_stats_in
---
21. **unsigned int rc_target_bitrate**
    * Target bitrate that the encoder will use for this stream (kbps).
    * Currently used: cfg->rc_target_bitrate = 1500

## Quantizer settings
22. **unsigned int rc_min_quantizer**
    * In video compression quantization is the process of removing less important visual details to reduce data size.
    * Range: (0 -> lossless, highest quality, larger file size or bitrate) to (63 -> very low quality, smaller file size or bitrate)
    * rc_min_quantizer sets the floor limit on the quantizer value so that the encoder can not produce a video of excessively high quality.
    * Currently used: cfg->rc_min_quantizer = 4

23. **unsigned int rc_max_quantizer**
    * Maximum (worst quality) quantizer
    * To meet the specified target bitrate it sets the upper limit to how much the encoder can reduce the quality.
    * Range: (0, no quantization, impractical) to (63, maximum quantization)

## Bitrate tolerance
24. unsigned int rc_undershoot_pct
25. unsigned int rc_overshoot_pct
---
## Decoder buffer model parameters

26. unsigned int rc_buf_sz
27. unsigned int rc_buf_initial_sz
28. unsigned int rc_buf_optimal_sz

## 2 pass rate control parameters

29. unsigned int rc_2pass_vbr_bias_pct
30. **unsigned int rc_2pass_vbr_minsection_pct**                                
    * Two-pass mode per-GOP minimum bitrate
    * Minimum percentage of the target bitrate allocated to any given section of a video during two-pass variable bitrate (VBR) encoding.
    * Prevents sections from being starved at bitrate, ensuring minimum acceptable quality for simpler or less active scenes.
    * Ex: cfg->rc_2pass_vbr_minsection_pct = 10. The encoder will allocate 10% of the target bitrate to any section of the video regardless of the simplicity of that section.
    * Protects sections from being compressed too aggressively.

---
31. **unsigned int rc_2pass_vbr_maxsection_pct**
    * Specifies the maximum percentage of the target bitrate that can be allocated to any single section of video during two-pass variable bitrate encoding.
    * The second pass encodes the video, using the analysis of the first pass to allocate bitrate efficiently across the video.

32. unsigned int rc_2pass_vbr_corpus_complexity

## Keyframe settings (kf)

33. enum vpx_kf_mode kf_mode    
34. **unsigned int kf_min_dist**
    * Minimum distance (in frames) between consecutive keyframes (Intra-frames / I-frames)
    * Currently used: cfg->kf_min_dist = 5

35. **unsigned int kf_max_dist**
    * Maximum distance (in frames) between consecutive keyframes (Intra-frames / I-frames)
    * Currently used: cfg->kf_max_dist = 5

---
## Spatial scalability settings (ss)

36. unsigned int ss_number_layers
37. int ss_enable_auto_alt_ref[VPX_SS_MAX_LAYERS]
38. unsigned int ss_target_bitrate[VPX_SS_MAX_LAYERS]
39. unsigned int ts_number_layers
40. unsigned int ts_target_bitrate[VPX_TS_MAX_LAYERS]
---
41. unsigned int ts_rate_decimator[VPX_TS_MAX_LAYERS]
42. unsigned int ts_periodicity
43. unsigned int ts_layer_id[VPX_TS_MAX_PERIODICITY]
44. unsigned int layer_target_bitrate[VPX_MAX_LAYERS]
45. **int temporal_layering_mode**
    * Organizes frames into layers based on temporal dependencies, improving scalability, bitrate allocation, and compression efficiency.
    * Currently used: cfg->temporal_layering_mode = VP9E_TEMPORAL_LAYERING_MODE_0212 (3)
      * Layer 0: Base layer (most critical frames).
      * Layer 1: Adds additional intermediate frames.
      * Layer 2: Provides the highest frame rate or smoothest motion.
     * Based on network conditions layers can be dropped (e.g.: 30fps -> 15fps -> 7.5fps)
---
46. int use_vizier_rc_params
47. vpx_rational_t active_wq_factor
48. vpx_rational_t err_per_mb_factor
49. vpx_rational_t sr_default_decay_limit
50. vpx_rational_t sr_diff_factor
---
51. vpx_rational_t kf_err_per_mb_factor
52. vpx_rational_t kf_frame_min_boost_factor
53. vpx_rational_t kf_frame_max_boost_first_factor
54. vpx_rational_t kf_frame_max_boost_subs_factor
55. vpx_rational_t kf_max_total_boost_factor
---
56. vpx_rational_t gf_max_total_boost_factor
57. vpx_rational_t gf_frame_max_boost_factor
58. vpx_rational_t zm_factor
59. vpx_rational_t rd_mult_inter_qp_fac
60. vpx_rational_t rd_mult_arf_qp_fac
61. vpx_rational_t rd_mult_key_qp_fac
---

# Description of Parameters (svc_context.h)
## struct: SvcContext

1. **int spatial_layers**
   * Refers to the number of spatial layers in a scalable video coding configuration.
   * Video streams are encoded at multiple resolutions (e.g.: 1920x1080, 1280x720, 640x360).
   * Currently used: svc_ctx->spatial_layers = 3
      * The base layer can be decoded independently whereas higher layers can not.
  
2. **int temporal_layers**
   * The number of temporal layers used in the video encoding process.
   * Allow the encoder to create a scalable video bitstream capable of decoded at different frame rates.
       * Base layer: Encodes the essential frames for playback at a lower frame rate (e.g.: 15fps)
       * Higher layers: For achieving a higher frame rate (30/60 fps) more frames are added.
       * Layer 0 contains a keyframe and some P-frames, layer 1 adds additional P-frames that are dependent on layer 0, and lastly, layer 2 adds more P-frames that            are dependent on layer 1 frames.
   * Currently used: svc_ctx->temporal_layers = 1

3. **int temporal_layering_mode**
   * Currently used: svc_ctx->temporal_layering_mode = VP9E_TEMPORAL_LAYERING_MODE_0212

4. SVC_LOG_LEVEL log_level
   * Determines the verbosity level of logs during the Scalable Video Coding.
      * SVC_LOG_ERROR -> Logs only critical errors that prevent the encoding process from functioning correctly.
      * SVC_LOG_INFO -> Logs general informational messages about the encoding process.
      * SVC_LOG_DEBUG -> Logs detailed debugging information for in-depth analysis.

5. int output_rc_stat
   * Controls whether rate control statistics are output during the SVC encoding process.
   * To enable outputting rate control statistics assign a non-negative value to output_rc_stat.
   * Rate control statistics help analyze the bitrate allocation, frame sizes, quantizer values, and overall efficiency of the encoder's rate control mechanism.
---
6. **int speed**
   * Adjusting this parameter determines how much time the encoder spends optimizing output video for a given input.
   * Faster encoding is useful for real-time applications such as video conferencing.
   * Range:
      * 0 -> Slowest encoding, highest quality, and best compression efficiency.
      * 1-4 -> Balanced presets for encoding speed vs. quality.
      * 5-8 -> Faster encoding at the cost of reduced quality and less efficient compression.
      * >=8 -> Faster encoding speed, significantly reduced quality, and compression efficiency.

7. **int threads**
   * When multiple threads are enabled, the encoder divides the video frame into smaller sections (e.g. tiles or slices). These sections are processed independently
     across the threads reducing encoding time.
   * Range:
      * 1 -> Single-threaded encoding; slowest but deterministic behavior.
      * 2-4 -> Moderate threading; suitable for systems with 2-4 CPU cores.
      * > 4 -> Higher threading; takes advantage of multi-core CUPs for high-speed encoding.
   * The upper limit of threads depends on the number of CPU cores available in the system.

8. **int aqmode**
   * Adaptive quantization mode (aqmode) adjusts the quantization parameter for different regions of a video frame based on specific criteria.
   * Quantization is a process in video compression where continuous or large sets of values (e.g., pixel intensity values or transform coefficients) are
     approximated or mapped to smaller discrete sets of values. This reduces the precision of the data, thereby saving storage and transmission bandwidth.
   * A lower quantization parameter (QP) value results in better quality but higher bitrate whereas a higher QP value results in lower quality but reduced bitrate.
   * Values: (Value -> Mode -> Description)
       * 0 -> Off (Default) -> No adaptive quantization; the same quantization parameter is applied uniformly across the frame.
       * 1 -> Variance-Based AQ -> Adjusts QP based on spatial variance in a frame, prioritizing high-detail areas.
       * 2 -> Complexity-Based AQ -> Adjusts QP based on frame complexity, allocating more bits to areas with significant motions.
       * 3 -> Cyclic Refresh AQ -> Temporarily boosts quality in specific frame regions to improve the perceptual experience.
   * Use Cases
       * Static Scenes: ```aqmode = 0``` might be sufficient for static, low-motion content like slideshow or presentation.
       * High-Detail Scenes: ```aqmode = 1``` is ideal for scenes with significant texture or spatial detail to preserve visual quality.
       * High-Motion Content: ```aqmode = 2``` works well for sports and action scenes, where motion complexity requires adaptive bit allocation. 

9. void *internal
   * Pointer that serves as a private storage area for the implementation-specific details of scalable video coding.
   * This field encapsulates internal states and data used by the encoding process but is hidden from the public interface to maintain abstraction and modularity.


## References
1. Descriptions of parameters collected by Fuji vai.<br>
   [gilab link](https://git.iptelephony.revesoft.com/sumit/reve_vpx_wrapper/-/blob/fuji/CodecVPX.cpp)

---
**Prepared by**<br>
*Jakir Hasan (Reve Systems'24)*<br>
*Date (creation) - 19/12/24*<br>
*Date (last modification) - 26/12/24*<br>


**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Nafiul Alam Fuji*<br>




