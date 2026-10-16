# YSR fork of LivePortrait — what is different and why

Upstream: KwaiVGI/LivePortrait. This fork is used by `ysr-studies-video/.github/workflows/liveportrait.yml`.

1. **Face-only paste-back** (`flag_face_only_pasteback`, default on). Upstream pastes the whole 512px crop
   (~`scale`× the face) back over the source, so hair and background inside the crop were replaced by an upscaled
   render (soft / blocky). The mask is now the landmark hull pushed up over the forehead (`pasteback_forehead`,
   `pasteback_feather`), so everything outside the face keeps the original pixels. `src/utils/crop.py::face_mask_from_lmk`.
2. **Expression-only by default; pose transfer on a video source is refused** unless `--flag_allow_pose`.
   With a video source the hair/body come from that video; transferring head pose from the driver makes the face
   slide inside a head that does not move ("the original face is getting detached"). Measured on reel 2:
   up to 0.30 × eye width shift, yaw 0.2, roll 7° with `all`; 0.02 × eye width with `exp`.
3. **Defaults**: `source_max_dim 1920` (upstream 1280 silently downscaled 1080p+ sources), `scale 1.8` (upstream 2.3),
   `flag_crop_driving_video True`, writer crf 14 (upstream 18).

Everything else is upstream. Upstream flags still work; the YSR defaults can be overridden on the CLI.
