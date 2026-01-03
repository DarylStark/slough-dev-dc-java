# hadolint ignore=DL3007
FROM dast1986/slough-dev-dc-generic-base:latest AS base

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Switch to root user
USER root

# Install Java tools - Multiple OpenJDK versions (21, 23, 24, 25)
# Install prerequisites and ca-certificates
# hadolint ignore=DL3008,DL3009
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    curl && \
    update-ca-certificates && \
    # Download and install OpenJDK 21 (Temurin)
    curl -L -o /tmp/jdk-21.tar.gz https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.5%2B11/OpenJDK21U-jdk_x64_linux_hotspot_21.0.5_11.tar.gz && \
    mkdir -p /usr/lib/jvm/temurin-21-jdk-amd64 && \
    tar -xzf /tmp/jdk-21.tar.gz -C /usr/lib/jvm/temurin-21-jdk-amd64 --strip-components=1 && \
    rm /tmp/jdk-21.tar.gz && \
    # Download and install OpenJDK 23 (Temurin)
    curl -L -o /tmp/jdk-23.tar.gz https://github.com/adoptium/temurin23-binaries/releases/download/jdk-23.0.1%2B11/OpenJDK23U-jdk_x64_linux_hotspot_23.0.1_11.tar.gz && \
    mkdir -p /usr/lib/jvm/temurin-23-jdk-amd64 && \
    tar -xzf /tmp/jdk-23.tar.gz -C /usr/lib/jvm/temurin-23-jdk-amd64 --strip-components=1 && \
    rm /tmp/jdk-23.tar.gz && \
    # Download and install OpenJDK 24 (Temurin) - Early Access
    curl -L -o /tmp/jdk-24.tar.gz https://github.com/adoptium/temurin24-binaries/releases/download/jdk-24%2B25-ea-beta/OpenJDK24U-jdk_x64_linux_hotspot_24_25.tar.gz && \
    mkdir -p /usr/lib/jvm/temurin-24-jdk-amd64 && \
    tar -xzf /tmp/jdk-24.tar.gz -C /usr/lib/jvm/temurin-24-jdk-amd64 --strip-components=1 && \
    rm /tmp/jdk-24.tar.gz && \
    # Download and install OpenJDK 25 (Temurin) - Early Access
    curl -L -o /tmp/jdk-25.tar.gz https://github.com/adoptium/temurin25-binaries/releases/download/jdk-25%2B5-ea-beta/OpenJDK25U-jdk_x64_linux_hotspot_25_5.tar.gz && \
    mkdir -p /usr/lib/jvm/temurin-25-jdk-amd64 && \
    tar -xzf /tmp/jdk-25.tar.gz -C /usr/lib/jvm/temurin-25-jdk-amd64 --strip-components=1 && \
    rm /tmp/jdk-25.tar.gz && \
    # Configure update-alternatives to manage Java versions
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/temurin-21-jdk-amd64/bin/java 2100 \
        --slave /usr/bin/javac javac /usr/lib/jvm/temurin-21-jdk-amd64/bin/javac \
        --slave /usr/bin/jar jar /usr/lib/jvm/temurin-21-jdk-amd64/bin/jar && \
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/temurin-23-jdk-amd64/bin/java 2300 \
        --slave /usr/bin/javac javac /usr/lib/jvm/temurin-23-jdk-amd64/bin/javac \
        --slave /usr/bin/jar jar /usr/lib/jvm/temurin-23-jdk-amd64/bin/jar && \
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/temurin-24-jdk-amd64/bin/java 2400 \
        --slave /usr/bin/javac javac /usr/lib/jvm/temurin-24-jdk-amd64/bin/javac \
        --slave /usr/bin/jar jar /usr/lib/jvm/temurin-24-jdk-amd64/bin/jar && \
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/temurin-25-jdk-amd64/bin/java 2500 \
        --slave /usr/bin/javac javac /usr/lib/jvm/temurin-25-jdk-amd64/bin/javac \
        --slave /usr/bin/jar jar /usr/lib/jvm/temurin-25-jdk-amd64/bin/jar && \
    # Set Java 21 (LTS) as default
    update-alternatives --set java /usr/lib/jvm/temurin-21-jdk-amd64/bin/java && \
    # Clean up
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Switch back to developer user
USER developer

# Copy local configuration files
COPY --chown=developer:developer .bashrc.local /home/developer/.bashrc.local

# Configure Starship
ENV PROMPTNAME="Java"

# Set the default command to bash
CMD ["bash"]
