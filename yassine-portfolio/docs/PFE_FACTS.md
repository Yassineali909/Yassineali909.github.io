# PFE facts — source of truth for site content

Verified against the thesis (`main.pdf`) and the GitHub repo (`Yassineali909`). Anything not listed here must not appear on the site as a claim.

## Education

- **IPEIEM** — Institut Préparatoire aux Études d'Ingénieurs d'El Manar, Tunis: 2021/2022 – 2022/2023, preparatory cycle, **PT** section (official name to confirm: Physique-Technologie).
- **ENET'Com** — École Nationale d'Électronique et des Télécommunications de Sfax: 2023/2024 – 2025/2026, engineering degree, Industrial Computing (exact speciality title to confirm).

## Project

IoT predictive maintenance for rotating machines: bearing fault classification with a 1D CNN running on an ESP32-S3 (TinyML), with MQTT transport, MLflow model tracking, InfluxDB time-series storage and Zabbix supervision.

## Data

- Dataset: **CWRU Bearing Data Center**, 12 kHz, drive-end accelerometer, 0.007" faults.
- Files (16): Normal, IR007, B007, OR007@6 — each at loads 0, 1, 2, 3 HP.
- Shaft speed: 1797 / 1772 / 1750 / 1730 rpm for 0 / 1 / 2 / 3 HP.
- Windows: 1024 samples (~85 ms), 50 % overlap, single axis.
- Recorded data only: CWRU windows stored on the ESP32 and replayed; no physical sensor rig.

## Model

- 1D CNN, file `cnn1d_bearing_int8.tflite` (v1.0.2, 15,160 bytes) — **not yet provided** (see TODO).
- Preprocessing: per-window z-score `(w − µ) / (σ + 1e-6)`, then int8 quantisation (input scale 0.04815943, zero-point −2).
- Labels (LabelEncoder, alphabetical): `0 = ball, 1 = inner_race, 2 = normal, 3 = outer_race`.

## Results

- Train: loads 0–2 HP (4,486 windows). Test: **3 HP, unseen during training** (1,655 windows).
- Accuracy **99.88 %**, macro-F1 **99.79 %**, 2 errors in total. Confusion matrices: thesis fig. 5.1, p. 68.

## ESP32-S3 deployment

- Model flash: **14.8 KB**; tensor arena: **10.5 KB** (PSRAM); inference: **107 ms** at 240 MHz; **int8**.

## MQTT / dashboards

- Topics: `factory/line1/+/telemetry`, `esp32/ota/status` (payload fields in the repo README).
- Dashboards: InfluxDB, Zabbix, PredictFlow (thesis figs. 4.1, 4.6, 4.7) — **may show Vantive name/logo/internal setup: do not publish without supervisor approval.**

## Bearing fault frequencies (SKF 6205: 9 balls, d = 7.94 mm, D = 39.04 mm, 0° contact angle)

| Load | rpm | Shaft (Hz) | BPFO (Hz) | BPFI (Hz) | BSF (Hz) | FTF (Hz) |
|---|---|---|---|---|---|---|
| 0 HP | 1797 | 29.95 | 107.4 | 162.2 | 70.6 | 11.93 |
| 1 HP | 1772 | 29.53 | 105.9 | 159.9 | 69.6 | 11.76 |
| 2 HP | 1750 | 29.17 | 104.6 | 157.9 | 68.7 | 11.62 |
| 3 HP | 1730 | 28.83 | 103.4 | 156.1 | 68.0 | 11.48 |

Multipliers of shaft speed: BPFO 3.5848×, BPFI 5.4152×, BSF 2.357× (CWRU tables list 2×BSF = 4.7135×), FTF 0.39828×.

## How to present it honestly

- Headline: "Tested on an operating load the model never saw: 1,653 of 1,655 windows correct."
- CWRU is a well-known, relatively "easy" benchmark; present the result with the unseen-load test and avoid "production-ready" wording.
- Be ready for the timing question: 107 ms inference vs a new window every ~43 ms at 50 % overlap → the device cannot classify every overlapping window in real time; e.g. classify every 3rd window, or explain that overlap was a training-time augmentation.
- In the demo, show model **probabilities per class**, never "damage severity".
- Label all replayed data as recorded CWRU data, not a live sensor.
