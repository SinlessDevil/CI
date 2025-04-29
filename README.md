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

**To set these:**
- Go to your repository → Settings → Secrets and variables → Actions → New repository secret

Paste full ULF license (or use activation step from [game-ci/unity-activate](https://github.com/game-ci/unity-activate)).

## 🧪 Notes on `build.cake`

This script:
- Is **not** responsible for building Unity directly — Unity must call your build method via `Plugins.CI.Editor.Builder.*`
- Runs test log parsing, sets version, handles Git commit history, and manages SHA file tracking
- You can run it separately for logging/tests even without actual build
