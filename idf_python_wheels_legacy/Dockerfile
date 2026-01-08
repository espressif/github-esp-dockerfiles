# Minimal manylinux-compatible Docker image for ARMv7l (32-bit ARM)
# Based on Debian Buster for older glibc compatibility (2.28)
# Use this for compatibility with older systems (e.g., older Raspberry Pi OS)

FROM arm32v7/debian:buster

LABEL maintainer="Espressif Systems"
LABEL description="Minimal manylinux-compatible environment for building Python wheels on ARMv7l (legacy glibc 2.28)"

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive \
    LC_ALL=C.UTF-8 \
    LANG=C.UTF-8

# Buster is EOL - archive repositories
RUN sed -i 's|deb.debian.org|archive.debian.org|g' /etc/apt/sources.list && \
    sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list && \
    sed -i '/buster-updates/d' /etc/apt/sources.list

# Install build dependencies and Python versions
RUN apt-get update && apt-get install -y \
    # Build essentials
    build-essential \
    gcc \
    g++ \
    make \
    cmake \
    git \
    wget \
    curl \
    ca-certificates \
    patchelf \
    # Python build dependencies
    libssl-dev \
    libffi-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libgdbm-dev \
    liblzma-dev \
    tk-dev \
    zlib1g-dev \
    # Additional libraries commonly needed for wheels
    libxml2-dev \
    libxslt1-dev \
    libyaml-dev \
    libglib2.0-dev \
    # PyGObject dependencies
    libcairo2-dev \
    pkg-config \
    libgirepository1.0-dev \
    # dbus-python dependencies
    libdbus-1-dev \
    libdbus-glib-1-dev \
    # Pillow dependencies
    libjpeg-dev \
    libpng-dev \
    libtiff-dev \
    libfreetype6-dev \
    liblcms2-dev \
    libwebp-dev \
    libopenjp2-7-dev \
    libfribidi-dev \
    libharfbuzz-dev \
    libxcb1-dev \
    libxau-dev \
    # Python from Debian repos (Buster provides 3.7)
    python3 \
    python3-dev \
    python3-venv \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install pip and wheel tools
RUN python3 -m pip install --no-cache-dir --upgrade \
    pip \
    setuptools \
    wheel \
    auditwheel

# Set up manylinux platform tag
# This makes auditwheel recognize the environment as manylinux-compatible
# Buster has glibc 2.28, so we use manylinux_2_28
RUN echo "manylinux_2_28_armv7l" > /etc/system-release-cpe

# Create a marker file for manylinux detection
RUN mkdir -p /opt/python && \
    echo "manylinux_2_28_armv7l" > /opt/python/PLATFORM

# Set working directory
WORKDIR /workspace

# Default Python
ENV PATH="/usr/bin:${PATH}"

CMD ["/bin/bash"]

