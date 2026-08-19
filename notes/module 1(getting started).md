# Python, Conda & the Basics — A Working Revision Guide

Think of this note as two connected stories: **how you set up a clean workspace** (Anaconda, Conda, VS Code, Jupyter) and **how Python itself thinks** (variables, types, operators). Master both and you'll never be confused about _"why isn't this working on my machine"_ again.

---

## Part 1 — Building Your Workspace

### 1. The Golden Rule: One Project, One Environment

Imagine you're a chef who cooks Italian food in one kitchen and Japanese food in another — you'd never want soy sauce accidentally ending up in your marinara. Python environments work the same way:

> **ONE PROJECT → ONE DEDICATED ENVIRONMENT**

Why bother? Different projects often need _different Python versions_ and _different package versions_. Without isolation, installing something for Project B can silently break Project A. With isolated environments, each project lives in its own bubble — no cross-contamination.

**A real example from this workspace:**

|Project|Environment|Python Version|
|---|---|---|
|`data_science`|`venv`|3.10.20|
|`python_series`|`venv`|3.12.13|

Notice both are named `venv` — that's fine! Since their _full folder paths_ differ, Conda treats them as completely separate environments.

> ⚠️ **Trap to avoid:** Installing a package in `base` does **not** make it available in `data_science\venv`. Every environment installs and tracks its own packages independently — even `ipykernel`.

---

### 2. Anaconda vs. Conda vs. Base — Untangling the Terms

|Term|What it actually is|
|---|---|
|**Anaconda**|A full Python _distribution_ — Python + Conda + a bundle of data-science tools/packages|
|**Conda**|The _engine_ that manages environments and packages|
|**Base**|Conda's _default_ environment (comes pre-installed)|
|**Project environment**|An _isolated_ environment you create for a specific project|

Key insight: the Python version sitting in `base` (say, 3.14.6) tells you nothing about the Python version inside your project environments. Each is independent.

---

### 3. Creating, Activating, and Verifying an Environment

**Create one:**

```bash
conda create -p venv python=3.10
```

Breaking that down:

- `conda create` → make a new environment
- `-p` → put it at a specific path (rather than Conda's default storage location)
- `venv` → the folder name for the environment
- `python=3.10` → the Python version to install inside it

**Activate / deactivate:**

```bash
conda activate .\venv
conda deactivate
```

Once active, your terminal prompt visibly changes to show the environment path — that's your confirmation it worked.

> 🚫 **Common typo:** Never prefix the command with `$` (that's shell notation from tutorials, not something you type). PowerShell will try to _execute_ the `$` and fail. ❌ `$ conda activate .\venv` → ✅ `conda activate .\venv`

**Check what environments exist:**

```bash
conda info --envs
```

The currently active one is marked with an asterisk (`*`).

**Verify which Python you're actually running** (do this constantly — it saves hours of debugging):

```bash
python --version
python -c "import sys; print(sys.executable)"
```

The second command is the more trustworthy one — it shows the _exact executable path_ in use, leaving no ambiguity.

---

### 4. Installing Packages the Right Way

```bash
pip install pandas                 # one package
pip install -r requirements.txt    # everything a project needs
pip install ipykernel              # lets Jupyter talk to this environment
```

**Golden sequence:** always activate _before_ you install.

```bash
conda activate .\venv
pip install pandas
```

Skip the activation step, and you might install pandas into the wrong environment entirely — a classic source of "but it works on my other project!" confusion.

**Why `requirements.txt` matters:** it's a shareable shopping list of dependencies —

```
pandas
numpy
scipy
scikit-learn
ipykernel
```

— so anyone (including future-you on a new machine) can recreate the exact setup with one command.

---

### 5. Jupyter Notebooks + VS Code: Making Them Talk to Each Other

A `.ipynb` file (Jupyter Notebook) needs a **kernel** to run its cells — essentially a live Python process attached to a specific environment.

If VS Code shows:

> _"Running cells with 'venv (...)' requires the ipykernel package."_

...it means that specific environment is missing `ipykernel`. Fix it exactly where it's needed:

```bash
pip install ipykernel
```

Then pick the matching interpreter in VS Code — e.g., `venv (Python 3.10.20)` for the `data_science` folder, `venv (Python 3.12.13)` for `python_series`. Always double-check with `sys.executable` if you're unsure which one is actually selected.

---

### 6. Three Ways to Create a Python Environment

| Method              | Setup                    | Create                             | Activate (Windows)      |
| ------------------- | ------------------------ | ---------------------------------- | ----------------------- |
| **Built-in `venv`** | Comes with Python        | `python -m venv venv`              | `venv\Scripts\activate` |
| **`virtualenv`**    | `pip install virtualenv` | `virtualenv venv`                  | `venv\Scripts\activate` |
| **Conda**           | Needs Anaconda/Miniconda | `conda create -p venv python=3.10` | `conda activate .\venv` |

**Why Conda tends to win for data science:** it manages Python _versions themselves_ (not just packages), handles non-Python dependencies gracefully, and is purpose-built for scientific computing workflows.

---

### 7. `.py` File vs. `.ipynb` Notebook

||Python file (`test.py`)|Notebook (`test.ipynb`)|
|---|---|---|
|Run with|`python test.py`|Cell-by-cell, via kernel|
|Shortcut|—|`Shift + Enter` runs the current cell|
|Cell types|—|Markdown cells (notes) + Code cells (executable)|

---

Fast Recall Zone

### 🩹 Common Errors & Fixes

|Error / Symptom|Cause|Fix|
|---|---|---|
|`$ conda activate .\venv` fails|`$` isn't valid in PowerShell|Drop the `$`|
|`python --VERSION` fails|Wrong casing|Use lowercase: `python --version`|
|`ModuleNotFoundError: No module named 'pandas'`|Not installed in the _active_ environment|`pip install pandas`|
|Notebook seems frozen after `input()`|It's waiting for input|Type the value into the notebook's own prompt|
|Wrong interpreter selected in VS Code|Mismatched environment|Manually reselect the correct `venv (Python x.x.x)`|

### 📋 Command Cheat Sheet

```bash
# Conda environment lifecycle
conda create -p venv python=3.10
conda activate .\venv
conda deactivate
conda info --envs

# Verifying Python
python --version
python -c "import sys; print(sys.executable)"

# Packages
pip install pandas
pip install -r requirements.txt
pip install ipykernel

# Running code
python test.py
```





---

## 🎯 Golden Rules to Lock In

1. One project → one dedicated environment.
2. Different environments can run different Python versions simultaneously.
3. Packages belong to a specific environment — never assume they carry over.
4. Always activate the correct environment _before_ installing anything.
5. Verify with `python --version` and `sys.executable` — don't guess.
6. In VS Code, explicitly select the right interpreter.
7. Jupyter notebooks need a kernel (`ipykernel`) installed _in that environment_.




---