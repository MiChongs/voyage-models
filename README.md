# voyage-models

On-device models downloaded at runtime by the Voyage Android app. Files are published as release assets; the app verifies each file's SHA-256 before use.

## photocount-v1 (拍照计数 / photo counting)

| File | Model | Input | Output | Classes |
|---|---|---|---|---|
| `photocount-yoloe11s-general-v1.tflite` | YOLOE-11S (open vocabulary), text prompts fused, dynamic INT8 weights | `[1,3,640,640]` RGB 0–1 | `[1,64,8400]` | 60, see `photocount-general-classes.txt` |
| `photocount-yolo11s-dense-v1.tflite` | YOLO11S fine-tuned for dense counting | `[1,3,960,960]` RGB 0–1 | `[1,6,N]` | `rebar` (rebar end face), `pill` (tablet or capsule) |

Output rows are `cx, cy, w, h` normalized to the input size, followed by one sigmoid score per class. No NMS is embedded. Resize with non-antialiased bilinear sampling (OpenCV `INTER_LINEAR`) and letterbox with gray 114, as Ultralytics does.

Exported with Ultralytics 8.4 (LiteRT-Torch), verified with LiteRT 2.1.

### Sources and licenses

- YOLOE weights: THU-MIG/yoloe (`jameslahm/yoloe` on Hugging Face). YOLO11 and the export toolchain: Ultralytics. Both are AGPL-3.0, so these model files are distributed under AGPL-3.0.
- Dense model training data: RebarCountDataset (linghugoogle), Roboflow 100 `pills-sxdht` (CC BY 4.0), Ultralytics Medical-pills (AGPL-3.0), HealthEat pill detection (Apache-2.0), PillsAndDefectivePills (MIT).
