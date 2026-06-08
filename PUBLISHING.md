# Avro Browser - Publishing Guide

## Prerequisites

### 1. Google Play Console Setup
- Create a Google Play Developer account
- Create an application entry in Google Play Console
- Generate a Service Account JSON key for CI/CD

### 2. GitHub Secrets
Add these secrets to your GitHub repository:
- `SIGNING_KEY`: Base64 encoded keystore file
- `KEYSTORE_PASSWORD`: Keystore password
- `KEY_ALIAS`: Key alias name
- `KEY_PASSWORD`: Key password
- `GOOGLE_PLAY_SERVICE_ACCOUNT`: Google Play service account JSON

### 3. Generate Signing Key
```bash
keytool -genkey -v -keystore avro-browser.keystore \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias avro_browser_key
```

Encode to Base64:
```bash
base64 -i avro-browser.keystore | pbcopy
```

## Publishing Process

### Automatic Publishing (via Tags)
1. Create and push a version tag:
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

2. GitHub Actions will automatically:
   - Build the APK
   - Sign the release
   - Upload to Google Play (internal track)
   - Create a GitHub release
   - Attach the signed APK

### Manual Publishing
1. Go to Actions tab
2. Select "Publish Android App" workflow
3. Click "Run workflow"
4. Monitor the build process

## Tracks and Rollout
- **internal**: Internal testing (current setup)
- **alpha**: Alpha testing
- **beta**: Beta testing
- **production**: Production release

Modify `track` in `.github/workflows/android-publish.yml` to change deployment track.

## Monitoring
- Check GitHub Actions for build logs
- Monitor Google Play Console for publishing status
- Review release notes on GitHub Releases
