FROM ubuntu:25.04

# Install essential packages including openjdk and utilities
RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    xz-utils \
    zip \
    libglu1-mesa \
    openjdk-17-jdk \
    bash \
    wget \
    && rm -rf /var/lib/apt/lists/*

CMD ["bash"]

