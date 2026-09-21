# V3.4.3 - Professional Compact Toolbar

- Reworked ROTATE into two balanced rows: evenly spaced presets above, Custom Angle + CCW/CW below.
- Removed the in-panel `PRO MINIMAL SEAMS` and `Seams -> Max Unfold` text labels.
- Moved every existing Seam / Unfold / Optimize / Straighten command onto one compact icon row.
- Reduced rollout height while preserving all existing tool behavior and alignment-icon support.
- AutoSeam worker and UV algorithms are unchanged from V3.4.2.

# V3.4.2 - Alignment Icons

- Replaced the Bottom / Top / Left / Right text labels with the four supplied alignment icons.
- Alignment commands and all other Rotate UV behavior are unchanged.
- Added `RotateUV_AlignBounds.png` plus `RotateUV_AlignBounds_Mask.png` beside the MaxScript for the four icon buttons.

# Changes

## V3.4.1 - Align Layout + Four Shell Edge Align Tools

- Reordered the rollout to match the requested compact layout: ROTATE first, then a full-width ALIGN row.
- Kept the existing Horizontal and Vertical orientation tools unchanged.
- Added Bottom Align, Top Align, Left Align and Right Align in the four requested ALIGN positions.
- New edge-align commands translate complete selected UV shells to the shared bounding edge without changing scale, rotation, seams or topology.
- With no active UV selection, the four new align commands operate on all UV shells.
- ARRANGE ELEMENTS and SEAM / UNFOLD / STRAIGHTEN behavior is unchanged; those blocks were only moved down to make room for the new ALIGN row.
- AutoSeam worker source and seam-generation logic are unchanged from V3.4.0.

## V3.4.0 - Clean New Repository

- One external worker only: `RotateUV_AutoSeam.exe`.
- Removed the external native-unfold worker and all libigl/Eigen build dependencies.
- Removed Desired OBJ/reference-test files from the production repository.
- Removed seam-profile dropdown logic; one automatic Professional Minimal Seams policy remains.
- **Critical integration fix:** Apply now resets the current UV mapping before remapping and applying the new seam proposal.
- The worker result is retained as geometry-edge pairs so it can be mapped again after the UV reset.
- `peltEdgeSelToSeam true` replaces the previous Peel/Pelt seam set.
- Unfold uses 3ds Max native `Unfold3DSolve()` followed by `Unfold3DOptimize()`.
- Cleaned stale documentation and macro comments that still referred to a second EXE.
- GitHub Actions builds only the AutoSeam worker and packages the runtime files.

- Tightened axial cap/shoulder recognition so high-segment tubes do not change classifier paths as segment count rises.
- Removed the redundant extra global slit on hollow tubes; tube seam structure now scales consistently from 8 through 128 sides in validation.
- Simplified the AutoSeam CLI; only input/output plus optional `--verbose` remain.
