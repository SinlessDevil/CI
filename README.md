# ☁️ Unity CI GitHub Action Integration

Automate Unity Android builds using GitHub Actions and Cake. Save time, avoid repetitive setup, and focus on game development.

---

## 🔀 Supported Versions

<details>
<summary>🔹 <strong>v1.0.0 — game-ci (Unity Personal License)</strong></summary>

Uses [game-ci/unity-builder@v4](https://github.com/game-ci/unity-builder). Works with Unity Personal.

### ⚙️ Setup Instructions

1. Import `ci-github-action-v1.0.0.unitypackage` into your project.
2. Configure `Assets/Plugins/CI/CIConfig.asset`.
3. Set the following GitHub secrets:

| Secret Name      | Description                      |
| ---------------- | -------------------------------- |
| `UNITY_EMAIL`    | Unity account email              |
| `UNITY_PASSWORD` | Unity account password           |
| `UNITY_LICENSE`  | Content of base64 `.ulf` license |

### 🔐 How to get your Unity license file:

1. Open Unity Hub on your local machine.
2. Go to `Preferences → Licenses → Manual Activation`.
3. Follow the instructions and locate the `.ulf` file after activation:

   * **Windows**: `C:\ProgramData\Unity\Unity_lic.ulf`
   * **macOS**: `/Library/Application Support/Unity/Unity_lic.ulf`
4. Open the file in a text editor and copy its full content.
5. Add it as `UNITY_LICENSE` in your GitHub repository secrets.

> ✅ **Recommended Runner:** [game-ci/unity-builder@v4](https://github.com/game-ci/unity-builder)

</details>

---

<details>
<summary>🔹 <strong>v2.0.0 — Custom Docker Image (Unity Pro License Only)</strong></summary>

Uses your own Docker image with pre-installed Unity Editor.

### ⚙️ Setup Instructions

1. Import `ci-github-action-v2.0.0.unitypackage` into the project.
2. Configure `CIConfig.asset` as usual.
3. Use this container section in your `yml` file:

```yaml
container:
  image: ghcr.io/sinlessdevil/unity-ci-image:2023.2.1f1
  credentials:
    username: ${{ secrets.USERNAME }}
    password: ${{ secrets.CR_PAT }}
```

4. Manually activate Unity Pro license inside Docker.
5. Unity Personal **WILL NOT WORK** in headless CI — it requires a display.

| Secret Name      | Description                                  |
| ---------------- | -------------------------------------------- |
| `USERNAME`       | GitHub username (for Docker registry access) |
| `CR_PAT`         | GitHub personal access token                 |
| `UNITY_EMAIL`    | Optional, for in-container activation        |
| `UNITY_PASSWORD` | Optional, for in-container activation        |

### 🧪 Notes on `build.cake`

* This file **does not build Unity itself**.
* Your method should be triggered via `Plugins.CI.Editor.Builder.*`
* The script is responsible for:

  * Parsing logs
  * Setting app version
  * Tracking Git SHA / commits
  * Optional test/log analysis

### 🔁 CI Triggers

* Automatically runs when commit message contains `#build`

### ⚠️ Common Issues & Gotchas

* Unity Personal builds will **fail** inside Docker without GUI (headless limitation).
* If using a custom Docker image, you **must activate Unity Pro license** manually inside it.
* Unity activation can silently fail — always check `/root/activation.log`.
* Make sure secrets are properly set, especially `USERNAME`, `CR_PAT`.
* GitHub caches Library folder by default. Invalidation relies on hashing `Assets`, `Packages`, `ProjectSettings`.

</details>

---

<details>
<summary>🔹 <strong>v3.0.0 — Local Builds via Unity Editor</strong></summary>

Adds support for building directly inside Unity Editor (no Docker/GitHub Actions required).

### ⚙️ Setup Instructions

Import `ci-github-action-v3.0.0.unitypackage` into your project.
Call any of the following methods from `Plugins.CI.Editor.Builder.cs`:

```csharp
Builder.BuildAndroidAPK_Dev(); // Fast APK build
Builder.BuildAab();            // Release AAB build
```

### 🧪 Features

* Runs build directly from Editor (ideal for local/dev use)
* Automatically skips `[Ignore]`d tests
* Suppresses Unity splash screens & dialogs (headless-friendly)
* Supports silent builds, log parsing, versioning

</details>
