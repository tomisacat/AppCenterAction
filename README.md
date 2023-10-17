# AppCenterAction
Upload artefacts to App Center by GitHub Action.

## Summary

Inspired by [wzieba/AppCeter-Github-Action](https://github.com/wzieba/AppCenter-Github-Action/), **without** limitation of executing on runners with a Linux operating system. 

## Sample usage

```
name: Build and publish to App Center

on: [push]

jobs:
  build:
    runs-on: macos-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          distribution: 'oracle'
          java-version: '17'
      - name: Build release
        run: ./gradlew assembleRelease
      - name: Upload artefact to App Center
        uses: tomisacat/AppCenterAction@main # not published to marketplace yet
        with:
          appName: tomisacat/Sample-App
          token: ${{ secrets.APP_CENTER_TOKEN }}
          group: Testers
          file: /path/to/app/build/outputs/apk/release/app-apk-name.apk
          notifyTesters: true
          debug: false
```

## Inputs

### `appName`

**Required** username followed by App name e.g. `wzieba/Sample-App`

### `token`

**Required** Upload token - you can get one from appcenter.ms/settings

### `group`

**Required** Distribution group (or multiple groups split by **;** delimiter)

### `file`

**Required** Artifact to upload (.apk or .ipa)

### `buildVersion`
Build version parameter required for .zip, .msi, .pkg and .dmg files

### buildNumber
Build number parameter required for macOS .pkg and .dmg files

### `releaseNotes`

Release notes visible on release page

### `gitReleaseNotes`

Generate release notes based on the latest git commit

### `notifyTesters`

If set to true, an email notification is sent to the distribution group

### `debug`

If set to true, shows useful debug information from the action execution.