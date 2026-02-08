# 🚀 Notebook Refactor Automation Suite

A modular, **security-first pipeline** designed to bridge the gap between data science experimentation and production-grade software engineering. This suite automates the conversion of `.ipynb` notebooks into clean, PEP 8 compliant Python scripts while enforcing strict security protocols.

## 🏗️ Architecture & Workflow

The suite operates as a decoupled orchestration layer, ensuring that your logic is transformed without compromising the integrity of your production environment.

* **Extraction:** Parses JSON-based notebooks, stripping "magics" (e.g., `%%bash`) and wrapping code in `if __name__ == "__main__":` guards.
* **Security Gate:** A zero-trust `security_scan.py` module audits the generated code for high-entropy strings or API patterns before allowing a Git push.
* **Secure Git Ops:** Utilizes `GIT_ASKPASS` to handle authentication, ensuring Personal Access Tokens (PATs) never leak into system logs or shell history.
* **Repo Hygiene:** Automatically bootstraps the destination repository with a `.gitignore` and descriptive documentation.

---

## 🛠️ Project Structure

```text
nb_refactor_suite/
├── nb_refactor/          # Core Package
│   ├── cli.py            # Orchestrator (The "Brain")
│   ├── git_ops.py        # Secure Git integration (GIT_ASKPASS)
│   ├── security_scan.py  # Pre-push secret detection
│   └── notebook_refactor.py # IPYNB -> PY logic
└── ...

```

---

## 🌟 Senior-Level Features

### 1. Idempotency

To prevent "Git noise," the suite uses a `write_text_if_changed` utility. It compares hashes of existing files and only performs a write/commit if changes are detected, keeping your contribution graph meaningful.

### 2. Zero-Trust Security

The pipeline assumes all notebooks are "unsafe." Even if a push is triggered, the `security_scan` acts as a hard gate. If a potential secret is detected, the entire operation is aborted.

### 3. Decoupled Logic

The architecture is provider-agnostic. While currently tuned for GitHub, the `git_ops.py` module is isolated, allowing for an easy swap to GitLab or Bitbucket without touching the core transformation logic.

---

## 🚀 Quick Start

1. **Configure:** Add your `PAT` and `REPO_URL` to `.env`.
2. **Install:** `pip install -r requirements.txt`
3. **Execute:** ```bash
python -m nb_refactor
```


