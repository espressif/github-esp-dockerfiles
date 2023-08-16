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
        libunwind8 \
        awscli \
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

# Install Python versions and boto3 package
# -----------------------------------------------------------------------------------------
ARG PYTHON_VERSIONS="\
    3.7.10 \
    3.8.17 \
    3.9.17 \
    3.10.12 \
    3.11.4 \
    "

RUN for item in $PYTHON_VERSIONS; \
        do \
            echo "Installing Python version: $item"; \
            pyenv install $item; \
            pyenv local $item; \
            echo "Installing boto3 for Python version: $item"; \
            python -m pip install boto3; \
        done;

SHELL ["/bin/bash", "-c"]
RUN : \
    && set -ex \
    &&  if [ `uname -m` == "armv7l" ]; then :\
            && wget -O powershell-7.2.2.tar.gz https://github.com/PowerShell/PowerShell/releases/download/v7.2.2/powershell-7.2.2-linux-arm32.tar.gz \
            ;\
            fi \
    &&  if [ `uname -m` == "aarch64" ]; then :\
            && wget -O powershell-7.2.2.tar.gz https://github.com/PowerShell/PowerShell/releases/download/v7.2.2/powershell-7.2.2-linux-arm64.tar.gz \
            ;\
            fi \
    &&  if [ `uname -m` == "x86_64" ]; then :\
            && wget -O powershell-7.2.2.tar.gz https://github.com/PowerShell/PowerShell/releases/download/v7.2.2/powershell-7.2.2-linux-x64.tar.gz \
            ;\
            fi \
    && mkdir ~/powershell \
    && tar xf powershell-7.2.2.tar.gz -C ~/powershell \
    && ln -s ~/powershell/pwsh /usr/bin/pwsh \
    && ln -s ~/powershell/pwsh /usr/local/bin/powershell \
    && pwsh --version \
    && :

# Install Rust
# -----------------------------------------------------------------------------------------
RUN : \
    && curl https://sh.rustup.rs -sSf | bash -s -- --default-toolchain stable -y \
    && . $HOME/.cargo/env \
    && rustc --version \
    && :
