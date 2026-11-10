# HandScope — Mobile Hand Tracking

A tiny, static, phone-friendly demonstration of monocular hand tracking. Uses your phone's selfie camera, MediaPipe Hand Landmarker for **21 joints**, and Three.js to show a touch-orbitable **3D hand skeleton**.

## Run on GitHub Pages

1. Open **Settings → Pages** in this repository.
2. Choose **Deploy from a branch**; select **main** and **/(root)**, then save.
3. Visit **https://t-tatsuya-w.github.io/demoMobileHandTracking/** on your phone.
4. Tap **Start camera**, grant permission, and hold a hand in frame.
5. With a hand detected, tap **Calibrate origin** to zero the current wrist position. Move left/right, up/down, and toward/away from the camera to see estimated displacement in centimetres.
6. Drag across the 3D panel to orbit; pinch to zoom; use **Freeze pose** to inspect the captured pose.

No build step or server needed. Camera capture needs HTTPS (provided by GitHub Pages) or localhost.

## What it measures

- 2D normalized image landmarks used to draw the live camera overlay.
- MediaPipe's **world landmarks** used for the 3D skeleton; these are *estimated hand-relative 3D positions*, not triangulated depth.
- The 3D skeleton is translated using estimated wrist location in camera coordinates. The depth estimate assumes a physical palm width (default 8 cm) and a horizontal camera FOV (default 65°). The median of several palm-bone perspective estimates is used to reduce sensitivity to finger motion.
- **Calibrate origin** records the current estimated wrist position and subtracts it from subsequent positions. This measures *relative displacement*, not a known real-world world origin. X is rightward in the mirrored selfie preview, Y upward, Z away from the camera.
- Distance estimates can drift when the hand turns, when the camera FOV differs from the setting, or when the model's 3D shape is wrong. A single camera cannot guarantee metric depth without additional assumptions or calibration.
- Front preview is mirrored like a selfie. Flip cameras via **Switch camera**.

## Performance and privacy

- One hand at a time, 640×480 requested video, ~22 max model inferences per second.
- GPU model inference attempted first; CPU fallback if unsupported.
- Tiny static site, no bundler or server API. Requires internet to download JavaScript, WASM, and the MediaPipe model on initial launch.
- Video is processed in the browser, not uploaded by this app. Camera access stops on page exit.

## Architecture

All source is currently in `index.html`. Remote dependencies: Three.js 0.170.0 and MediaPipe Tasks Vision 0.10.22. Model: Google's MediaPipe hand landmarker float16.

If the demo does not start: use a modern Chrome/Android browser, enable camera permission, ensure HTTPS and a network connection for the initial model load, and check the on-screen error message.

## Future ideas

Calibrate actual palm size and phone camera intrinsics, improve pose-aware depth solving, landmark snapshots, joint angle readouts, and benchmarking.
