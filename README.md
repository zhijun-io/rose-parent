# Rose Parent POM

Optional parent POM that provides shared configuration for Maven Central publishing.

## Usage

Projects can optionally inherit from this parent to get pre-configured:

- `central-publishing-maven-plugin` with `autoPublish=true`
- `maven-gpg-plugin` with loopback pinentry for CI/CD
- `maven-source-plugin` and `maven-javadoc-plugin`
- `flatten-maven-plugin` for CI-friendly versioning
- `spring-javaformat-maven-plugin` for consistent code formatting

### Option 1: Inherit as Parent

```xml
<parent>
    <groupId>io.github.zhijun-io</groupId>
    <artifactId>rose-parent</artifactId>
    <version>0.0.1</version>
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

### Plugin Management

All plugin versions are centrally managed:

| Plugin | Version |
|--------|---------|
| maven-compiler-plugin | 3.11.0 |
| maven-surefire-plugin | 3.1.2 |
| maven-source-plugin | 3.3.0 |
| maven-javadoc-plugin | 3.6.0 |
| maven-gpg-plugin | 3.2.7 |
| flatten-maven-plugin | 1.5.0 |
| central-publishing-maven-plugin | 0.9.0 |
| spring-javaformat-maven-plugin | 0.0.43 |

### Java Version

Default Java version is 17. Override with:

```xml
<properties>
    <java.version>21</java.version>
</properties>
```

## Required Secrets

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
