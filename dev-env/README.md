# Local environment

A Docker image that runs the course notebooks on your own machine, for
when Colab is not an option.

**Colab is the supported path.** Use it unless you cannot — see
[`../practice/README.md`](../practice/README.md). This image exists as a
fallback and is kept deliberately small.

## Quick start

You need Docker installed and running. From this folder:

```shell
make build     # once, or after the Dockerfile changes
make up        # start the container, with JupyterLab, in the background
make open      # open JupyterLab in your browser
```

When you are done:

```shell
make down
```

`make` on its own lists every target.

## What you get

JupyterLab at <http://localhost:8888>, with `../practice` mounted at
`/workspace` — so the three lessons and their homework are the first thing
you see when the browser opens.

No token is set, so there is no password prompt. If you want one:

```shell
JUPYTER_TOKEN=somesecret make up
```

## What is installed

Python 3.12, matching Colab, and the six libraries the notebooks import:

| | |
|---|---|
| `numpy` | arrays, the whole of Lesson 1 |
| `pandas` | tables, Lesson 2 |
| `matplotlib` | every figure |
| `seaborn` | the statistical plots in Lesson 2 |
| `scipy` | one distribution in Lesson 3 |
| `scikit-learn` | every model in Lesson 3 |

Plus `jupyterlab` to run them. That is the complete list.

Versions are not pinned, so the image tracks current releases the way
Colab does. To see what you actually have:

```shell
make versions
```

Every notebook also prints its own versions in its first cell, so if a
result differs between this image and Colab, the first diagnostic is
already on screen.

## What is deliberately absent

No `nltk`, `wordcloud`, `statsmodels`, `tensorflow`, `torch` or
`transformers`. Nothing in `practice/` imports them, and an NLTK corpus
alone would add roughly 5 GB to the image.

There is also no C toolchain. Every package above ships a prebuilt wheel
for Python 3.12, so nothing is compiled during the build. If you add a
dependency that has no wheel, the build will fail rather than quietly
pulling in a compiler — install `build-essential` in the Dockerfile at
that point, knowingly.

## Two ways to load the data

Every dataset is loaded by a pair of adjacent cells, and you run **one of
them**:

- the first reads from a full `raw.githubusercontent.com` URL. This is
  what Colab uses, and it needs network access to GitHub.
- the second is marked `LOCAL ALTERNATIVE` and reads the same file from
  `../datasets/` in this checkout. Nothing leaves the machine.

Run the local cells here and the notebooks work with no network at all.
The data is identical either way — same files, same bytes — so results do
not depend on which you pick. See
[`../practice/README.md`](../practice/README.md) §6 for why the URL is
written out in full in the first place.

## Targets

| Target | What it does |
|---|---|
| `make build` | Build the image |
| `make up` | Start the container in the background |
| `make down` | Stop and remove the container |
| `make restart` | Restart it |
| `make logs` | Follow the log, including the startup URL |
| `make shell` | A shell inside the running container |
| `make status` | Is it running |
| `make open` | Open the browser |
| `make versions` | Print the installed library versions |
| `make clean` | Remove the container and the image |

## Troubleshooting

| Problem | Fix |
|---|---|
| `docker: command not found` | Install Docker Desktop and start it. |
| `Cannot connect to the Docker daemon` | Docker is installed but not running. Start Docker Desktop and retry. |
| Port 8888 already in use | Something else holds the port — often a previous container. `make down`, or change the host side of `ports` in `docker-compose.yml` to e.g. `"8899:8888"`. |
| Browser asks for a token | You set `JUPYTER_TOKEN`. Use that value, or `make down` and `make up` without it. |
| `HTTPError` when a notebook loads data | You ran the URL cell without network access to GitHub. Run the `LOCAL ALTERNATIVE` cell directly beneath it instead — same file from `../datasets/`, no network needed. |
| Edits vanish after `make down` | Only `../practice` is mounted. Anything written elsewhere in the container is lost when it is removed. Save your work under `/workspace`. |
