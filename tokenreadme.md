# 🚀 Secure Automated Multi-Repository Version Sync (GitHub Actions)

## 📌 Overview

This project automates version updates across multiple **private GitHub repositories** by securely fetching the latest release tags and updating them in a central configuration file (`abc.yml`).

It uses **GitHub Actions + Python + Fine-Grained Personal Access Token (PAT)** to ensure secure and reliable automation.

---

# ❗ Problem Statement

In our setup:

* A **central repository** maintains integrations via a YAML file (`abc.yml`)
* Around **10–12 private repositories** are referenced
* Each repository belongs to the same organization and has its own release cycle

---

## 🔴 Manual Process (Before Automation)

1. Navigate to each private repository
2. Check latest release/tag
3. Copy version manually
4. Return to main repository
5. Update version in YAML
6. Commit changes

---

## ⚠️ Challenges

* ⏳ Time-consuming (10–15 minutes per update cycle)
* ❌ High risk of human error
* 🔄 Version inconsistency across environments
* 🚫 Difficult to scale as repositories increase
* 🔐 Private repos require authenticated access

---

# 💡 Solution

Implemented a **secure GitHub Actions workflow** that:

* Reads repository URLs from YAML
* Authenticates using PAT
* Fetches latest release tags from private repositories
* Updates version fields automatically
* Commits and pushes changes

---

# 🔐 Authentication & Security

## Why Token is Required

GitHub default token (`GITHUB_TOKEN`) cannot access **other private repositories** within the organization.

Therefore, a **Fine-Grained PAT** is used.

---

## 🔑 Token Implementation

* Fine-Grained PAT created with:

  * Access: **Only selected repositories**
  * Permission: **Contents → Read-only**

* Stored securely in GitHub Secrets:

```yaml
env:
  MY_GH_TOKEN: ${{ secrets.MY_GH_TOKEN }}
```

* Used in API call:

```python
Authorization: Bearer MY_GH_TOKEN
```

---

## 🔒 Security Best Practices Followed

* ❌ No hardcoded tokens
* 🔐 Token stored in GitHub Secrets
* 🔒 Least privilege access (only required repos)
* 🔁 No write access to external repos
* 🚫 Main branch protected

---

# ⚙️ Workflow Steps

## 1️⃣ Trigger

* Manual trigger (`workflow_dispatch`)

---

## 2️⃣ Checkout Code

* Fetch selected branch
* Load repository content

---

## 3️⃣ File Validation

* Ensure `abc.yml` exists
* Fail fast if missing

---

## 4️⃣ Processing Logic

For each repository:

* Extract `owner` and `repo` from URL
* Call GitHub API:

```
/repos/{owner}/{repo}/releases/latest
```

* Authenticate using PAT
* Fetch latest release tag
* Update corresponding `version` field

---

## 5️⃣ Change Detection

* Detect if version changed
* Skip unnecessary commits

---

## 6️⃣ Commit & Push

* Commit only when changes exist
* Push updates to same branch

---

## 7️⃣ Main Branch Protection

* Workflow blocked on `main`
* Prevents accidental production changes

---

# ⚠️ Precautions Taken

* 🔐 Secure authentication via PAT
* 📁 File existence validation
* ⚠️ Error handling for API failures
* 🔍 Strict regex to avoid incorrect updates
* ⚡ Conditional commit logic
* 🚫 Avoid execution on main branch

---

# 📊 Benefits

## ⏳ Time Saving

| Task               | Before    | After      |
| ------------------ | --------- | ---------- |
| Update 10–12 repos | 10–15 min | ~30–60 sec |

---

## ❌ Error Reduction

* Eliminates manual mistakes
* Ensures correct version updates

---

## 🔄 Consistency

* All integrations use latest versions
* No version drift

---

## 📈 Scalability

* Easily supports multiple repositories
* No code change required

---

## 🔐 Secure Access

* Private repositories handled securely
* Controlled access using PAT

---

# 💰 Impact

* Improved DevOps efficiency
* Reduced operational overhead
* Increased deployment reliability
* Secure automation for private environments

---

# 📁 Example (`abc.yml`)

```yaml
- src: https://github.com/org/repo1
  version: v1.0.0

- src: https://github.com/org/repo2
  version: v2.3.1
```

---

# ⚙️ Tech Stack

* GitHub Actions
* Python (urllib, regex, JSON)
* GitHub REST API
* YAML
* GitHub Secrets (Secure Token Management)

---

# 👨‍💻 Author

**Omkar Singh**

---

# ⭐ Conclusion

This solution automates version management across private repositories using secure authentication, eliminating manual effort and ensuring consistency, scalability, and reliability in a production environment.
