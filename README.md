<p align="center">
  <img src="docs/icon.png" width="96" alt="A2 mapper">
</p>

<h1 align="center">A2 mapper</h1>

<p align="center">
  Point clouds from SLAM scanner recordings.<br>
  Windows · Android · free
</p>

<p align="center">
  <a href="v4/A2-mapper-v4.exe"><b>⬇ Download for Windows (.exe)</b></a> &nbsp;·&nbsp;
  <a href="v4/A2-mapper-v4.apk"><b>⬇ Download for Android (.apk)</b></a>
</p>

<p align="center">
  <img src="docs/window-light.png" width="48%" alt="A2 mapper, light theme">
  <img src="docs/window-dark.png" width="48%" alt="A2 mapper, dark theme">
</p>

## What it is

A2 mapper builds a dense **point cloud (`.las`) and a trajectory (`.txt`)** from
raw SLAM scanner recordings. It reads:

- **ROS1 bag files** (`.bag`, uncompressed) with the topics `/livox/lidar`
  (`livox_ros_driver2/CustomMsg`) and `/livox/imu` (`sensor_msgs/Imu`) —
  recordings of **SHARE S20** SLAM scanners (Livox Mid-360);
- **`.bin` recordings** of **A2 scanners** — see the
  [OpenSLAM](https://github.com/Werdna0107/OpenSLAM) project.

### Features

- **Colour for SHARE S20 scans.** Choose the whole scan folder (the one with the
  `all_*.bag` and the `info` folder) and the point cloud comes out **coloured**
  from the scanner's cameras. Choose a single file and you get a grey cloud.
- **Control points (RTK).** The trajectory can be corrected by your control
  points, and the cloud comes out in their coordinates — see
  [below](#control-points-rtk--trajectory-correction).
- **Closed loop.** Start and finish are tied together when the recording is made
  as described [below](#getting-the-best-quality-record-a-closed-loop).
- **Private.** Everything runs on your own device. No account, no network, no
  cloud: your recordings never leave your computer or phone.

The odometry algorithm was designed and written by the author independently. It
is not derived from, and does not reuse code of, well-known open-source SLAM
implementations.

## Download

| Platform | File | Notes |
|---|---|---|
| Windows 10 / 11, x64 | [`v4/A2-mapper-v4.exe`](v4/A2-mapper-v4.exe) | one file, 1.6 MB, nothing to install |
| Android 8.0+, arm64 | [`v4/A2-mapper-v4.apk`](v4/A2-mapper-v4.apk) | the same engine on the phone (no control points) |

SHA-256 checksums: [`v4/SHA256SUMS.txt`](v4/SHA256SUMS.txt). The program is not
code-signed, so Windows SmartScreen may warn you on first start: choose
*More info → Run anyway*. On Android, allow installation from this source.

## How to use (Windows)

1. Start **A2-mapper-v4.exe**.
2. Choose the recording:
   - **File** — a `.bag` or `.bin` recording → grey point cloud;
   - **Folder** — a whole **SHARE S20 scan folder** → **coloured** point cloud;
   - or just drop a file or a folder into the window.
3. The output folder is suggested automatically. Press **Build**.
4. You get two files in the output folder: **`cloud.las`** and
   **`trajectory.txt`**.

`trajectory.txt` is plain text, one pose per line:
`x y z roll pitch yaw qx qy qz qw timestamp` (metres, radians, quaternion in
`x y z w` order, timestamp in seconds). The theme follows Windows (light or
dark).

## Getting the best quality: record a closed loop

The cloud is best when the recording is **closed**:

1. Stand still at the start for **10 seconds**.
2. Walk the whole route.
3. **Come back to exactly the same spot** you started from.
4. Stand still for **10 seconds** again, then stop the recording.

A2 mapper finds the two stationary segments and ties the finish to the start, so
the drift of the loop is closed.

## Control points (RTK) — trajectory correction

Tick **Correct by control points** and the trajectory is pulled onto your
control points. The cloud and the trajectory are then written **in the
coordinates of your points** (any metric coordinate system: local, UTM, a
national grid…). The offset between the scanner and the marked point is found
automatically.

The program looks for the points file **next to the recording**.

### SHARE S20

File name: **`ctrl_points_edit.csv`** (or `control_points.csv`), in the same
folder as the `all_*.bag`.

```csv
Name,timestamp,x,y,z
PT1,1735725777.104061985,2170499.478,312016.336,240.424
PT2,1735725849.091454401,2170487.621,311994.746,240.505
PT3,1735725916.910032466,2170468.898,312007.000,240.282
PT6,1735725968.217876747,2170476.178,311990.227,242.312
PT7,1735726039.760526743,2170490.040,312018.295,242.373
```

- `timestamp` — the moment the point was marked during the scan, in seconds, on
  the scanner's clock. SHARE saves these moments in `control_points.csv` with
  empty `x,y,z`: just fill the coordinates in and save the file as
  `ctrl_points_edit.csv`.
- `x, y, z` — coordinates of the mark in metres; they are used exactly as
  written. Rows with empty coordinates are skipped.
- At least **3** points are required; **5–7**, spread along the route, work
  best. Column names are case-insensitive, extra columns are ignored.

### A2 scanners

File name: **`scan_NNNNN.gcp.csv`**, next to `scan_NNNNN.bin`. The A2 scanner
writes it itself: the RTK position at every press of the control-point button.
Used columns: `board_real_ns`, `lat_deg`, `lon_deg`, `h_ellps_m` (optionally
`hacc_m`, `vacc_m`, `carr_soln`). Only RTK-fixed points (`carr_soln = 2`) are
used; the result is in local east-north-up metres around the first point.

## Not affiliated

A2 mapper is an independent project. SHARE, SHARE Studio and Livox are
trademarks of their owners; they are named here only to say which recordings the
program reads.

## License

Free to use, for personal and commercial work. **Reverse engineering,
decompiling, disassembling and modification are not allowed.** Full text:
[LICENSE](LICENSE).

## Links

- Telegram — https://t.me/a2blog
- YouTube — https://www.youtube.com/@a2-lab
- OpenSLAM — https://github.com/Werdna0107/OpenSLAM

<sub>#SLAM #LiDAR #PointCloud #Livox #Mid360 #SHAREslam #ROS #rosbag #LAS #Odometry #Mapping #3DScanning #Surveying #Geodesy #RTK #GNSS #GCP #ControlPoints #PointCloudProcessing #Windows #Android</sub>
