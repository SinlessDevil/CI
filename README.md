# ☁️ Unity CI GitHub Action Integration

Automate Unity Android builds with GitHub Actions and Cake. Save your time, avoid repeating setups, and focus on development.

---

## 🚀 Getting Started

### 1. Import Unity Package
- Import `ci-github-action-v1.0.0.unitypackage` into your Unity project.
- Navigate to `Assets/Plugins/CI/CIConfig.asset` to configure.
- You will now have a working `build.cake` file.

### 2. Required GitHub Secrets
> Without these, your CI build won't authenticate or work with Unity.

| Secret Name         | Description                      |
|---------------------|----------------------------------|
| `UNITY_EMAIL`       | Your Unity account email         |
| `UNITY_PASSWORD`    | Your Unity account password      |
| `UNITY_LICENSE`     | Your Unity license file content  |

**How to get your Unity license:**
1. Open Unity Hub on your local machine.
2. Go to `Preferences → Licenses → Manual Activation`.
3. After activating manually, locate the `.ulf` file.
   - On Windows: `C:\ProgramData\Unity\Unity_lic.ulf`
   - On macOS: `/Library/Application Support/Unity/Unity_lic.ulf`
4. Open it in a text editor and copy the full content.
5. Paste it into the `UNITY_LICENSE` GitHub secret.

**To set these:**
- Go to your repository → Settings → Secrets and variables → Actions → New repository secret
Paste full ULF license (or use activation step from [`game-ci/unity-activate`](https://github.com/game-ci/unity-activate)).
> ✅ Recommended CI Runner  
> For best stability and compatibility, use [`game-ci/unity-builder@v4`](https://github.com/game-ci/unity-builder).  
> It's a well-maintained GitHub Action that installs Unity in CI and triggers your build method via CLI.

## 🧪 Notes on `build.cake`

This script:
- Is **not** responsible for building Unity directly — Unity must call your build method via `Plugins.CI.Editor.Builder.*`
- Runs test log parsing, sets version, handles Git commit history, and manages SHA file tracking
- You can run it separately for logging/tests even without actual build

### 🔁 Triggering Builds

Builds are triggered automatically on any commit message containing `#build`.
