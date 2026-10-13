# Boundary Lab — legacy Ath fork

This is a fork of [JWSound/boundary-lab](https://github.com/JWSound/boundary-lab)
that bundles the **legacy Ath executable** (`ath/ath.exe`, the build shipped before
August 2026) instead of the newer `ath202608.exe`.

## Why

The 2026-08 Ath build does not run reliably under Wine on macOS. The older build
does. This fork exists so that Boundary Lab can still generate Ath geometry on
macOS/Linux through Wine, while keeping every other upstream improvement.

## What differs from upstream

Only the Ath integration:

- `ath/ath.exe` (legacy) replaces `ath/ath202608.exe`.
- `src/blab/ath.py` drives Ath the way the old integration did:
  - it writes an `ath.cfg` with `OutputRootDir` into each run folder;
  - it runs Ath without blab mode (`-b`);
  - it reads the `ABEC_*/bem_mesh.geo` that Ath writes;
  - it takes mesh symmetry from Ath's `solving.txt`.
- The GEO is then meshed by upstream's in-process Gmsh worker, so mesh cleaning,
  symmetry reduction, quality warnings, the GUI and the CLI behave as upstream.

## Limitations

- Legacy Ath does not write the newer `//#// DRV` drive-group metadata. Radiators
  fall back to the classic `SD1D100x` naming (a single `throat` driver, or the
  dome/surround split).
- Ath config keywords added after the legacy build are not understood. Some
  bundled examples written for the newer Ath may need adjusting.
- On macOS/Linux, a `wine` executable must be on `PATH`.

## Staying up to date

```bash
git fetch upstream
git rebase upstream/main   # conflicts, if any, are expected only in src/blab/ath.py
```
