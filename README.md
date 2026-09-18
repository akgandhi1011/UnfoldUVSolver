# RotateUV Professional V3.4

Clean one-worker repository for the Rotate UV tool.

## Architecture

There is only **one external executable**:

- `RotateUV_AutoSeam.exe` - analyzes the selected mesh and proposes structural seam edges.

UV opening is performed by **3ds Max native Unfold3D** through MAXScript:

- `Unfold3DSolve()`
- `Unfold3DOptimize()`

There is no external unfold EXE, no libigl/Eigen checkout, no reference OBJ in the repository, and no seam-profile dropdown.

## Important V3.4 runtime behavior

`Generate` only analyzes the mesh and does not modify it.

`Preview` shows the proposed seam edges.

`Apply` is intentionally destructive to the current UV mapping, but it is wrapped in one Undo step. It performs a clean UV reset first, remaps the proposed **geometry-edge pairs** onto the fresh UV topology, and then replaces the Peel/Pelt seam set. This prevents old UV shell splits from surviving and making a new seam plan open like an older unwrap.

`Unfold` then uses 3ds Max Unfold3D from those explicit seams.

Workflow:

1. Generate
2. Preview
3. Apply
4. Unfold

## Supported target versions

The native Unfold3D MAXScript methods are available in 3ds Max 2022.2 and newer. This repository is intended primarily for 3ds Max 2026/2027.

## Build on GitHub

1. Create a new empty GitHub repository.
2. Upload the **contents** of this folder to the repository root.
3. Open **Actions**.
4. Select **Build RotateUV Professional V3.4**.
5. Click **Run workflow**.
6. Download the artifact named `RotateUV-Professional-V3.4-Windows`.

The artifact contains:

- `RotateUV_AutoSeam.exe`
- `Rotate_UV_PRO_NATIVE_UNFOLD_V2.ms`
- `Rotate_UV_PRO_NATIVE_UNFOLD_V2.mcr`
- `README.md`
- `CHANGES.md`

## Installation

Keep these files together in the same folder:

- `RotateUV_AutoSeam.exe`
- `Rotate_UV_PRO_NATIVE_UNFOLD_V2.ms`
- `Rotate_UV_PRO_NATIVE_UNFOLD_V2.mcr`

The script also searches `<userScripts>\RotateUVAtlas\` and `<scripts>\RotateUVAtlas\` for the EXE.

For the macro button, place the `.mcr` in the user macros folder and make sure the `.ms` is on the scripts path, or run the `.ms` once from **Scripting -> Run Script**.

## Validation performed before this repository was packaged

The AutoSeam worker was compiled independently and evaluated against the supplied Desired OBJ reference containing 8 test objects. All 8 reproduced the desired UV chart/island count. Seam counts were:

| Object | Desired | V3.4 worker |
|---|---:|---:|
| 1 | 156 | 176 |
| 2 | 27 | 31 |
| 3 | 10 | 10 |
| 4 | 18 | 16 |
| 5 | 122 | 122 |
| 6 | 72 | 72 |
| 7 | 45 | 45 |
| 8 | 46 | 40 |

The reference OBJ is **not included in this repository and is not used at runtime**. The worker uses geometry/topology, not stored reference meshes or fixed vertex counts.

Additional segment-count sweeps were run before packaging:

- solid cylinder: 8, 12, 16, 24, 32, 48, 64, 96 and 128 sides -> consistent `2N + 1` structural seams and 3 charts
- hollow tube: the same 8..128 side sweep -> consistent `4N + 4` structural seams and 4 charts
- torus: tested through 128 major segments -> one chart with the expected two fundamental cuts
- sphere: tested through 96 longitudinal segments / 48 rings -> one controlled meridian chart

These sweeps are validation only; they are **not stored as runtime files**.

The actual 3ds Max UV result cannot be executed in this build environment because 3ds Max itself is not available here. V3.4 specifically fixes the integration issue found in the earlier package by resetting the UV mapping before applying the newly generated seams.
