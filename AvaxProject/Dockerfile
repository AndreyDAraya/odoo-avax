FROM --platform=linux/amd64 odoo:18.0

LABEL maintainer="Agustin Wisky. <agustinwisky@gmail.com>"

USER root

# Update and install dependencies
RUN apt-get clean && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    apt-utils \
    python3 \
    python3-dev \
    python3-wheel \
    chromium \
    git \
    wget \
    curl \
    build-essential \
    cmake \
    sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Build Solidity compiler from source
RUN wget https://github.com/ethereum/solidity/releases/download/v0.8.4/solidity_0.8.4.tar.gz && \
    tar xvzf solidity_0.8.4.tar.gz && \
    mv solidity_0.8.4 solidity && \
    rm solidity_0.8.4.tar.gz

# Add docker user
RUN useradd -m docker && \
    echo "docker:docker" | chpasswd && \
    adduser docker sudo

# Install Solidity dependencies and build
RUN solidity/scripts/install_deps.sh && \
    solidity/scripts/build.sh

# Install Python requirements
COPY ./requirements.txt ./
RUN python3 -m pip install --no-cache-dir -r ./requirements.txt

# Set permissions
RUN chown -R odoo /mnt/*

# Set default user
USER odoo