FROM ubuntu:22.04 AS build-python

ARG PYTHON_VERSION_MM=3.8
ARG PYTHON_VERSION=3.8.17

# Install essential build tools and necessary runtime libraries for Python compilation
RUN : \
    && apt-get update \
    && apt-get install -y \
        build-essential \
        wget \
        libffi-dev \
        libssl-dev \
        zlib1g-dev \
        liblzma-dev \
        libbz2-dev \
        libreadline-dev \
        libsqlite3-dev \
    && :

# Download, compile, and install Python from source code (to have this version available for both amd64 and armv7l)
WORKDIR /tmp
RUN : \
    && wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tar.xz \
    && tar xvf Python-${PYTHON_VERSION}.tar.xz \
    && cd Python-${PYTHON_VERSION} \
    && ./configure --enable-shared --enable-optimizations \
    && make -j$(nproc) \
    && make altinstall \
    && :

# Update symbolic links for python3 and pip3 (to be able to use commands without version suffix)
RUN : \
    && ln -s /usr/local/bin/python${PYTHON_VERSION_MM} /usr/local/bin/python3 \
    && ln -s /usr/local/bin/pip${PYTHON_VERSION_MM} /usr/local/bin/pip3 \
    && ln -s /usr/local/bin/python${PYTHON_VERSION_MM} /usr/local/bin/python \
    && ln -s /usr/local/bin/pip${PYTHON_VERSION_MM} /usr/local/bin/pip \
    && :

# -------------------------------------------------------------------------------------------------------------------------------
FROM ubuntu:22.04 as main

# Set up required development and runtime dependencies
RUN : \
    && apt-get update \
    && apt-get install -y \
        wget \
        libffi-dev \
        libssl-dev \
        zlib1g-dev \
        liblzma-dev \
        libbz2-dev \
        libreadline-dev \
        libsqlite3-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && :

# Copy Python binaries, libraries, includes, and virtualenv from the build-python stage
COPY --from=build-python /usr/local/ /usr/local/

# Update the dynamic linker run-time bindings (Python shared libraries are not in the default search path)
RUN ldconfig

# This prefers the Espressif pypi wheel registry (https://dl.espressif.com/pypi/) over the public PyPI (fallback)
RUN pip install --upgrade pip
ENV PIP_INDEX_URL=https://dl.espressif.com/pypi/
ENV PIP_EXTRA_INDEX_URL=https://pypi.org/simple

# Prioritize using binary (wheel) packages over source packages for faster installations and to avoid unnecessary compilations.
ENV PIP_PREFER_BINARY=true

CMD [ "/bin/bash" ]
