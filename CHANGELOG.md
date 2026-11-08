# Changelog

## v4 — 2026-11-08

- **Control points (RTK):** trajectory correction by the control points of a CSV
  (SHARE S20 `ctrl_points_edit.csv`, A2 `scan_NNNNN.gcp.csv`); the cloud and the
  trajectory come out in the coordinates of the points.
- New window: modern flat design, light and dark theme, one-file executable.
- The output folder now contains only `cloud.las` and `trajectory.txt`.
- Clearer error for a damaged or unsupported file.
- Android app updated to the v4 engine, new icon (control points are not in the
  mobile app).
