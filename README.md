# Sly Technologies SDK Parent

Parent POM for all [Sly Technologies](https://www.slytechs.com/) SDK modules. Provides shared build configuration, plugin management, and deployment targets inherited by every module in the SDK.

## Overview

`sdk-parent` is not a library — it contains no code. It exists so that build configuration is defined once and inherited consistently across all SDK modules. Every SDK module declares this as its `<parent>`.

## What It Provides

- **Java 22** compiler configuration
- **Plugin management** — pre-configured versions for `maven-compiler-plugin`, `maven-surefire-plugin`, `maven-source-plugin`, `maven-javadoc-plugin`, `maven-gpg-plugin`, `maven-deploy-plugin`
- **Source and Javadoc JARs** attached automatically on every build
- **Deployment targets** — Maven Central for OSS releases (see [Deployment](https://claude.ai/chat/2e850609-7937-4b3b-96e2-83681dc59415#deployment))
- **JUnit platform alignment** — pins JUnit platform versions to prevent surefire conflicts across all modules

## Requirements

| Tool  | Version |
| ----- | ------- |
| Java  | 22+     |
| Maven | 3.9+    |

## Usage

Declare `sdk-parent` as the parent in any SDK module:

```xml
<parent>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>sdk-parent</artifactId>
    <version>3.0.0-SNAPSHOT</version>
    <relativePath>../sdk-parent/pom.xml</relativePath>
</parent>
```

Once declared, your module inherits:

- Compiler and plugin configuration — no versions needed in `<build>`
- License, organization, and developer metadata
- Deployment target configuration

## Deployment

Artifacts are deployed to [Maven Central](https://central.sonatype.com/) via the Sonatype Central Portal.

**Required entries in `~/.m2/settings.xml`:**

```xml
<servers>
    <server>
        <id>maven-snapshots</id>
        <username>YOUR_PORTAL_TOKEN_USERNAME</username>
        <password>YOUR_PORTAL_TOKEN_PASSWORD</password>
    </server>
    <server>
        <id>maven-releases</id>
        <username>YOUR_PORTAL_TOKEN_USERNAME</username>
        <password>YOUR_PORTAL_TOKEN_PASSWORD</password>
    </server>
</servers>
```

Generate a portal token at [central.sonatype.com](https://central.sonatype.com/) → Account → Generate User Token.

Snapshot publishing must be enabled for the `com.slytechs.sdk` namespace at [central.sonatype.com/publishing/namespaces](https://central.sonatype.com/publishing/namespaces).

## Consuming Snapshots

To use snapshot builds in your project, add the Central Portal snapshot repository:

```xml
<repositories>
    <repository>
        <id>central-portal-snapshots</id>
        <url>https://central.sonatype.com/repository/maven-snapshots/</url>
        <releases><enabled>false</enabled></releases>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
</repositories>
```

## Related Modules

| Module                                                   | Description                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| [sdk-bom](https://github.com/slytechs-repos/sdk-bom)     | Bill of Materials — centralizes dependency version management |
| [sdk-build](https://github.com/slytechs-repos/sdk-build) | Reactor aggregator — builds all OSS modules in dependency order |

## License

Licensed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).