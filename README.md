# PixelOS CertifiedPropsOverlay CI

Automated GitHub Actions CI/CD workflow to track, compile, sign, release, and distribute Google Play Integrity Certified Props overlays and JSON files for PixelOS devices.

## How It Works

1. **Version Monitoring (`check-version`)**: Checks hourly against `PixelOS-AOSP/official_devices` (`seventeen` branch) for new fingerprints in `certified_build_props.json`.
2. **Overlay Generation & Compilation (`build`)**: 
   - Dynamically translates the JSON properties into an Android Runtime Resource Overlay (`config_certifiedBuildProperties` array).
   - Compiles and links the RRO APK targeting `android` using Android SDK `aapt2`.
   - Zipaligns and signs the overlay APK with your custom platform release key (`releasekey.pk8` / `releasekey.x509.pem`).
   - Publishes GitHub Releases and artifact archives with both `certified_build_props.apk` and `certified_build_props.json`.
3. **Distribution (`push`)**: Pushes updated overlay and JSON files to target device branches (e.g. `xaga`, `spes`) in `PIXELOS-OTA`.

## Required GitHub Secrets

| Secret | Description |
| :--- | :--- |
| `RELEASE_KEY_PRIV` | Base64-encoded `releasekey.pk8` platform private key used to sign your PixelOS ROM. |
| `RELEASE_KEY_PUB` | Plain text contents of `releasekey.x509.pem` platform certificate. |
| `CERTS_TOKEN` | GitHub Personal Access Token (classic or fine-grained) with `repo` (contents write) access to push releases and update `PIXELOS-OTA`. |
