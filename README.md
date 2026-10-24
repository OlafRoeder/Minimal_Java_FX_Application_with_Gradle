# Minimal Java FX Application

Template for a minimal Java FX Application built with Gradle. It opens a window
containing **Hello, world!**.

## Requirements

* Install a stable JDK 27 and set `JAVA_HOME` to its installation directory.
* Uses Gradle 9.8.1 through the complete checked-in wrapper; no separate Gradle installation is needed.
* Uses JavaFX 27 and the OpenJFX Gradle plugin 0.1.0. The plugin selects the
  platform-specific JavaFX dependencies and native libraries automatically;
  no separately downloaded JavaFX SDK is required.
* A graphical desktop is required. Linux also needs GTK 3 and its native libraries.
  For headless interactive runs, use a virtual display such as Xvfb.

The Java toolchain, Gradle daemon criteria, and CI JDK all require Java 27.
In IntelliJ IDEA, select JDK 27 as both the Project SDK and Gradle JVM under
Settings > Build, Execution, Deployment > Build Tools > Gradle. IDE settings
remain local. The daemon criteria require an installed JDK; automatic JDK
downloads are not configured.

## How to start

Clone the repository, then run:

```powershell
.\gradlew.bat run
```

On Linux or macOS:

```sh
sh ./gradlew run
```

Or use **Tasks > application > run** in a Gradle GUI. Close the window to exit.
The build launches the named module `minimal.javafx.application` and enables
native access specifically for `javafx.graphics`. Use the generated launcher
rather than `java -jar`, which does not configure the JavaFX module path.

## Verification and distribution

```powershell
.\gradlew.bat clean build --warning-mode fail
.\gradlew.bat installDist
```

The build compiles and packages the application; it does not open a window.
There are no automated tests. Run the application manually to verify the GUI.
Distributions contain native libraries for the build machine's platform; build
separately for each target OS and architecture.

## Release verification

Stable versions were checked on 2026-10-13; previews, RCs, milestones, and early
access builds were excluded:

| Component | Stable version | Official evidence |
| --- | --- | --- |
| Java | 27 | [OpenJDK GA builds](https://jdk.java.net/27/) |
| Gradle | 9.8.1 | [Release metadata](https://services.gradle.org/versions/current), [Java compatibility matrix](https://docs.gradle.org/current/userguide/compatibility.html) (Java 27 supported since 9.8.0) |
| JavaFX | 27 | [Maven Central artifact metadata](https://repo.maven.apache.org/maven2/org/openjfx/javafx-controls/maven-metadata.xml), [release notes](https://gluonhq.com/products/javafx/openjfx-27-release-notes/) |
| JavaFX plugin | 0.1.0 | [Plugin metadata](https://plugins.gradle.org/m2/org/openjfx/javafxplugin/org.openjfx.javafxplugin.gradle.plugin/maven-metadata.xml), [plugin documentation](https://github.com/openjfx/javafx-gradle-plugin) |
| Actions checkout | 7.0.1 | [Release](https://github.com/actions/checkout/releases/tag/v7.0.1) |
| Actions setup-java | 6.0.1 | [Release](https://github.com/actions/setup-java/releases/tag/v6.0.1) |
| Gradle Actions | 6.4.0 | [Release](https://github.com/gradle/actions/releases/tag/v6.4.0) |

Maven metadata's `latest`/`release` fields include JavaFX 28 early access; those
were deliberately excluded. `jar --describe-module` on the resolved JavaFX 27
Windows JARs confirmed `javafx.base`, `javafx.graphics`, and `javafx.controls`.
Gradle's built-in module-path inference replaces the old plugin's implicit
module helper. The wrapper distribution is SHA-256 pinned; its regenerated JAR
matches the [official wrapper checksum](https://services.gradle.org/distributions/gradle-9.8.1-wrapper.jar.sha256).

The original Gradle 8.6 build passed on Java 21 but reported APIs removed in
Gradle 9 (legacy Java conventions and JavaFX plugin internals). Updating the
plugin and replacing source/target compatibility with a toolchain resolved these
migration issues. There was no existing workflow or test suite.

Local validation: Windows/JDK 27 clean build with deprecations treated as errors.
The standard test task has no sources. Workflow syntax was checked with
actionlint 1.7.12 (ShellCheck unavailable). Hosted GitHub Actions execution
still requires a CI run.
