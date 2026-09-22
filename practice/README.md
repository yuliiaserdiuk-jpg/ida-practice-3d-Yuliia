# Intellectual Data Analysis — practical part

Three lessons. Each has a **practice** notebook, worked through in class,
and a **homework** notebook you complete yourself.

## 1. What this folder is

The three lessons build one pipeline, in order, and each begins where the
previous one ended.

**Lesson 1 — Data representation and computation.** How a number, a
column and a cloud of points are stored, and what operations on them
cost. NumPy arrays, views against copies, broadcasting, reductions,
missing values, floating point, seeded random generation, the
distributions this course uses, and matplotlib. It ends by generating two
small datasets whose true parameters you know.

**Lesson 2 — From raw table to analysable data.** The same arrays with an
index and column names attached, which is what makes missing values,
mixed types and categories possible. Selection, missingness as a pattern
with a mechanism, grouping, dates, charts chosen from the question being
asked, correlation, encoding, scaling, PCA and t-SNE. It ends with a
cleaned Titanic table and a written record of every decision that
produced it.

**Lesson 3 — Modeling: fitting, judging, choosing.** Regression, trees,
k nearest neighbours, support vector machines and clustering — and, more
importantly, how to tell whether a reported number means anything.
Cross-validation, pipelines, the bias-variance trade-off, classification
metrics, and one honest end-to-end run that finishes with a decision
rather than a leaderboard.

| Lesson | Practice | Homework | Class time | Homework time |
|---|---|---|---|---|
| 1 | Data representation and computation | 11 tasks, 5 written questions | 90-120 min | 3-4 h |
| 2 | From raw table to analysable data | 12 tasks, 8 written questions | 90-120 min | 3-4 h |
| 3 | Modeling: fitting, judging, choosing | 10 tasks, 7 written questions | 90-120 min | 3-4 h |

The last task of each homework is open-ended and has no assert; the rest
carry `assert` self-checks.

## 2. Opening a notebook in Colab

You need a Google account and nothing else. No installation, no Python on
your own machine.

Every notebook in this repository opens in Colab through a URL of this
shape:

```
https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/<lesson>/<file>.ipynb
```

All six notebooks, two per lesson:

| Lesson | Practice | Homework |
|---|---|---|
| 1 | [1-practice.ipynb](https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/1/1-practice.ipynb) | [1-homework.ipynb](https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/1/1-homework.ipynb) |
| 2 | [2-practice.ipynb](https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/2/2-practice.ipynb) | [2-homework.ipynb](https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/2/2-homework.ipynb) |
| 3 | [3-practice.ipynb](https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/3/3-practice.ipynb) | [3-homework.ipynb](https://colab.research.google.com/github/djnzx/ida-practice-3d/blob/main/practice/3/3-homework.ipynb) |

Each notebook also carries an "Open in Colab" badge in its first cell.

**Use those links, or the badge — not File → Open notebook → GitHub.** The
repository is public, so a direct link needs no GitHub sign-in: a Google
account is enough to open and run everything here. Submitting is
different and does need a GitHub account, but only at the end — see §8.
The Open-notebook dialog
works differently: it lists repositories on your behalf, so it asks you to
authorize Colab against GitHub. You do not need to grant that, and granting
it will not make anything here work better. If you ever see a GitHub
authorization prompt while opening one of these notebooks, close it and
check the link instead — the usual cause is a mistyped or outdated address,
which Colab cannot distinguish from a private file it has no access to.

## 3. Save a copy in Drive before you edit anything

**This is the single most common way students lose their homework.**

A notebook opened from GitHub is opened **read-only**. Colab will let you
type in it and run cells, and it looks exactly like a normal editable
notebook. When you close the tab, everything you wrote is gone. There is
no recovery and no autosave, because there was never anywhere to save to.

Before you type a single character:

**File → Save a copy in Drive**

A new tab opens, titled `Copy of 3-homework.ipynb`, stored in your own
Google Drive under `My Drive/Colab Notebooks/`. Work in that tab. Close
the original.

Do this every time you open a homework notebook.

## 4. The runtime

A **runtime** is the temporary virtual machine that executes your cells.
Colab gives you one when you first run something and takes it away again
later. Three consequences:

- **It resets.** When the runtime is recycled, every variable you defined
  is gone. Your notebook text is safe (if you saved it to Drive, §3); the
  *state* is not. Recovering is one action: Runtime → Restart and run all.
- **Idle disconnects happen.** Leave a tab alone for a while and Colab
  reclaims the machine. This is normal and not an error.
- **You do not need a GPU for this course.** Leave the runtime type on
  CPU. Every workload here is small tabular data and classical
  scikit-learn estimators, which are CPU-bound and mostly
  single-threaded; a GPU would sit idle. Selecting one only makes you
  wait longer for a free machine.

The slowest notebook in this course is the Lesson 3 practice notebook,
which runs end to end in well under a minute. If a single cell has been
running for several minutes, see §9.

## 5. Running cells, and why the order matters

- **Run one cell:** Shift+Enter, or the play button to its left.
- **Run everything from the top:** Runtime → Run all.
- **Start clean and run everything:** Runtime → Restart and run all.

To the left of each code cell is a counter in brackets. `[ ]` means never
run. `[*]` means running now. `[7]` means it was the seventh cell
executed **in this runtime**, which is not necessarily the seventh cell
on the page.

That distinction is the whole point. Colab lets you run cells in any
order, and the notebook remembers results, not positions. So a notebook
whose counters read `[1] [5] [2] [4] [3]` produced its output through a
sequence nobody can reconstruct — including you, tomorrow. If you
redefine a variable in cell 30 and then re-run cell 12, cell 12 now uses
the new value, and the page gives no indication of it.

Practice 1 §4.1 makes this concrete: the notebooks draw random numbers from
a single generator created at the top, and a generator advances every
time you draw from it. Running cells out of order therefore changes the
numbers, silently.

**The check: Runtime → Restart and run all.** If the notebook completes
from a clean runtime with no errors, the results on the page are the
results the code produces. If it does not, they were an artifact of your
click history. Do this before submitting anything (§8).

## 6. Where the data comes from

Every dataset in this course is a real file, committed to this
repository, and read from its complete address:

```python
titanic = pd.read_csv(
    "https://raw.githubusercontent.com/djnzx/ida-practice-3d/main/practice/datasets/titanic.csv"
)
```

That address is written out **in full, every single time**, with no
`DATA_URL` constant and no string concatenation. It is repetitive on
purpose.

Directly beneath each such cell is a second one marked `LOCAL
ALTERNATIVE`, which reads the same file from `../datasets/` instead. It
exists for students running the notebooks locally (§10). **Run one cell or
the other, not both.** On Colab, always the first.

**Why not a loader function.** Many tutorials begin `from sklearn.datasets
import load_wine`, and a student who has only ever done that has never
acquired a dataset. Real data sits somewhere specific, in a format
somebody else chose, and the first act of any analysis is saying where it
is and parsing what comes back. No notebook in this course calls
`load_iris`, `load_wine`, `load_digits`, `make_blobs`, `make_moons` or
`make_circles`. Synthetic data is generated by code you can read, in the
notebook, so you can see exactly which assumption a method later violates.

**Why a raw GitHub URL and not a Drive link.** A
`drive.google.com/uc?id=1a2b3c...` link names an opaque identifier. You
cannot tell what it points at, whether it changed yesterday, or which
version produced a published result. A raw GitHub URL names a repository,
a branch and a path; the file it serves is under version control, and its
entire history is inspectable. You can also pin an exact commit by
replacing `main` with a commit hash, which makes the address permanent.

**If the repository moves**, every data URL in every notebook breaks
together. Search for `raw.githubusercontent.com/djnzx/ida-practice-3d` and
replace the owner and repository name. That is the cost of writing the
address out in full, and it is smaller than the cost of not knowing where
your data came from.

**Per-file provenance** — original source, date retrieved, licence, and
any transformation applied — is in
[`datasets/README.md`](datasets/README.md). Five datasets are used:
Titanic, Iris and Wine, plus a 1500-row sample of MNIST and two years of
hourly bicycle rentals from Washington D.C.

## 7. Doing the homework

Each homework notebook has the same structure.

**TODO cells.** A function signature and a docstring stating exactly what
to return, with the body replaced by `raise NotImplementedError`. Delete
that line and write the implementation. Everything around it — imports,
data loading, plotting of your result — is already written, so your time
goes on the idea rather than on boilerplate.

**Assert cells.** Immediately after each task, a cell of `assert`
statements that check your function's behaviour: its output shape, its
types, known values, invariants. Run it. If it prints `Task N passed.`,
you are done with that task.

**A failing assert is information, not a grade.** Nobody is counting how
many times you ran the cell. The message tells you which property does
not yet hold; read it, fix the function, run it again. The assertions
test *behaviour*, never the particular way you wrote the code, so any
correct implementation passes.

**Written questions.** Markdown cells with an empty answer cell beneath.
These are marked, and they carry the real learning objective. Every one
asks you to run something two ways and explain the difference — not to
recall a fact. Answers must refer to numbers you actually produced. One
sentence will not be enough; a paragraph that cites your own output will.

**No searching required.** Every function a task needs is demonstrated in
that lesson's practice notebook, and each task names the section, like
`see Practice 2 §5.4`. If you find yourself wanting to search the web for
a function signature, the reference is almost certainly in the practice
notebook.

**The final task of each homework is open-ended** and has no assert. It
is marked entirely on the written analysis.

## 8. Submitting

You submit a **committed file in your own fork** of this repository. Fork
it once, at the start of the course, from the repository page on GitHub;
after that every submission is a commit to that same fork.

1. **Runtime → Restart and run all.** This is a requirement, not a
   suggestion. Every cell must execute in order, from a clean runtime,
   with no errors and every assert passing.
2. Check that every written question has an answer.
3. **File → Download → Download .ipynb.** Colab names the download after
   the notebook you were working in, so it usually arrives as
   `Copy of 2-homework.ipynb`.
4. **Rename it to `<n>-homework-results.ipynb`**, where `<n>` is the
   lesson number. Lesson 2's homework becomes `2-homework-results.ipynb`.
5. **Commit it to your fork**, in that lesson's folder, next to the
   homework it answers — so `practice/2/2-homework-results.ipynb`.
6. Open a pull request against this repository, or send the link to your
   fork.

Submit the notebook **with its outputs**: do not clear them. The outputs
are the evidence that the asserts passed on your machine.

A notebook that fails restart-and-run-all is not finished, whatever it
looks like on your screen.

## 9. Troubleshooting

| What you see | What it means | What to do |
|---|---|---|
| `NameError: name 'rng' is not defined` | The runtime restarted, or you ran a cell before the one that defines the name. The variable was never created in *this* runtime. | Runtime → Run all. Do not just re-run the failing cell. |
| `ModuleNotFoundError` | An import Colab does not ship. Nothing in this course needs one — every notebook uses only numpy, pandas, matplotlib, seaborn, scipy and scikit-learn. | Check the import line for a typo. Do not `pip install`; if a genuinely missing package is needed, the notebook has a bug — report it. |
| `HTTPError: 404` on a `read_csv` | The data URL did not resolve: a typo in the path, or the repository or branch was renamed. | Paste the URL into a browser. If it 404s there too, compare it against §6 character by character. |
| A cell produces no figure | Either the cell has no `plt.show()`, or the figure was drawn onto an `Axes` from a different cell that has since been replaced. | Re-run from the cell that creates the figure. If you edited plotting code, Runtime → Run all. |
| `SettingWithCopyWarning` | You assigned into something that may be a copy of a slice rather than the original. The assignment may have silently done nothing. | Use a single `frame.loc[rows, column] = value`. See Practice 2 §4.2, which triggers this deliberately and then fixes it. |
| `ConvergenceWarning` | An iterative fit hit its iteration limit before settling. The result exists but may not be the optimum. | Usually means unscaled features: put a `StandardScaler` in the pipeline. Otherwise raise `max_iter`. See Practice 2 §14. |
| A cell has run for minutes | Something is much larger than intended — a grid search over too many combinations, a decision-boundary mesh too fine, or t-SNE on too many rows. | Interrupt it (Runtime → Interrupt execution). Check the sizes against the runtime notes in the practice notebook; Practice 3 §12.3 explains what was reduced and why. |
| Everything is disconnected | The runtime was idle too long and was reclaimed. Normal. | Reconnect, then Runtime → Run all. |
| Your numbers differ from the instructor's | Almost always out-of-order execution (§5). | Runtime → Restart and run all, then compare again. |

## 10. Working locally instead

Colab is the supported path for this course. If you cannot use it,
[`dev-env/`](../dev-env/) at the root of this repository holds a local
Docker setup with Jupyter and the same libraries; its own README has the
three commands you need.

Each dataset is loaded by a pair of cells and you run one of them: the
first fetches it over HTTPS, the second — marked `LOCAL ALTERNATIVE` —
reads the same file from the checkout. On Colab you always use the first.
Locally either works, and the local one needs no network.
