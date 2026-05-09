# GitHub Actions release signing

Do **not** commit Android keystore files to this repository.

The workflow in `.github/workflows/android.yml` supports release signing through GitHub Actions Secrets.

## Required repository secrets

Add these secrets under:

`Settings` → `Secrets and variables` → `Actions` → `New repository secret`

| Secret | Meaning |
| --- | --- |
| `ANDROID_KEYSTORE_BASE64` | Base64-encoded keystore file |
| `ANDROID_KEYSTORE_PASSWORD` | Keystore password |
| `ANDROID_KEY_ALIAS` | Key alias |
| `ANDROID_KEY_PASSWORD` | Key password |

## Encode the keystore

On macOS / Linux:

```bash
base64 -w 0 release.keystore > release.keystore.base64
```

If your `base64` does not support `-w`, use:

```bash
base64 release.keystore | tr -d '\n' > release.keystore.base64
```

Copy the content of `release.keystore.base64` into the `ANDROID_KEYSTORE_BASE64` secret.

## Build behavior

- If signing secrets are configured, GitHub Actions runs `gradle assembleRelease` and uploads a signed release APK artifact.
- If signing secrets are not configured, GitHub Actions runs `gradle assembleDebug` and uploads a debug APK artifact.
- When a GitHub Release is published, the workflow also uploads the APK files to that Release.

## Local signed release build

```bash
export ANDROID_KEYSTORE_FILE=/path/to/release.keystore
export ANDROID_KEYSTORE_PASSWORD='...'
export ANDROID_KEY_ALIAS='...'
export ANDROID_KEY_PASSWORD='...'
gradle assembleRelease
```
