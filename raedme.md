# 🚀 Automated Multi-Repository Version Sync (GitHub Actions)

## 📌 Overview

This project automates version updates across multiple GitHub repositories by fetching the latest release tags and updating them in a central configuration file (`abc.yml`).

It is designed for environments where multiple tools (e.g., Tenable, CrowdStrike, Splunk, SCCM, etc.) are integrated through separate repositories and managed centrally.

---

# ❗ Problem Statement

In our setup:

* A **central repository** contains a YAML file referencing multiple repositories
* Around **10–12 repositories** are used for different agents/integrations
* Each repository has its own release lifecycle

### 🔴 Manual Process (Before)

1. Open each repository
2. Check latest release/tag
3. Copy version
4. Update in main repo (`abc.yml`)
5. Commit changes

---

## ⚠️ Challenges

* ⏳ Time-consuming (10–15 minutes per cycle)
* ❌ High chance of human error
* 🔄 Version inconsistency across environments
* 🚫 Risk of missing updates
* 📉 Not scalable

---

# 💡 Solution

A GitHub Actions workflow automates this entire process.

---

## 🔧 What This Automation Does

* Reads repository URLs from `abc.yml`
* Fetches latest release tag using GitHub API
* Updates corresponding `version` fields
* Commits and pushes changes automatically

---

# ⚙️ Workflow Steps

## 1️⃣ Trigger

* Manual trigger using `workflow_dispatch`

---

## 2️⃣ Checkout Code

* Fetch selected branch
* Load full repository history

---

## 3️⃣ File Validation

* Ensure `abc.yml` exists
* Fail early if file missing

---

## 4️⃣ Processing Logic

For each repository:

* Extract repository URL using regex
* Call GitHub API:

  ```
  /repos/{owner}/{repo}/releases/latest
  ```
* Fetch latest release tag
* Update corresponding version field

---

## 5️⃣ Change Detection

* Check if any version was modified
* Avoid unnecessary commits

---

## 6️⃣ Commit & Push

* Commit only when changes exist
* Push updates to same branch

---

## 7️⃣ Main Branch Protection

* Workflow blocked on `main` branch
* Prevents accidental production changes

---

# 🔐 Security Implementation

## Without Token

* Works only for public repositories
* Limited rate (60 requests/hour)

## With Token (Recommended)

* Fine-Grained Personal Access Token (PAT)
* Access restricted to selected repositories
* Stored securely using GitHub Secrets

---

# ⚠️ Precautions Taken

* 🔐 No hardcoded secrets (uses GitHub Secrets)
* 🔒 Least privilege access (read-only permissions)
* 🚫 Main branch protection enabled
* ⚠️ Error handling implemented (no full workflow failure)
* 🔍 Strict regex matching to avoid incorrect updates
* ⚡ Commit only when changes detected
* 📁 File existence validation added

---

# 📊 Benefits

## ⏳ Time Saving

| Task              | Before    | After      |
| ----------------- | --------- | ---------- |
| Check 10–12 repos | 10–15 min | ~30–60 sec |

---

## ❌ Error Reduction

* Eliminates manual mistakes
* Prevents version mismatch

---

## 🔄 Consistency

* Always uses latest versions
* Keeps all integrations aligned

---

## 📈 Scalability

* Works for multiple repositories without changes

---

## 🚀 Efficiency

* Can be triggered anytime

---

# 💰 Impact

* Saves DevOps time significantly
* Improves deployment reliability
* Reduces production issues
* Standardizes version management

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

---

# 👨‍💻 Author

**Omkar Singh**

---

# ⭐ Conclusion

This automation replaces a repetitive manual task with a scalable, secure, and efficient solution, ensuring consistent and up-to-date versions across multiple repositories.
