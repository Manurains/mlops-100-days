# Day 001 – Solution

## Objective

Build a reproducible Python development environment for Machine Learning workloads and prepare the project for future MLOps automation and containerized deployments.

---

# Solution Approach

The solution was implemented in four phases:

1. Environment Preparation
2. Dependency Management
3. Automation
4. Containerization Readiness

---

# Step 1: Verify Python Installation

Validate Python availability and version.

```bash
python3 --version
which python3
```

Example Output:

```text
Python 3.10.12
/usr/bin/python3
```

---

# Step 2: Resolve Virtual Environment Dependency

## Problem Encountered

The initial environment creation failed with:

```text
The virtual environment was not created successfully because ensurepip is not available.
```

## Root Cause

The Ubuntu server did not have the required virtual environment package installed.

## Resolution

```bash
sudo apt update

sudo apt install -y python3.10-venv
```

## Verification

```bash
python3 -m venv testenv

ls testenv
```

Output:

```text
bin
include
lib
lib64
pyvenv.cfg
```

This confirmed that Python virtual environments were functioning correctly.

---

# Step 3: Create Virtual Environment

Create a dedicated environment for the project.

```bash
python3 -m venv .venv
```

Activate the environment:

```bash
source .venv/bin/activate
```

Verification:

```bash
which python

python --version
```

Example Output:

```text
/home/administrator/Mlops-100days/mlops-100-days/Day-001-Reproducible-Python-Environments/.venv/bin/python
```

---

# Step 4: Install Machine Learning Dependencies

Install commonly used Machine Learning libraries.

```bash
pip install \
numpy \
pandas \
scikit-learn \
jupyter \
matplotlib
```

Verify installation:

```bash
pip list
```

---

# Step 5: Generate Dependency Lock File

Create a reproducible dependency snapshot.

```bash
pip freeze > code/requirements.txt
```

Verify:

```bash
cat code/requirements.txt
```

This file can be used to recreate the exact environment later.

---

# Step 6: Automate Environment Setup

Created a reusable setup script.

File:

```text
code/setup.sh
```

Contents:

```bash
#!/bin/bash

python3 -m venv .venv

source .venv/bin/activate

pip install -r requirements.txt

echo "MLOps environment ready"
```

Grant execution permission:

```bash
chmod +x code/setup.sh
```

Benefits:

* Faster onboarding
* Repeatable setup
* Reduced manual errors

---

# Step 7: Configure Git Ignore Rules

Created a `.gitignore` file to prevent unnecessary files from entering source control.

```gitignore
.venv/
**/.venv/

__pycache__/
*.pyc

.ipynb_checkpoints/

.env
.vscode/
```

Benefits:

* Cleaner repository
* Smaller commits
* Improved security

---

# Step 8: Containerization Readiness

Although the primary objective focused on virtual environments, containerization was evaluated as a deployment strategy.

Sample Dockerfile:

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["python", "--version"]
```

Build:

```bash
docker build -t mlops-day1 .
```

Run:

```bash
docker run --rm mlops-day1
```

Benefits:

* Consistent execution environment
* Portable deployments
* CI/CD friendly
* Kubernetes compatible

---

# Validation Results

| Validation                           | Status |
| ------------------------------------ | ------ |
| Python Installed                     | ✅      |
| Virtual Environment Created          | ✅      |
| Environment Activated                | ✅      |
| Dependencies Installed               | ✅      |
| Requirements File Generated          | ✅      |
| Setup Script Created                 | ✅      |
| Git Ignore Configured                | ✅      |
| Containerization Strategy Documented | ✅      |

---

# MLOps Perspective

Environment reproducibility is the foundation of every successful MLOps platform.

Without it:

* Experiments become difficult to reproduce.
* CI/CD pipelines fail unexpectedly.
* Model behavior becomes inconsistent.
* Deployment troubleshooting becomes more complex.

By establishing reproducible environments early, teams improve reliability throughout the Machine Learning lifecycle.

---

# LLMOps Perspective

AI and Large Language Model workloads introduce additional dependencies:

* CUDA versions
* GPU drivers
* PyTorch compatibility
* vLLM runtime versions
* Transformer library dependencies

For AI platforms, environment consistency directly impacts:

* Model performance
* GPU utilization
* Deployment success
* Operational stability

---

# Outcome

Successfully created a reproducible Python environment for Machine Learning development, documented troubleshooting steps, automated setup procedures, and evaluated containerization as a deployment strategy.

This environment serves as the foundation for future MLOps activities including experimentation, model training, tracking, deployment, and monitoring.
