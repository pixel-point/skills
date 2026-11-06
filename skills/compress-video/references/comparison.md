# Compare quality

Run only when the user requests a quality or size comparison, including
when using `scan-and-optimize`.
Read [compression.md](compression.md) for source inspection and encoder settings.
For standalone comparisons, inspect the source first.

- Compare 2–8 codec/CRF pairs, for example VP9 at 36/42/48
  and H.265 at 24/30/36. Honor requested candidates.
  Include AV1 only when requested.
- Default to three 1-second samples centered at 25%, 50%, and 75%
  of the source duration, clamped to file bounds. Honor requested
  sample positions and durations.
- Create one shared lossless video-only FFV1/MKV reference reel using
  `trim`, `setpts`, and `concat`, with the requested crop, scale,
  and bit depth. Encode every candidate from this reference using
  the presets in [compression.md](compression.md) and `-an`.
- Measure SSIM against the reference with FFmpeg; add PSNR when
  requested. Align timestamps and corresponding frames.
  Save a preview video and matching still for each candidate.
- Estimate full-video bytes as
  `ceil(preview_bytes × source_duration / reference_duration)`.
  Label this a video-only estimate.
- Report a table of codec, CRF, SSIM, optional PSNR, and estimated
  size. Highlight the Pareto frontier. On that frontier, normalize
  SSIM and size to 0–1, favoring higher SSIM and smaller size,
  then recommend the highest average score. Treat a zero range
  as 1; break ties by smaller size, then higher SSIM.
- Report sample positions and coverage. Single or clustered samples
  provide weaker whole-video evidence.

For comparison-only requests, finish with these results.

These metrics measure encoding loss against the transformed reference;
inspect resizing separately at the intended display size. Short samples
provide estimates, not a guarantee of full-video quality or size.
