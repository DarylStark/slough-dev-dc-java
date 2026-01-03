# Slough - Dev Container - Java

Docker image for a development container for Java projects.

## Installed Java Versions

This dev container includes multiple OpenJDK versions from Eclipse Temurin:

- **OpenJDK 21** (LTS) - Default version
- **OpenJDK 23**

### Switching Between Java Versions

Use the `use-java` helper function to switch between installed Java versions:

```bash
# Switch to Java 21 (LTS - default)
use-java 21

# Switch to Java 23
use-java 23
```

The function will update `JAVA_HOME` and `PATH` to use the selected version and display the current Java version.

