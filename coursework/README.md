# Trustworthy GeoAI — Course Work

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/coursework/TrustGeoAI_CourseWork.ipynb)

**Spatial Visual Analytics · Trustworthy GeoAI**

In this course work you explore **where a GeoAI model can and cannot be trusted**, using *spatial
uncertainty* (prediction intervals from conformal prediction) as the trust signal. You work inside
one notebook — **`TrustGeoAI_CourseWork.ipynb`** — and spend your effort **choosing visualizations
and interpreting trust**, not writing code.

> Using an LLM (ChatGPT, Claude, …) to help is **allowed but optional** — what we grade is **your
> perspective on trust**, not your code. You do **not** need an API key or a paid plan.

---

## ▶️ Run it in Google Colab (recommended — only a Google account needed)

**Click the badge above**, or this link:
<https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/coursework/TrustGeoAI_CourseWork.ipynb>

Then:

1. **Sign in** with your Google account.
2. **⚠️ Make your own copy first: *File ▸ Save a copy in Drive*.** Opening from GitHub is read-only —
   work in the copy in *your* Drive so your code, answers, and results are saved.
3. In your copy, **Runtime ▸ Run all** (or run cells top to bottom with `Shift`+`Enter`).
   The first cell downloads the method and the data for you — **nothing to install**, Colab already
   has the rest. Only the dataset you actually pick is downloaded.
4. **Explore** with the drop-down menus, and **fill in the 🟨 *YOUR TURN* cells**.
5. Your work auto-saves to Drive. To hand in, also *File ▸ Download ▸ Download .ipynb* (and your `report.html`).

That is the whole setup. The rest of this page explains how to use the notebook; a short
[*Run locally*](#run-locally-optional) appendix is at the end if you prefer your own machine.

---

## How to use the notebook

Two kinds of cells:

- 🟦 **PROVIDED** — already work; just run them, do not edit.
- 🟨 **YOUR TURN** — you fill in (choose a dataset, build a view, or write a reflection).

The core is the **dashboard explorer** in section ②③. Its controls let you compose your own story:

- **dataset** — the theme you study
- **method** — `geocp` (prediction intervals) or `bayesian` (adds a meta-uncertainty)
- **slots 1–6** — place a chart in each slot to **position** the panels in the order you want
  (leave a slot `(empty)` to skip); **columns** wraps them into a grid
- **palette** — the color scheme · **US basemap** on/off (equal-area projection) · **point size**
- **alpha** (target coverage) and **bandwidth** (how local the method is) — tune the trade-offs

Below the dashboard there is also an **interactive map** (Plotly): **drag to pan, scroll/box to
zoom, hover** for values, and type a **region** to focus — the easiest way to pick the visualization
**range** you want to discuss. (Static dashboard maps use an equal-area projection so proportions
are correct; the interactive map is fully pannable.)

When you are happy, the **📤 Export your report** section turns your selection + your four write-ups
(and an optional **region** focus) into a standalone **`report.html`** — your unique deliverable.

---

## The four aspects you are graded on

| | Aspect | What to produce |
|---|---|---|
| **①** | **The scientific question** | Choose a dataset, read its column dictionary, and state a clear question where *trust matters*. |
| **②** | **The design of user interface** | Choose a **combination** of charts and justify **why** it lets people see the model's **risk** more clearly than one chart alone. |
| **③** | **The way of revealing trustworthiness** | Interpret **where the model can / cannot be trusted**, pointing to a concrete region or pattern. |
| **④** | **Retrospect the limits of GeoAI** | Explain where/why this model is not trustworthy, then **propose how the GeoAI trust problem could be solved**. |

---

## The visualization methods (your main toolkit)

Each exposes a *different* facet of trustworthiness — try several and compare:

- **Uncertainty map** — where is the model unsure (spatially)?
- **Prediction map** / **Absolute-error map** — the values, and where it is actually wrong.
- **Coverage hit/miss map** — where the prediction intervals fail.
- **Interval-width histogram** — how wide (confident) the intervals are.
- **Honesty check** & **Reliability** — does the *claimed* uncertainty match the *real* error?
- **Spatial grid (hexbin)** — aggregated uncertainty per area.
- **Regional coverage bars** — does the global 90% guarantee also hold *locally*?
- **Bayesian meta-uncertainty map** — only when `method = bayesian`.

### Build your own view (encouraged — this is where your perspective shows)

You are not limited to the ready-made methods. In the *“Build your own visualization”* cell you can
register your own view in a few lines, and it appears in the same drop-down automatically:

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

## Tips for a strong submission

- Switching `method` to **`bayesian`**, or comparing **two datasets**, often changes the trust
  picture — great material for ③ and ④.
- Look for **mismatches**: high coverage overall but low coverage in one region; wide intervals where
  the model is actually accurate (over-cautious), or narrow intervals where it is wrong
  (over-confident).
- Keep your writing concise and **evidence-based** — refer to what the plot shows.

---

## What to submit

Both of:
- the completed `TrustGeoAI_CourseWork.ipynb` with **all cells run** (plots visible) and the four 🟨
  write-up cells filled in — from Colab: *File ▸ Download ▸ Download .ipynb*; and
- your exported **`report.html`** (from the *📤 Export your report* section).

---

## Run locally (optional)

Colab is recommended. If you prefer your own machine:

1. Install Python **3.9+** (e.g. via [Miniconda](https://docs.conda.io/en/latest/miniconda.html)).
2. In a terminal:
   ```bash
   git clone https://github.com/Vezarachan/TUM_Course_SVA_TrustGeoAI.git
   cd TUM_Course_SVA_TrustGeoAI/coursework
   pip install numpy pandas matplotlib scikit-learn scipy ipywidgets plotly jupyterlab
   jupyter lab
   ```
3. Open `TrustGeoAI_CourseWork.ipynb` and **Run All**. It uses the local `dataset/` folder if present,
   and otherwise downloads what it needs — so it works the same as on Colab.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| **No drop-down menus appear** | `ipywidgets` is missing/older. On Colab it is preinstalled — just **Runtime ▸ Restart and run all**. Locally: `pip install ipywidgets` and restart the kernel. The notebook still works without it (it prints how to call `explore(...)` by hand). |
| **The interactive map does not appear** | It needs `plotly` (preinstalled on Colab). Locally: `pip install plotly` and restart the kernel. Without it, a static map is shown instead. |
| **A download fails / no internet** | The notebook fetches data from GitHub on first use. Re-run the cell once your connection is back. |
| **No column dictionary shown** | It is read from `dataset/metadata.json`, which downloads automatically; re-run the setup cell if a download was interrupted. |
| **`KeyError: 'Y'`** | You used a dataset outside the provided list. Pick one from the drop-down. |
| **It is slow the first time** | The first run of a dataset/method downloads it and trains a model (a few seconds); results are then cached. Large datasets are auto-sampled to 1,800 rows. |
