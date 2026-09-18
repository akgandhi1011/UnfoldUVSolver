# Changes

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
