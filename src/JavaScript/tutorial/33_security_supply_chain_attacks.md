---

[<< Chapter 32](./32_javaScript_security.md) | [Chapter 34 >>](./34_web_cryptography.md)

---

# Chapter 33: Security — Supply Chain Attacks in JavaScript

## 1. What Are Supply Chain Attacks?

A `supply chain attack` targets the dependencies and tools that developers use — rather than the application itself.
In the JavaScript ecosystem, this often means compromising packages, build pipelines, or third-party libraries.

### Definition

A supply chain attack occurs when a trusted component in your development or deployment process is tampered with, allowing an attacker to inject malicious code or behavior into your application.

### Common Attack Vectors

#### Malicious Packages:

- Attackers publish harmful packages to npm (e.g., event-stream incident).

#### Typosquatting:

- Creating packages with similar names to popular libraries (reactt, lodas).

#### Dependency Hijacking:

- Taking over abandoned or unmaintained packages.

#### Compromised Maintainer Accounts:

- Attackers steal npm credentials or tokens to inject malicious updates.

#### Build Pipeline Compromise:

- Injecting scripts into CI/CD or release automation tools.

### Example Scenario

You install a dependency:

```bash
npm install lodash
```

But accidentally type:

```bash
npm install lodas
```

The fake lodas package could exfiltrate environment variables or access tokens.

## 2. npm / Yarn Security

Both npm and Yarn provide mechanisms to help mitigate supply chain risks.

### a. npm Security Features

- npm audit — Scans dependencies for known vulnerabilities.
- npm audit fix — Automatically updates to patched versions.
- npm ci — Installs from package-lock.json to ensure deterministic builds.
- Integrity Checks — Uses SHA512 checksums to verify package contents.
- Two-Factor Authentication (2FA) — Protects maintainer accounts.

#### Example:

```bash
npm audit
# Run security audit
npm audit fix
# Automatically update vulnerable dependencies
```

### b. Yarn Security

- yarn audit — Similar to npm audit.
- Offline cache integrity — Prevents tampering with cached packages.
- Lockfiles (yarn.lock) — Ensures consistent dependency trees across environments.

#### Example:

```bash
yarn audit --level high
```

## 3. Dependency Auditing Tools

### a. npm / Yarn Built-in Audit

- Detects known vulnerabilities from the npm advisory database.
- Outputs severity levels (low, moderate, high, critical).

### b. Third-Party Tools

| Tool                   | Description                                            | Highlights                              |
| :--------------------- | :----------------------------------------------------- | :-------------------------------------- |
| Snyk                   | Scans open source dependencies and container images    | Monitors repos continuously             |
| Dependabot (GitHub)    | Auto-updates dependencies with security patches        | Integrates directly with GitHub         |
| OWASP Dependency-Check | Cross-language tool for CVE scanning                   | Supports JavaScript, Python, Java, etc. |
| Socket.dev             | Analyzes package behavior (network, file access, etc.) | Detects suspicious patterns             |
| npm Graph Analyzer     | Maps full dependency tree                              | Identifies outdated or risky packages   |

#### Example: Using Snyk

```bash
npm install -g snyk
snyk test
# or
snyk monitor
```

## 4. Best Practices for Supply Chain Security

### a. Dependency Hygiene

- Keep dependencies updated using Dependabot or Renovate.
- Remove unused or outdated packages.
- Avoid installing packages with low downloads or unknown maintainers.

### b. Use Lockfiles

- Commit package-lock.json or yarn.lock to version control.
- Ensures consistent installations across environments.

### c. Restrict Package Installation

- Use npm scopes or private registries for internal packages.
- Configure `.npmrc` to only allow trusted registries:

```ini
registry=https://registry.npmjs.org/
always-auth=true
```

### d. Verify Maintainers and Integrity

- Check package maintainers before installation:

```bash
npm info <package-name>
```

- Enable npm 2FA for publishing.
- Prefer packages with verified maintainers and signed commits.

### e. Secure Build Pipelines

- Store secrets securely using environment variables or vaults.
- Use CI/CD secrets management (GitHub Actions, GitLab, Jenkins).
- Don’t store credentials in .env or code repositories.

### f. Monitor and Respond

- Subscribe to vulnerability alerts (GitHub, npm advisories).
- Automate rebuilds after patch releases.
- Review changelogs before updating major versions.

## 5. Example Workflow: Securing a Node.js Project

### Step 1: Run audits

```bash
npm audit --json > audit-report.json
```

### Step 2: Use Snyk for deeper insights

```bash
snyk test
snyk monitor
```

### Step 3: Enable GitHub Dependabot

Add `.github/dependabot.yml`:

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Step 4: Harden your npm configuration

```bash
npm config set ignore-scripts true
npm config set strict-ssl true
```

### Step 5: Enable 2FA

```bash
npm profile enable-2fa auth-and-writes
```

## 6. Case Studies

### a. Event-Stream Incident (2018)

- A popular npm package (event-stream) was hijacked by a malicious actor who added code to steal cryptocurrency wallet data from dependent apps.

### b. Colors.js and Faker.js (2022)

- Maintainer sabotaged packages intentionally, breaking production apps worldwide.
- Reinforced the need for dependency vetting and mirroring trusted registries.

## 7. Advanced Defenses

- Package Signing:
  npm is introducing package signing with digital signatures (using sigstore).

### Runtime Security Monitoring:

Detect malicious network or file I/O at runtime.

### Immutable Infrastructure:

Use container images with known, verified dependencies.

### Provenance Tracking (SLSA Framework):

Establish supply chain levels for secure artifacts.

## 8. Summary

| Topic                | Key Takeaway                                                  |
| :------------------- | :------------------------------------------------------------ |
| Supply Chain Attacks | Target your dependencies, not just your code                  |
| npm / Yarn Security  | Use audit tools and lockfiles                                 |
| Auditing Tools       | Automate vulnerability scanning (Snyk, Dependabot)            |
| Best Practices       | Least privilege, lock versions, enable 2FA                    |
| Build Security       | Protect CI/CD, secrets, and dependency sources                |
| Future Trends        | Package signing, provenance tracking, AI-based risk detection |

## Conclusion

Supply chain attacks are among the most significant threats in modern JavaScript development.
By maintaining strict dependency hygiene, leveraging audit tools, and securing your build pipelines, you can drastically reduce your project’s exposure.

**Golden Rule:** Trust is earned, not assumed — even in your package.json.

---

[<< Chapter 32](./32_javaScript_security.md) | [Chapter 34 >>](./34_web_cryptography.md)

---
