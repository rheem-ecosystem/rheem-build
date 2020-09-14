# Rheem Build Infrastructure

This repository contains common infrastructure to be used by Rheem modules that build with Maven. It consists of a *rheem-build-resources* project that bundles up resources that are needed during the build. The second project is *rheem-build-parent* that can be used as parent project to pre-configure core dependencies, properties, reference documentation generation and most important of all the appropriate distribution assembly.

The parent project can be eased for either a single-module Maven project or a multi-module one. Each of the setups requires a slightly different setup of the project.

## Project setup

### General setup

The following dependencies are pre-configured.

* Logging dependencies: SLF4j + Commons Logging bridge and Logback as test dependency
* Test dependencies: JUnit / AssertJ / Hamcrest / Mockito
* Dependency versions for commonly used dependencies

### Single project setup

If the client project is a project consisting of a single project only all that needs to be done is declaring the parent project:


```xml
<parent>
	<groupId>io.rheem</groupId>
	<artifactId>rheem-parent</artifactId>
	<version>${most-recent-release-version}</version>
</parent>
```
Be sure to adapt the version number to the latest release version.

### Multi project setup

A multi module setup requires slightly more setup and some structure being set up.

* The root `pom.xml` needs to configure the `project.type` property to `multi`.
* The assembly needs to be build in a dedicated sub-module (e.g. `distribution`), declare the assembly plugin (see single project setup) in that submodule and reconfigure the `project.root` property in that module to `${basedir}/..`.
* Configure `${dist.id}` in the root project to the basic artifact id (e.g. `rheem-example`) as this will serve as file name for distribution artifacts, static resources etc. It will default to the artifact id and thus usually resolve to a `…-parent` if not configured properly.

### Build configuration

* Goals to execute `clean (dependency:tree) install -Pci` to run the build
* Goals to execute `clean deploy -Pci,artifactory` to deploy artifacts to Artifactory

### Additional build profiles

* `ci` - Packages the JavaDoc as JAR for distribution (needs to be active on the CI server to make sure we distribute JavaDoc as JAR).
