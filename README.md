---

## Before you start — every session

You work on the class VS Code server, in your own clone of this repo.
Your userid shows up in your repo name, your clone URL, and your filenames
via `$(whoami)`: `whoami` prints your userid, and `$(whoami)` inserts it
automatically. If the folder is missing, re-clone it — your lesson has the
exact URL, always ending in `_student.git`.

**Pull before work, every session** — it gets any changes I pushed to your
repo since last class:

```bash
cd ~/<your-clone-folder>
git config pull.rebase false
git pull
```

- `git config pull.rebase false` tells git how to combine work; run it once,
  it is not an error if you already ran it.
- If the pull prints `Already up to date.` you have everything.
- **Asked for a username/password?** GitHub username plus Personal Access
  Token (PAT) — never your GitHub password.

---

# Range, Variance, and Standard Deviation — Lab Starter (Unit 3)

Your job this lesson: build a spread analysis toolkit from scratch and apply it
to real donation data.

The lesson linked in the Google Classroom assignment is the authority for the
whole lab — every function, every number, and the turn-in. This README is the
repo map plus the contract your pushed `spread.py` has to meet.

## What's here

- `spread.py` — **your file to create**. Four functions: `mean_of`,
  `median_of`, `variance_of`, `stdev_of`. It opens the data file itself so it
  runs cold with `python3 spread.py`.
- `data/u3_donations.txt` — 200 alumni donation amounts (one per line). One
  mis-key of 90000 is included on purpose.
- `README.md` — this file.
- `journal-TEMPLATE.md` — copy this to make your reflection journal.
- `self_check.py` + `journal_check.yml` — CI progress meter (never pass/fail).
- `.github/workflows/grade.yml` — runs the check on every push.

## Your job

Follow the lesson parts in order. Create `spread.py` as you go — do not rename
it. The code below is the artifact contract; the lesson gives the reasoning.

### PART 0 — Set up (5 min)

```bash
cd ~/compmath-u3-stats-lab
python3 -c "
with open('data/u3_donations.txt') as f:
    raw = [line.strip() for line in f if line.strip()]
donations = [float(line) for line in raw if not line.startswith('#')]
clean = [d for d in donations if d < 5000]
print(len(donations), len(clean))
"
```

You should see `200 199` — 200 total, 199 after removing the 90000 outlier.

### PART 1 — Load the data inside spread.py (5 min)

Put this at the **top** of `spread.py`. Variables you made in the PART 0
one-liner do not exist when you run the file as a script — without this block
`python3 spread.py` dies with `NameError: name 'donations' is not defined`.

```python
with open("data/u3_donations.txt") as f:
    raw = [line.strip() for line in f if line.strip()]
donations = [float(line) for line in raw if not line.startswith("#")]
clean = [d for d in donations if d < 5000]
```

### PART 2 — Variance (15 min)

Add to `spread.py`:

```python
def variance_of(values):
    m = sum(values) / len(values)
    total = sum((v - m) ** 2 for v in values)
    return total / len(values)
```

Test on the paper set `25 40 50 60 75 100` first (should match 588.89), then on
real data. Divide by `len(values)` — population variance. No `statistics`
library, no pandas.

### PART 3 — Standard deviation (10 min)

Add to `spread.py`:

```python
def stdev_of(values):
    return variance_of(values) ** 0.5
```

No `import math` and no `statistics` module — the lesson writes the formulas.

### PART 4 — Run it cold (10 min)

Add `mean_of` and `median_of` as the lesson gives them, then print the
side-by-side table. `python3 spread.py` from the repo root must produce exactly
the expected output in the lesson:

```text
set             mean    median       range      stdev
all 200        716.07     242.76    89928.67    6330.32
clean 199      267.40     242.38      742.56     121.05
```

### PART 5 — Save, Commit, Push (last 10 min)

```bash
git add spread.py
git commit -m "U3 L05 range, variance, and standard deviation over donations"
git push
```

**Turn in**: Screenshot the push confirmation line (with the four numbers from
the lesson) and submit to Google Classroom. Due 11:59 PM tonight.

## Check your progress

```bash
python3 self_check.py
```

This prints a progress meter — it is a **progress meter**, not a pass/fail
grade. Green output means your journal is tracked; the checks tell you what's
done and what's next. Your push always succeeds regardless of the meter.

---

## Reflection journal

At the end of class, create your journal from the template:

```bash
cp journal-TEMPLATE.md journal-1123-$(whoami).md
```

Edit it with your answers, then commit and push it with your code. The CI
check will verify it on your next push.
