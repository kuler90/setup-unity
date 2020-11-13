# setup-unity

<p align="left">
  <a href="https://github.com/kuler90/setup-unity/actions"><img alt="GitHub Actions status" src="https://github.com/kuler90/setup-unity/workflows/test%20ubuntu/badge.svg?branch=master"></a>
  <a href="https://github.com/kuler90/setup-unity/actions"><img alt="GitHub Actions status" src="https://github.com/kuler90/setup-unity/workflows/test%20macos/badge.svg?branch=master"></a>
  <a href="https://github.com/kuler90/setup-unity/actions"><img alt="GitHub Actions status" src="https://github.com/kuler90/setup-unity/workflows/test%20windows/badge.svg?branch=master"></a>
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

Available modules can be found in test workflows ([test-ubuntu](https://github.com/kuler90/setup-unity/blob/master/.github/workflows/test-ubuntu.yml), [test-macos](https://github.com/kuler90/setup-unity/blob/master/.github/workflows/test-macos.yml), [test-windows](https://github.com/kuler90/setup-unity/blob/master/.github/workflows/test-windows.yml)). 

Also list of available modules can be found by execute `<unity-hub> -- --headless help` but result may contains wrong names.

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

 - Installing `android` module with childs modules may freeze on macOS. Recommended to use with [`timeout-minutes`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepstimeout-minutes).
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
