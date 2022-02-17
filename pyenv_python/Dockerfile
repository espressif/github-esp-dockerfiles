FROM debian:bullseye
ARG DEBIAN_FRONTEND=noninteractive

# Install system packages
# -----------------------------------------------------------------------------------------
RUN : \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \
        build-essential \
        ca-certificates \
        curl \
        git \
        libbz2-dev \
        libffi-dev \
        liblzma-dev \
        libncurses5-dev \
        libreadline-dev \
        libsqlite3-dev \
        libssl-dev \
        libxml2-dev \
        libxmlsec1-dev \
        llvm \
        make \
        python \
        tk-dev \
        wget \
        xz-utils \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && :


# Install Pyenv
# -----------------------------------------------------------------------------------------
ENV HOME="/root"
WORKDIR ${HOME}

RUN update-ca-certificates --fresh
RUN git clone --depth=1 https://github.com/pyenv/pyenv.git .pyenv

ENV PYENV_ROOT="${HOME}/.pyenv"
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${PATH}"

# Install Python versions 
# -----------------------------------------------------------------------------------------
ARG PYTHON_VERSIONS="\
    3.6.15 \
    3.7.10 \
    3.8.12 \
    3.9.9 \
    3.10.2 \
    "

RUN for item in $PYTHON_VERSIONS; \
        do \
            echo "Installing Python version: $item"; \
            pyenv install $item; \
        done;
