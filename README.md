# Jarvis-
Jarvis name: Build Jarvis APK

on:
  workflow_dispatch:
  push:
    branches: [ main, master ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Arrange files
        run: |
          mkdir -p app/src/main/java/com/jarvis/assistant
          mv Commands.kt JarvisService.kt MainActivity.kt app/src/main/java/com/jarvis/assistant/
          mv AndroidManifest.xml app/src/main/
          mv app_build.gradle.kts app/build.gradle.kts

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7

      - name: Build debug APK
        run: gradle assembleDebug --stacktrace

      - uses: actions/upload-artifact@v4
        with:
          name: Jarvis-APK
          path: app/build/outputs/apk/debug/app-debug.apk
