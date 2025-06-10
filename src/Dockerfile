# hadolint ignore=DL3007
FROM dast1986/slough-dev-dc-generic-base:latest AS base

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Switch to root user
USER root

# Intall Java tools
# hadolint ignore=DL3008
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    openjdk-17-jdk && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists

# Switch back to developer user
USER developer

# Copy local configuration files
COPY --chown=developer:developer .bashrc.local /home/developer/.bashrc.local

# Configure Starship
ENV PROMPTNAME="Java"

# Set the default command to bash
CMD ["bash"]
