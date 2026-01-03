# Slough - Dev Container - Java

Docker image for a development container optimized for Java projects.

## About Slough

This dev container is part of the **Slough project** by Daryl Stark, which provides consistent development tooling including dev containers across different programming languages and technology stacks. The goal of Slough is to deliver pre-configured, reproducible development environments that work seamlessly across different platforms and teams.

## Table of Contents

- [Using This Dev Container](#using-this-dev-container)
- [Container Configuration](#container-configuration)
- [Installed Tools](#installed-tools)
- [Working with Dev Containers](#working-with-dev-containers)
- [Troubleshooting](#troubleshooting)

## Using This Dev Container

### Image Tag

The Docker image for this dev container is available at:

```
dast1968/slough-dev-dc-java:1.0.0
```

### In VS Code

1. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) for VS Code
2. Create a `.devcontainer/devcontainer.json` file in your project root:

```json
{
  "name": "Java Development Container",
  "image": "dast1968/slough-dev-dc-java:1.0.0",
  "mounts": [
    "source=/var/run/docker.sock,target=/var/run/docker.sock,type=bind"
  ],
  "customizations": {
    "vscode": {
      "extensions": [
        "vscjava.vscode-java-pack"
      ]
    }
  }
}
```

3. Open your project in VS Code
4. Press `F1` and run **Dev Containers: Reopen in Container**

### With Docker CLI

Run the container directly with Docker:

```bash
docker run -it --rm \
  -v "$(pwd):/workspace" \
  -w /workspace \
  dast1968/slough-dev-dc-java:1.0.0
```

## Container Configuration

### User Configuration

- **Username**: `developer`
- **Home directory**: `/home/developer`
- **Sudo access**: Passwordless (no password required for `sudo` commands)
- **Shell**: Bash with custom configuration

### Pre-configured Features

- Starship prompt (customized with "Java" label)
- Git configuration support
- Docker socket mounting support for Docker-in-Docker scenarios
- Persistent bash history and configuration

## Installed Tools

### Java Development Kit (JDK)

This dev container includes multiple OpenJDK versions from Eclipse Temurin:

- **OpenJDK 21** (LTS) - Default version
- **OpenJDK 23** - Latest release

#### Switching Between Java Versions

Use the `use-java` helper function to switch between installed Java versions:

```bash
# Switch to Java 21 (LTS - default)
use-java 21

# Switch to Java 23
use-java 23
```

The function will update `JAVA_HOME` and `PATH` to use the selected version and display the current Java version.

#### Verifying Java Installation

Check your current Java version:

```bash
java -version
javac -version
echo $JAVA_HOME
```

### Additional Tools from Base Image

This container inherits common development tools from the Slough generic base image, including:

- **Git** - Version control system
- **curl** and **wget** - HTTP clients
- **Docker CLI** - For Docker-in-Docker scenarios (when Docker socket is mounted)
- **Build essentials** - Common build tools and compilers
- **Text editors** - vim, nano
- **Starship** - Cross-shell prompt with custom configuration

## Working with Dev Containers

### General Tips

1. **First-time setup**: The first time you open a project in a dev container, Docker will pull the image, which may take a few minutes depending on your internet connection.

2. **File permissions**: Files created in the container are owned by the `developer` user (UID 1000), which typically matches your host user on Linux and macOS.

3. **Extensions**: VS Code extensions installed in the container are separate from your local extensions. You may need to install or configure extensions specifically for the container environment.

4. **Port forwarding**: VS Code automatically forwards ports from the container to your host. You can also manually forward ports in the "Ports" panel.

### Tips for Microsoft Windows

#### Use WSL 2 Backend

For the best performance and compatibility on Windows, ensure you're using Docker Desktop with the WSL 2 backend:

1. Install [WSL 2](https://learn.microsoft.com/windows/wsl/install)
2. Install [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
3. Enable WSL 2 in Docker Desktop settings: Settings → General → "Use the WSL 2 based engine"

#### File System Performance

- **Store your code in WSL**: Keep your project files in the WSL file system (e.g., `~/projects/`) rather than the Windows file system (`/mnt/c/`). This significantly improves file I/O performance.
- **Access from Windows**: You can still access WSL files from Windows Explorer at `\\wsl$\<distro-name>\home\<username>\`

#### Line Endings

- Configure Git to use Linux line endings (LF) when working with dev containers:
  ```bash
  git config --global core.autocrlf input
  ```

#### Windows Paths with Docker Socket

When mounting the Docker socket on Windows with WSL 2, the path should be:
```json
{
  "mounts": [
    "source=/var/run/docker.sock,target=/var/run/docker.sock,type=bind"
  ]
}
```

This works automatically when using WSL 2 backend.

### Building Your Java Project

The container provides a clean environment for building Java projects with your build tool of choice:

#### Maven
```bash
# If your project uses Maven
mvn clean install
mvn test
mvn package
```

#### Gradle
```bash
# If your project uses Gradle
./gradlew build
./gradlew test
./gradlew clean build
```

#### Plain Java
```bash
# Compile Java files directly
javac -d bin src/**/*.java
java -cp bin com.example.Main
```

## Troubleshooting

### Container Won't Start

- Ensure Docker is running on your host system
- Check that you have the latest version of VS Code and the Dev Containers extension
- Verify that the Docker image tag is correct and accessible

### Permission Issues

- If you encounter permission issues with files, ensure the `developer` user has appropriate access
- Use `sudo` for operations requiring root privileges (no password required)

### Java Version Issues

- Always verify your active Java version with `java -version` before building
- Use `use-java <version>` to switch to the required version for your project
- Remember that `JAVA_HOME` is automatically updated when switching versions

### Docker Socket Access

- If Docker commands don't work inside the container, ensure the Docker socket is properly mounted in your `devcontainer.json`
- On Windows, confirm you're using Docker Desktop with WSL 2 backend

### Performance on Windows

- Move your project to the WSL 2 file system for better performance
- Ensure Docker Desktop has sufficient resources allocated (Settings → Resources)
- Consider excluding large directories (like `target` or `build`) from antivirus scanning

## Contributing

This project is part of the Slough development tooling suite. For issues, suggestions, or contributions, please visit the [GitHub repository](https://github.com/DarylStark/slough-dev-dc-java).

## License

See [LICENSE.md](LICENSE.md) for license information.

---

**Slough Project** by Daryl Stark - Consistent development tooling for modern software development.
