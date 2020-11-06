# setup-unity

<p align="left">
  <a href="https://github.com/kuler90/setup-unity/actions"><img alt="GitHub Actions status" src="https://github.com/kuler90/setup-unity/workflows/test/badge.svg"></a>
</p>

GitHub Action to download and install Unity. Based on Unity Hub.

Works on Ubuntu, macOS and Windows.

## Inputs

### `unity-version`

Unity version to install. For example, `2019.4.9f1`. Project version will be used if not provided.

### `unity-version-changeset`

Unity version changeset. For example, `50fe8a171dd9`. Automatically parsed from Unity site if not provided.

### `unity-modules`

List of Unity modules (e.g. build support) to install. For example, `[ios, android, webgl]`.

Available modules:

    Documentation: documentation
    Standard Assets: standardassets
    Example Project: example
    Android Build Support: android
    iOS Build Support: ios
    tvOS Build Support: appletv
    Linux Build Support: linux-mono
    SamsungTV Build Support: samsung
    Tizen Build Support: tizen
    WebGL Build Support: webgl
    Windows Build Support: windows
    Facebook Gameroom Build Support: facebook-games
    MonoDevelop / Unity Debugger: monodevelop
    Vuforia Augmented Reality Support: vuforia-ar
    Language packs: language-ja, language-ko, language-zh-cn, language-zh-hant, language-zh-hans
    Mac Build Support (IL2CPP): mac-il2cpp
    Windows Build Support (Mono): windows-mono
    Android SDK & NDK Tools: android-sdk-ndk-tools
    OpenJDK: android-open-jdk
    Lumin OS (Magic Leap) Build Support: lumin

Also list of available modules can be checked by execute: `<path-to-unity-hub> -- --headless help`.

### `unity-modules-child`

Automatically installs all child modules of selected modules. For example, `android-open-jdk` and `android-sdk-ndk-tools` for android. Default `true`.

### `project-path`

Path to Unity project. Used to find Unity version. Default `${{ github.workspace }}`.

### `install-path`

Path where the Unity editor will be installed.

## Outputs

### `unity-version`

Unity version.

### `unity-path`

Unity executable path. Also setted env `UNITY_PATH`.

## Known issues

 - Installing `android` module with childs modules may freeze on macOS.
 - Workflow may fail with `System.IO.IOException: No space left on device` on GitHub-hosted Ubuntu. Setting `install-path: /mnt` can fix the problem.

## Example usage

```yaml
- name: Checkout project
  uses: actions/checkout@v2

- name: Setup Unity
  uses: kuler90/setup-unity@v1
  with:
    unity-modules: android

- name: Activate Unity
  uses: kuler90/activate-unity@v1
  with:
    unity-username: ${{ secrets.UNITY_USERNAME }}
    unity-password: ${{ secrets.UNITY_PASSWORD }}
    unity-serial: ${{ secrets.UNITY_SERIAL }}

- name: Build Unity
  uses: kuler90/build-unity@v1
  with:
    build-target: Android
    build-path: ./build.apk
```