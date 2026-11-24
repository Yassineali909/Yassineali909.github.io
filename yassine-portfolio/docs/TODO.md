# To do

## Positioning (decided)

Option A: **Industrial Computer Engineer · Embedded Systems · Industrial Automation · Industrial Software**. Three flagship projects: PredictFlow, Smart Process Station (in progress), Supply Chain Intelligence Platform. Content limited to the current CV (Yassine_Ali_CV_Propper.pdf).

Not on the site unless confirmed: CODESYS, STM32, Raspberry Pi, React/Node.js, fieldbuses (SPI/I2C/CAN/LIN/PROFIBUS/PROFINET), Baccalaureate honours, CTF, phone number.

## Needed from Yassine

- [ ] **CV PDF for the website** at `portfolio/cv/Yassine_Ali_CV.pdf` — ideally a version **without the phone number**.
- [ ] **Personal email** to replace the university address (yassine.ali@enetcom.u-sfax.tn) after graduation.
- [ ] **LinkedIn URL** when the profile is ready.

- [ ] **`cnn1d_bearing_int8.tflite`** (from the VM or MinIO `mlflow-artifacts`). Fallback: `train_cnn1d.py` → model gets retrained and the site must say "reproduction".
- [ ] **The 16 CWRU `.mat` files** (Normal, IR007, B007, OR007@6 × loads 0–3 HP), as a zip.
- [x] **Full name**: Yassine Ali.
- [x] **Official logos** received (ENET'Com PNG 589×260 transparent; IPEIEM JPG 193×144, white background). A larger IPEIEM file would look sharper on the 3D plaque close-up.
  - (old note) IPEIEM and ENET'Com, SVG or high-res transparent PNG.
  - ENET'Com header logo: https://enetcom.rnu.tn/images/logo.png (check resolution; ask the school for SVG/high-res if small).
  - IPEIEM: official site www.ipeiem.rnu.tn (logo in the page header).
- [x] **Academic path** (received): IPEIEM 2021/2022 – 2022/2023, preparatory cycle, PT section; ENET'Com 2023/2024 – 2025/2026, engineering degree (Industrial Computing).
  - Decision: plaques show only school name + years; programme details are not displayed.
- [ ] Real PLC / SCADA / protocol / test tools used (to fill the `[PLACEHOLDERS]` and size the automation panel honestly).
- [ ] Email, LinkedIn, GitHub URLs and the CV PDF.

## Security & confidentiality (do these first)

- [ ] **Rotate the InfluxDB token** hard-coded in `mlops/pipeline.py` and `mlops/watcher.py`, move it to `.env`, add `.env` to `.gitignore`. If the repo is public, assume the old token is compromised.
- [ ] Ask the supervisor before publishing anything showing **Vantive** (name, logo, VPN, IPs, dashboard screenshots).
- [ ] Check IPEIEM / ENET'Com logo-usage rules.
- [ ] Update the GitHub repo so it shows the **ESP32-S3 CNN model** from the thesis, not the old Edge Impulse / ESP32-WROOM-32 export.

## Next build steps (Claude)

1. Real **Portfolio** page from `design-mockups/homepage.html` with verified facts from `PFE_FACTS.md`.
2. **Fault demo v2**: CWRU replay + real predictions, 4-class selector, spectra + fault-frequency markers, simple and engineering views.
3. Re-compose the lab around the rig; add the academic plaques once logos arrive.
4. Performance pass: AO off by default, adaptive pixel ratio, render on demand, pause when hidden.
5. OHM design sheet (front/side + 4 expressions; floating hands vs small arms).
