# Created on Fri, 7 Aug 2026

# Project Athena Changelog

All significant changes to the Project Athena platform are documented in this file.

This changelog records releases, improvements, and important decisions.

---

# [Unreleased]

Changes planned for the next release:

- Establish server foundation documentation.
- Define production server architecture.
- Begin Infrastructure as Code structure.
- Document security hardening approach.

---

# [v0.1.0] — Foundation Release

Release Date: Fri, 7 Aug 2026

## Added

v0.1.0 Foundation Release

Included:
✅ Repository created
✅ Git workflow established
✅ Engineering principles defined
✅ ADR process created
✅ Milestone tracking created
✅ Changelog process created

## Infrastructure

- Rebuilt Raspberry Pi server environment.
- Confirmed Raspberry Pi 5 booting from 512GB NVMe storage.
- Validated backup strategy before rebuild.

## Security

- Established policy that secrets, credentials, and private keys must never be stored in source control.
- Adopted public-by-design, private-until-ready approach.

## Documentation

- Created Project Athena Engineering Principles.
- Created milestone tracking process.
- Established decision documentation through ADRs.

---

# Version History Convention

Project Athena follows Semantic Versioning (SemVer):

Example: v1.0.0

MAJOR.MINOR.PATCH

Meaning:

- MAJOR: Significant architectural changes.
- MINOR: New features or capabilities.
- PATCH: Bug fixes and minor improvements.

Version numbers communicate the maturity and impact of changes.

## MAJOR Version

Example: v2.0.0

Used for significant architectural changes, breaking changes, or major platform redesigns.

Examples:

- Changing the infrastructure architecture.
- Replacing core technology components.
- Major changes that require migration.

---

## MINOR Version

Example: v0.2.0

Used when introducing new features or capabilities without breaking existing functionality.

Examples:

- Adding monitoring.
- Adding automated deployments.
- Introducing new infrastructure components.

---

## PATCH Version

Example: v0.1.1

Used for smaller improvements, fixes, or documentation updates.

Examples:

- Fixing scripts.
- Correcting documentation errors.
- Minor configuration improvements.

---

# Pre-Production Versioning

While Project Athena is under active development, versions will remain below:

v1.0.0

The `0.x.x` range indicates that the platform is still evolving and architecture may change.

The first production-ready release will be:

v1.0.0

when:

- Infrastructure can be rebuilt reproducibly.
- Security controls are established.
- Monitoring and observability are implemented.
- Backup and recovery procedures are validated.
- Operational documentation is complete.

---

# Release Process

Each release should:

1. Complete planned implementation work.
2. Update documentation.
3. Update CHANGELOG.md.
4. Review security implications.
5. Validate functionality.
6. Commit changes.
7. Create a version tag.
8. Publish release notes.

The version tag represents the final approved state of that release.

---

# Updated on Mon, 7 Sept 2026

# Changelog

All notable infrastructure and platform changes are documented here.

The project is currently in the infrastructure foundation stage.

## [Unreleased]

### Planned

* Containerised Teragram application
* Application/database architecture
* Private Docker networking
* Health checks and monitoring
* Deployment automation
* Infrastructure as Code
* Security testing
* Production deployment workflow

## 2026-09-07

### Infrastructure

* Completed Raspberry Pi server foundation.
* Confirmed Debian ARM64 server environment.
* Established Docker as the application runtime.
* Created `/opt/containers/` organisational structure.
* Established Cloudflare Tunnel as the public access path.
* Routed both primary public website hostnames through the same tunnel.
* Configured Apache as the HTTPS origin.
* Installed and configured Cloudflare Origin CA certificate.
* Verified local HTTPS origin connectivity.
* Removed the previous direct DNS path that bypassed the Cloudflare Tunnel.
* Updated server DNS configuration for reliable public DNS resolution.

### Security

* Adopted Cloudflare Tunnel instead of direct inbound origin exposure.
* Adopted HTTPS between Cloudflare and Apache.
* Kept application workloads behind the intended container/network boundary.
* Ensured tunnel credentials and private certificate material remain outside source control.
* Established placeholder-based documentation for deployment-specific values.

### Documentation

* Established public-safe infrastructure documentation.
* Documented the layered hosting architecture.
* Added ADR-001 covering the secure hosting architecture.
* Updated the project README with architecture, principles, security requirements, backup strategy, and current status.
