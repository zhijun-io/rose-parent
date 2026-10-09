# Rose Parent POM

[![CI](https://github.com/zhijun-io/rose-parent/actions/workflows/ci.yml/badge.svg)](https://github.com/zhijun-io/rose-parent/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://adoptium.net/)

Optional parent POM that provides shared configuration for Maven Central publishing.

## Usage

Inheriting from this parent gives you two things.

Activated by the `release` profile (`./mvnw deploy -Prelease`):

- `central-publishing-maven-plugin` with `autoPublish=true`
- `maven-gpg-plugin` with loopback pinentry for CI/CD
- `maven-source-plugin` and `maven-javadoc-plugin`

Managed, so declared and configured by the child project without repeating a version:

- `maven-compiler-plugin`, `maven-surefire-plugin`, `maven-jar-plugin`, `maven-deploy-plugin`
- `flatten-maven-plugin` — only useful if the project actually uses `${revision}`-style CI-friendly versions
- `spring-javaformat-maven-plugin`

### Option 1: Inherit as Parent

```xml
<parent>
    <groupId>io.github.zhijun-io</groupId>
    <artifactId>rose-parent</artifactId>
    <version>0.0.3</version>           
</parent>
```

### Option 2: Copy Configuration

Projects that cannot use parent inheritance can copy the relevant plugin configurations from this POM into their own `pom.xml`.

## What's Included

### Release Profile

The `release` profile activates Maven Central publishing with GPG signing:

```bash
./mvnw deploy -Prelease
```

Snapshots go to the Sonatype snapshot repository declared in `distributionManagement`; that is how this
parent POM itself becomes resolvable for the projects that inherit from it.

### Plugin Management

Versions are pinned through properties named `<artifactId>.version` and currently resolve to:

| Plugin | Version |
|--------|---------|
| maven-compiler-plugin | 3.11.0 |
| maven-surefire-plugin | 3.1.2 |
| maven-jar-plugin | 3.3.0 |
| maven-deploy-plugin | 3.1.1 |
| maven-source-plugin | 3.3.0 |
| maven-javadoc-plugin | 3.6.0 |
| maven-gpg-plugin | 3.2.7 |
| flatten-maven-plugin | 1.6.0 |
| central-publishing-maven-plugin | 0.10.0 |
| spring-javaformat-maven-plugin | 0.0.43 |

### Java Version

Default Java version is 17. Override with:

```xml
<properties>
    <java.version>21</java.version>
</properties>
```

## CI and Required Secrets

`.github/workflows` calls the shared `zhijun-io/github-workflows` reusable workflows.
`maven-release.yml` grants `permissions: contents: write` because that workflow pushes the release tag and the
development-version bump back to `main`.

For Maven Central publishing, configure these secrets:

| Secret | Description |
|--------|-------------|
| `MAVEN_USERNAME` | Sonatype Portal username |
| `MAVEN_PASSWORD` | Sonatype Portal token |
| `GPG_SECRET_KEY` | ASCII-armored GPG private key |
| `GPG_PASSPHRASE` | GPG passphrase |

## Opt-in Philosophy

This parent POM is **optional**. Projects can:

1. **Full adoption**: Inherit from parent POM
2. **Partial adoption**: Copy specific plugin configurations
3. **Independent**: Maintain their own complete configuration

All Rose projects work with or without this parent.
