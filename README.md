# A2 SLAM Player

A portable LiDAR point cloud viewer that "builds" the scene over time — the cloud grows in the exact order the scanner captured it, with a cinematic camera fly-through in first- or third-person view. Built for a handheld SLAM scanner based on the Livox Mid-360.

Supports coloring by height, intensity (with selectable palette), and true color (RGB).

![demo](docs/demo.gif)

> 📺 Functionality demo on YouTube Shorts: **[link]**

---

## Download

**[⬇ Download A2 SLAM Player](https://drive.google.com/your-link-here)**

This is a **portable application** — no installation required.

1. Download the archive from the link above.
2. Extract the whole `SlamPlayer` folder anywhere you like.
3. Run `SlamPlayer.exe` from that folder.

> Run the app **from the complete folder** — all required libraries sit next to the exe. Move/copy the entire folder, not the single file.

OS: Windows.

---

## Input data

You need files from a single scan:

| File | What it is |
|------|------------|
| **LAS** | point cloud (geometry). Can be colored or not. Must contain a `gps_time` field |
| **trajectory** | scanner trajectory text file (format below) |
| **bag** | original ROS1 bag with the `/livox/lidar` topic. Needed **only** if the timestamps in the LAS are not correct |

### Trajectory file format

Plain text file, one pose per line. Columns are space-separated:

```
#x y z roll pitch yaw qx qy qz qw timestamp
-0.001865105370326 -0.001197191466079 0.003077496898486 0.126456236323594 -1.129065277511176 -0.081384494349920 0.031616080125796 -0.535681553317742 -0.000522370420903 0.843827840285987 1779542012.653738737106323
-0.005390107823818 0.000342167068417 0.001297772216864 0.128775394382585 -1.128230074471805 -0.083837126087330 0.031966718942177 -0.535370157459312 -0.000967982925936 0.844011840196047 1779542012.755607843399048
```

Where: `x y z` — position, `roll pitch yaw` — angles, `qx qy qz qw` — orientation quaternion, `timestamp` — pose unix time. A header line starting with `#` is ignored.

---

## How to use

### Step 1. Load and check

1. Pick the **LAS** and **trajectory** files (the `⋯` buttons).
2. Click **Check data**. The app inspects the timestamps in the LAS:
   - **"DATA OK"** — timestamps are correct. No bag needed, you can play right away.
   - **"TIME BROKEN"** — timestamps are not correct. A **bag** field and a **PREPARE** button appear to restore them.

### Step 2. PREPARE (only if timestamps are not correct)

Click **PREPARE**. Under the hood, entirely in RAM (no intermediate files on disk):
1. the bag is read and per-point honest timestamps are built along the trajectory;
2. those timestamps are transferred onto the LAS geometry (nearest-neighbor, KD-tree);
3. the result is sorted by time.

A progress bar shows bag reading and time transfer. When done, the Play button becomes active.

### Step 3. Playback

- **Play / Pause** — start/pause the build. Points appear in ascending time order — the cloud "builds up" the way the scanner captured it.
- **Restart** — start the build again from an empty scene.

---

## Modes and controls

### Camera
- **free** — free orbit with the mouse (rotate, wheel to zoom — wheel toward you zooms in, CAD/RViz convention).
- **follow** — camera flies behind the scanner along the trajectory, looking ahead down the route (third-person view).
- **first person** — first-person view at human eye height, wide far-reaching field of view.

The camera follows the **trajectory** (the smooth real path of the scanner), so the motion is calm and jitter-free.

### Color
- **height** — colored by height (turbo palette: low red → high blue).
- **intensity** — by reflection intensity, with a selectable palette (see below).
- **rgb** — true point color if the LAS is colored. If there is no color in the file, falls back to height.

### Intensity gradient
Active only in **intensity** mode. Three full smooth gradients across the whole range:
- **gray** — black → white;
- **turbo** — red → orange → yellow → green → blue;
- **jet** — blue → cyan → green → yellow → red.

### Other
- **point size** (0.5–4.0) — point size, changes live. Small points (~1.0) give a "sand-grain" look like ReCap.
- **speed (frames per tick)** (1–200) — build speed. Low values = slow cinematic reveal; high values = fast pass over the whole scene.

---

## Compatibility

Tested on **Share S20** scanner data. Not tested with other manufacturers' scanners — correct operation is not guaranteed.

---

## License

MIT License

Copyright (c) 2026 A2

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Author

A2 — proprietary handheld SLAM LiDAR scanners.
