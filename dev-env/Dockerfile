# Local stand-in for Google Colab, for students who cannot use Colab.
#
# Colab is the supported path for this course; this image exists so the
# notebooks can be run offline. It therefore installs the Colab Python
# version and the six libraries the notebooks import, and nothing else.
#
# Deliberately NOT here: nltk and its corpora, wordcloud, statsmodels,
# tensorflow, torch, transformers. No notebook in practice/ imports any of
# them, and an NLTK corpus alone would add about 5 GB.

# Colab runs Python 3.12. Matching it keeps a syntax or stdlib difference
# from showing up here and not there.
FROM python:3.12-slim

ARG NB_USER=jupyter
ARG NB_UID=1000
ARG NB_GID=100
RUN useradd -m -u ${NB_UID} -g ${NB_GID} -s /bin/bash ${NB_USER}

# tini reaps the kernel processes Jupyter spawns, so Ctrl-C and `docker stop`
# shut the container down cleanly instead of leaving zombies.
# fonts-dejavu gives matplotlib a real font; without it every figure falls
# back to a bitmap font and the axis labels look wrong.
RUN apt-get update && apt-get install -y --no-install-recommends \
        tini \
        fonts-dejavu-core \
 && rm -rf /var/lib/apt/lists/*

ENV PIP_DISABLE_PIP_VERSION_CHECK=1 \
    PIP_NO_CACHE_DIR=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# No build toolchain is installed. Every package below ships a prebuilt
# wheel for CPython 3.12 on manylinux, so nothing is compiled from source.
# If a future version ever loses its wheel, the build will fail loudly here
# rather than silently producing a 1.5 GB image full of compilers.
#
# Versions are intentionally unpinned so the image tracks current releases,
# the way Colab does. Each notebook prints its library versions in its first
# cell, so a drift between this image and Colab is visible immediately.
RUN python -m pip install --upgrade pip \
 && python -m pip install \
        jupyterlab \
        numpy \
        pandas \
        matplotlib \
        seaborn \
        scipy \
        scikit-learn \
 && python -c "import numpy, pandas, matplotlib, seaborn, scipy, sklearn; print('numpy, pandas, matplotlib, seaborn, scipy, sklearn imported cleanly')"

# Create the mount point before dropping privileges, so it belongs to the
# notebook user. Compose bind-mounts over it, but a plain `docker run`
# without a mount would otherwise leave a root-owned directory that
# Jupyter cannot write to.
RUN mkdir -p /workspace && chown ${NB_UID}:${NB_GID} /workspace

USER ${NB_USER}
WORKDIR /workspace
EXPOSE 8888

# Empty token means no password prompt, which is what you want on a laptop.
# Set one with `JUPYTER_TOKEN=... docker compose up` to require it.
ENV JUPYTER_TOKEN=""

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["sh", "-c", "jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --IdentityProvider.token=\"$JUPYTER_TOKEN\""]
