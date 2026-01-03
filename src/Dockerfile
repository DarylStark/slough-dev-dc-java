# hadolint ignore=DL3007
FROM dast1986/slough-dev-dc-generic-base:1.0.0 AS base

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Switch to root user
USER root

# Install Java tools - Multiple OpenJDK versions (21, 23)
# Install prerequisites and ca-certificates
# hadolint ignore=DL3008,DL3009
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    curl && \
    update-ca-certificates && \
    # Download and install OpenJDK 21 (Temurin)
    curl -L -o /tmp/jdk-21.tar.gz https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.5%2B11/OpenJDK21U-jdk_x64_linux_hotspot_21.0.5_11.tar.gz && \
    echo "3c654d98404c073b8a7e66bffb27f4ae3e7ede47d13284c132d40a83144bfd8c  /tmp/jdk-21.tar.gz" | sha256sum -c - && \
    mkdir -p /usr/lib/jvm/temurin-21-jdk-amd64 && \
    tar -xzf /tmp/jdk-21.tar.gz -C /usr/lib/jvm/temurin-21-jdk-amd64 --strip-components=1 && \
    rm /tmp/jdk-21.tar.gz && \
    # Download and install OpenJDK 23 (Temurin)
    curl -L -o /tmp/jdk-23.tar.gz https://github.com/adoptium/temurin23-binaries/releases/download/jdk-23.0.1%2B11/OpenJDK23U-jdk_x64_linux_hotspot_23.0.1_11.tar.gz && \
    echo "2400267e4e9c0f6ae880a4d763af6caf18c673714bdee5debf8388b0b5d52886  /tmp/jdk-23.tar.gz" | sha256sum -c - && \
    mkdir -p /usr/lib/jvm/temurin-23-jdk-amd64 && \
    tar -xzf /tmp/jdk-23.tar.gz -C /usr/lib/jvm/temurin-23-jdk-amd64 --strip-components=1 && \
    rm /tmp/jdk-23.tar.gz && \
    # Configure update-alternatives to manage Java versions
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/temurin-21-jdk-amd64/bin/java 2100 \
        --slave /usr/bin/javac javac /usr/lib/jvm/temurin-21-jdk-amd64/bin/javac \
        --slave /usr/bin/jar jar /usr/lib/jvm/temurin-21-jdk-amd64/bin/jar && \
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/temurin-23-jdk-amd64/bin/java 2300 \
        --slave /usr/bin/javac javac /usr/lib/jvm/temurin-23-jdk-amd64/bin/javac \
        --slave /usr/bin/jar jar /usr/lib/jvm/temurin-23-jdk-amd64/bin/jar && \
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
