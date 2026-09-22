# Intellectual Data Analysis — practical part

Three practical lessons taking you from NumPy arrays to fitted models and
the judgement to tell whether a result means anything. Each lesson is a
**practice** notebook worked through in class and a **homework** notebook
you complete yourself.

| Lesson | Topic                                |
|--------|--------------------------------------|
| 1      | Data representation and computation  |
| 2      | From raw table to analysable data    |
| 3      | Modeling: fitting, judging, choosing |

## Start here

**[`practice/README.md`](practice/README.md)** — how to open the
notebooks, how the homework works, how to submit it, and where the data
comes from. Read it before the first session.

## Running the notebooks

**Google Colab is the main tool.** A Google account is all you need: no
installation, no Python on your own machine, nothing to configure. Every
notebook opens from a link in `practice/README.md` §2.

**Running locally is supported too.**
[`dev-env/README.md`](dev-env/README.md) describes a small Docker image
with JupyterLab and the same libraries. Each dataset is loaded by a pair
of cells — one reading over the network for Colab, one reading from this
checkout — so the same notebook works either way, unchanged.

## Layout

```
practice/         the course: three lessons, six notebooks
  README.md       setup, homework and submission guide
  datasets/       the five CSV files, with provenance and licences
  1/ 2/ 3/        <n>-practice.ipynb and <n>-homework.ipynb
dev-env/          optional local Docker environment
```
