
# Trustworthy GeoAI — Course Work

**Spatial Visual Analytics · Trustworthy GeoAI**

In this course work you explore **where a GeoAI model can and cannot be trusted**, using *spatial
uncertainty* (prediction intervals from conformal prediction) as the trust signal. You work inside
one notebook — **[`TrustGeoAI_CourseWork.ipynb`](TrustGeoAI_CourseWork.ipynb)** — and spend most of
your effort **choosing visualizations and interpreting trust**, not writing code.

> Using an LLM (ChatGPT, Claude, …) to help is **allowed but optional** — what we grade is **your
> perspective on trust**, not your code. You do **not** need an API key or a paid plan.

---

## ▶️ Recommended: run in Google Colab (only a Google account needed)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/live_demo/TrustGeoAI_CourseWork.ipynb)

Click the badge above (or this link):
<https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/live_demo/TrustGeoAI_CourseWork.ipynb>

1. Sign in with your Google account.
2. Menu **Runtime ▸ Run all**. The first cell downloads the method and datasets for you — **nothing
   to install** (Colab already has the rest).
3. Use the drop-down menus and fill in the 🟨 *YOUR TURN* cells.
4. Save your work: **File ▸ Save a copy in Drive** (and download the `.ipynb` to submit).

> Prefer to run on your own computer instead? Follow the local-setup guide below.

---

## Table of contents (local setup)
1. [Install Python](#1-install-python)
2. [Get the course files](#2-get-the-course-files)
3. [Create an environment and install the packages](#3-create-an-environment-and-install-the-packages)
4. [Launch the notebook](#4-launch-the-notebook)
5. [Jupyter basics (how to run cells)](#5-jupyter-basics-how-to-run-cells)
6. [How to use this notebook](#6-how-to-use-this-notebook)
7. [The four aspects you are graded on](#7-the-four-aspects-you-are-graded-on)
8. [The visualization methods](#8-the-visualization-methods-your-main-toolkit)
9. [Tips for a strong submission](#9-tips-for-a-strong-submission)
10. [Troubleshooting](#10-troubleshooting)
11. [What to submit](#11-what-to-submit)

---

## 1. Install Python

*(Skip this whole section if you use Colab.)*

You need Python **3.9 or newer**. If you are unsure whether you already have it, open a terminal and
run `python --version` (Windows) or `python3 --version` (macOS/Linux). If it prints a version ≥ 3.9,
skip to step 2.

We recommend **one** of the two options below.

### Option A — Anaconda / Miniconda (recommended for beginners)
A single installer that bundles Python, Jupyter, and most scientific packages.

1. Download **Miniconda** (small) or **Anaconda** (large, includes everything) from
   <https://www.anaconda.com/download> or <https://docs.conda.io/en/latest/miniconda.html>.
2. Run the installer. **On Windows, tick “Add to PATH”** if asked (or just use the
   *“Anaconda Prompt”* it installs).
3. Verify — open **Anaconda Prompt** (Windows) or a terminal (macOS/Linux) and run:
   ```bash
   conda --version
   python --version
   ```

### Option B — Plain Python from python.org
1. Download from <https://www.python.org/downloads/> and install.
2. **Windows users: on the first installer screen, tick “Add python.exe to PATH”.** This is the most
   common mistake — without it, `python` will not be found in the terminal.
3. Verify in a new terminal:
   ```bash
   python --version      # Windows
   python3 --version     # macOS / Linux
   ```

> **What is a “terminal”?**
> - **Windows:** press `Win`, type *PowerShell* (or *Anaconda Prompt* if you chose Option A), open it.
> - **macOS:** press `Cmd+Space`, type *Terminal*, open it.
> - **Linux:** your usual terminal app.

---

## 2. Get the course files

Make sure you have the whole **`live_demo`** folder, including its `dataset/` subfolder. Note where
it is on your computer, e.g. `C:\Users\you\Downloads\live_demo` or `~/Downloads/live_demo`.

In the terminal, move into that folder (this is important — the notebook expects to be opened from
inside `live_demo`):

```bash
# Windows (PowerShell)
cd "C:\Users\you\Downloads\live_demo"

# macOS / Linux
cd ~/Downloads/live_demo
```

---

## 3. Create an environment and install the packages

An *environment* is an isolated set of packages, so this course work cannot break your other Python
projects. Create one, activate it, then install the packages.

### If you chose Anaconda/Miniconda (Option A)
```bash
conda create -n trustgeoai python=3.11 -y
conda activate trustgeoai
pip install numpy pandas matplotlib scikit-learn scipy ipywidgets jupyterlab
```

### If you chose plain Python (Option B)
```bash
# Windows (PowerShell)
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install numpy pandas matplotlib scikit-learn scipy ipywidgets jupyterlab

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
pip install numpy pandas matplotlib scikit-learn scipy ipywidgets jupyterlab
```

> If on Windows PowerShell `Activate.ps1` is blocked by an execution-policy error, run once:
> `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` and try again.

What each package is for:

| Package | Why it is needed |
|---|---|
| `numpy`, `pandas`, `scipy` | data handling + the conformal method |
| `scikit-learn` | trains the underlying prediction model |
| `matplotlib` | draws every visualization |
| `ipywidgets` | the interactive drop-down menus |
| `jupyterlab` | the app you run the notebook in |

*(Optional: `openpyxl` — only if you want to re-read the original `Metadata.xlsx`. The dataset
dictionary is also bundled as `dataset/metadata.json`, which needs no extra package.)*

You only install once. **Next time**, you just `cd` into `live_demo` and run `conda activate
trustgeoai` (Option A) or activate the `.venv` (Option B).

---

## 4. Launch the notebook

With the environment **activated** and inside the `live_demo` folder:

```bash
jupyter lab
```

This opens Jupyter in your web browser. In the left-hand file list, double-click
**`TrustGeoAI_CourseWork.ipynb`**.

**Alternative — VS Code:** install the *Python* and *Jupyter* extensions, open the `live_demo` folder
(*File ▸ Open Folder*), open the `.ipynb`, and pick your `trustgeoai` / `.venv` environment as the
kernel (top-right of the notebook).

---

## 5. Jupyter basics (how to run cells)

A notebook is a list of **cells**: *code* cells (gray, run Python) and *markdown* cells (text).

- **Run one cell:** click it, press `Shift+Enter`.
- **Run everything from the top:** menu **Run ▸ Run All Cells**. *Do this first.*
- **Edit a text answer:** double-click a markdown cell, type, then press `Shift+Enter` to render it.
- **A number `[1]` appears** to the left of a code cell after it runs. `[*]` means it is still running.
- **Something broke?** Menu **Kernel ▸ Restart Kernel and Run All Cells** clears state and re-runs.

You will mostly **run** provided cells and **type text** into answer cells — very little code.

---

## 6. How to use this notebook

1. **Open** `TrustGeoAI_CourseWork.ipynb`.
2. **Run All Cells** once. The 🟦 **PROVIDED** cells do all the computation for you.
3. **Explore with the menus.** In the *“② Design of the interface & ③ Revealing trustworthiness”*
   section there are three drop-downs:
   - **dataset** — the theme you study
   - **method** — `geocp` (prediction intervals) or `bayesian` (adds a meta-uncertainty)
   - **visualization** — switch between the ready-made methods and **compare what each reveals**
4. **Write your answers** in the 🟨 **YOUR TURN** cells (double-click to edit).

> Two kinds of cells:
> - 🟦 **PROVIDED** — already work; just run them, do not edit.
> - 🟨 **YOUR TURN** — you fill in (choose a dataset, or write a reflection).

---

## 7. The four aspects you are graded on

Work through them in order; each has a 🟨 cell to complete.

| | Aspect | What to produce |
|---|---|---|
| **①** | **The scientific question** | Choose a dataset, read its column dictionary, and state a clear question where *trust matters*. |
| **②** | **The design of user interface** | Pick the visualization method(s) that best present trust, and justify **why**. |
| **③** | **The way of revealing trustworthiness** | Interpret **where the model can / cannot be trusted**, pointing to a concrete region or pattern in your plot. |
| **④** | **Retrospect the limits of AI tools** | Reflect on where the method (or an LLM you used) falls short, and what stays a **human judgment**. |

---

## 8. The visualization methods (your main toolkit)

Each exposes a *different* facet of trustworthiness. Try several and compare:

- **Uncertainty map** — where is the model unsure (spatially)?
- **Prediction map** / **Absolute-error map** — the values, and where it is actually wrong.
- **Coverage hit/miss map** — where the prediction intervals fail.
- **Interval-width histogram** — how wide (confident) the intervals are.
- **Honesty check** & **Reliability** — does the *claimed* uncertainty match the *real* error?
- **Spatial grid (hexbin)** — aggregated uncertainty per area.
- **Regional coverage bars** — does the global 90% guarantee also hold *locally*?
- **Bayesian meta-uncertainty map** — only when `method = bayesian`.

A strong answer **compares** a few of these and argues which one tells the trust story most clearly.

### Build your own view (encouraged — this is where your perspective shows)

You are not limited to the ready-made methods. In the *“Build your own visualization”* cell you can
register your own view in a few lines and it appears in the same drop-down automatically:

```python
@trust_view("My idea")
def my_view(R):
    # R is a tidy table: lon, lat, pred, truth, lower, upper,
    #   uncertainty, width, error, covered, region, posterior_std
    quickmap(R, R["uncertainty"] / (R["error"] + 1e-9),
             "uncertainty / error  (>1 cautious, <1 over-confident)", cmap="coolwarm")
```

Helpers `quickmap(R, values, title, cmap)` and `quickhist(values, title)` keep it to one or two
lines; or use `matplotlib` directly for full control. After adding a view, re-run the explorer cell.

---

## 9. Tips for a strong submission

- Switching `method` to **`bayesian`**, or comparing **two datasets**, often changes the trust
  picture — great material for ③ and ④.
- Look for **mismatches**: high coverage overall but low coverage in one region; wide intervals where
  the model is actually accurate (over-cautious), or narrow intervals where it is wrong
  (over-confident).
- Keep your writing concise and **evidence-based** — refer to what the plot shows.

---

## 10. Troubleshooting

| Problem | Fix |
|---|---|
| **`python` / `conda` not found** | You did not add it to PATH during install. Reinstall and tick *Add to PATH* (Option B), or use the *Anaconda Prompt* (Option A). |
| **No drop-down menus appear** | `ipywidgets` missing or kernel not restarted. Run `pip install ipywidgets`, then **Kernel ▸ Restart Kernel and Run All**. The notebook still works without it — it prints how to call `explore("dataset.csv", "geocp", "Uncertainty map …")` by hand. |
| **No column dictionary shown** | The dataset dictionary is read from the bundled `dataset/metadata.json` — make sure that file is present. (It does **not** need `openpyxl`.) Everything else runs regardless. |
| **`ModuleNotFoundError: geocp`** | Open the notebook from **inside** the `live_demo` folder, then **Run All** again (the Setup cell adds the right paths). |
| **`KeyError: 'Y'`** | You picked a dataset without the standard `Y / X* / lon-lat` schema. Choose one listed by the catalog cell (e.g. a `US_Health_*` file). |
| **Wrong kernel / packages “not found” although installed** | Top-right of the notebook, make sure the selected kernel is your `trustgeoai` / `.venv` environment, not a different Python. |
| **PowerShell blocks `Activate.ps1`** | Run once: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, then activate again. |
| **It is slow** | Big datasets are auto-sampled to 1,800 rows; the first run of each dataset/method trains a model (a few seconds), then results are cached. |

---

## 11. What to submit

The completed `TrustGeoAI_CourseWork.ipynb`, with **all cells run** (plots visible) and the four 🟨
write-up cells filled in. Make sure your dataset choice and your reflections for ①–④ are present.

*(Optional: export a PDF via **File ▸ Save and Export Notebook As ▸ PDF/HTML** if the assignment asks
for it.)*
