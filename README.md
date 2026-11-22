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

## What's here

- `spread.py` — **your file to create**. You will write functions for range,
  variance, and standard deviation, then run them on the donations dataset.
- `data/u3_donations.txt` — 200 alumni donation amounts (one per line). One
  extreme outlier (90000) is included on purpose.
- `README.md` — this file. You'll edit it at the end with your results.
- `journal-TEMPLATE.md` — copy this to make your reflection journal.
- `self_check.py` + `journal_check.yml` — CI progress meter (never pass/fail).
- `.github/workflows/grade.yml` — runs the check on every push.

## Your job

Follow the lesson parts in order. Each part tells you exactly what function to
write and what to test. Create `spread.py` as you go — do not rename it.

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

### PART 1 — Range (5 min)

Add to `spread.py`:

```python
def range_of(values):
    return max(values) - min(values)
```

Test it:

```python
from spread import range_of
print(f"range with outlier: {range_of(donations):.2f}")
print(f"range clean:        {range_of(clean):.2f}")
print(f"ratio: {range_of(donations) / range_of(clean):.1f}x")
```

### PART 2 — Variance (15 min)

Add to `spread.py`:

```python
def variance_of(values):
    m = sum(values) / len(values)
    total = sum((v - m) ** 2 for v in values)
    return total / len(values)
```

Test on the paper set first (should match 588.89), then on real data.

### PART 3 — Standard Deviation (10 min)

Add to `spread.py`:

```python
import math

def stddev_of(values):
    return math.sqrt(variance_of(values))
```

### PART 4 — Empirical Rule Check (10 min)

Add to `spread.py`:

```python
def within_one_sd(values):
    m = sum(values) / len(values)
    s = stddev_of(values)
    return sum(1 for v in values if m - s <= v <= m + s) / len(values) * 100
```

Print the percentage for the clean dataset.

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