# Security Policy

### Reporting Vulnerabilities
If you discover a security issue (e.g., intent injection risks), please report it via the `Issues` tab with the label `security`.

### Privacy & Network usage
This project is designed to run locally. However, one specific feature requires network access:

*   Circadian Engine (Sunrise/Sunset Calculation)
*   If your device fails to provide GPS coordinates (e.g. due to location being disabled), the app falls back to `http://ip-api.com/json` to determine your approximate latitude/longitude.
*   You can avoid this call by manually entering your Latitude and Longitude in the **Experiment Settings** page or directly setting `%AAB_Latitude` and `%AAB_Longitude`.

### Verification
For security, I recommend verifying the integrity of the APK before installation.
1. Download the `.prj.xml` source.
2. Search the XML for `http` to verify.
3. Compare the SHA-256 checksum of the APK against the one published in the Release notes.
