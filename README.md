# Created on Fri, 7 Aug 2026

# Project Athena

Engineering the Teragram Platform using secure-by-design principles, Infrastructure as Code, automation, documentation, and observability.

## Purpose

Project Athena is a reproducible and maintainable infrastructure platform supporting Teragram projects and services.

## Engineering Principles

- Secure by Design
- Reproducible
- Observable
- Maintainable
- Explainable

## Current Status

**Milestone M0 — Foundation**

Establishing the foundations for a secure, documented, and reproducible platform.

# Updated on Mon, 7 Sept 2026

# Teragram Platform

Secure-by-design infrastructure and application platform for Teragram.

This repository documents the architecture, engineering decisions, deployment principles, and development workflow used to build the Teragram platform.

> **Security note:** This is a public repository. Deployment-specific secrets, credentials, private keys, internal network information, tunnel identifiers, and other sensitive infrastructure details are intentionally excluded.

## Architecture

The platform uses a layered, minimal-exposure architecture:

```text
Internet
   │
   ▼
Cloudflare
   │
   │ HTTPS
   ▼
Cloudflare Tunnel
   │
   │ encrypted outbound tunnel
   ▼
Apache HTTPS
   │
   │ private local/container networking
   ▼
Docker
   │
   ▼
Teragram application
```

The origin server is not intended to be directly exposed to the Internet.

Public traffic is delivered through Cloudflare and reaches the server through an outbound Cloudflare Tunnel.

## Current Infrastructure

### Server

* Raspberry Pi 5
* Debian GNU/Linux
* ARM64
* NVMe storage
* Apache HTTP Server
* Docker Engine
* Cloudflare Tunnel (`cloudflared`)

### Public access

The public website is served through:

* `teragram.au`
* `www.teragram.au`

Both hostnames use the same Cloudflare Tunnel.

The tunnel is locally managed using a server-side configuration file.

Sensitive deployment values are represented by placeholders in documentation.

## HTTPS

HTTPS is used between Cloudflare and the origin server.

The origin uses a Cloudflare Origin CA certificate.

Apache terminates HTTPS locally on port `443`.

Example deployment-specific configuration:

```text
SSLCertificateFile <ORIGIN_CERTIFICATE_PATH>
SSLCertificateKeyFile <ORIGIN_PRIVATE_KEY_PATH>
```

Private keys and certificate material are never stored in this repository.

Cloudflare SSL/TLS mode is configured for strict origin verification.

## Docker

Application workloads are intended to run inside Docker rather than being directly exposed by the host.

The server uses the following organisational structure:

```text
/opt/containers/
├── projects/
├── data/
└── backups/
```

### Design principles

* Application services remain private unless explicitly required.
* Host ports are published only when necessary.
* Services communicate through Docker networking where appropriate.
* Persistent application data is separated from project source code.
* Backups are kept outside application containers.
* Secrets are supplied through deployment configuration rather than source control.

## Security Principles

### Secure by design

Security is considered during architecture and implementation rather than added after deployment.

### Least privilege

Services, users, containers, and network access should receive only the permissions required for their function.

### Minimal attack surface

Avoid unnecessary Internet-facing services, published ports, packages, services, and administrative interfaces.

### Reproducible

The platform should be rebuildable from documented configuration, automation, and source code rather than depending on undocumented manual changes.

### Observable

Important services and infrastructure should provide useful logs, health information, and eventually monitoring and alerting.

### Maintainable

Updates, backups, configuration changes, and operational procedures should be documented and repeatable.

### Explainable

Important architectural and security decisions should be documented so that the reasoning behind the implementation remains understandable.

## Repository Structure

```text
teragram-platform/
├── README.md
├── CHANGELOG.md
├── LICENSE
├── .gitignore
├── principles.md
└── docs/
    └── ADR-001.md
```

As the platform grows, application, infrastructure, deployment, and testing directories will be added without mixing secrets or host-specific state into the repository.

## Development Workflow

Changes should follow a simple workflow:

```text
Plan
  ↓
Implement
  ↓
Test
  ↓
Review
  ↓
Commit
  ↓
Deploy
  ↓
Verify
  ↓
Document
```

Infrastructure changes should be documented when they materially change the architecture, security posture, deployment process, or operational behaviour.

## Backup and Recovery

The server is designed to be rebuildable rather than dependent on an irreplaceable installation.

Backups should include required application data and configuration while excluding unnecessary temporary files.

Secrets and private credentials should be restored separately from the public repository.

A successful backup is not considered validated until the recovery process has been tested.

## Current Status

### Milestone M0 — Infrastructure Foundation

Completed:

* Fresh Raspberry Pi OS installation on NVMe
* Backup strategy validated
* Engineering principles established
* GitHub repository established
* Apache installed
* Docker installed
* Cloudflare Tunnel established
* HTTPS origin configured
* Public DNS routed through Cloudflare Tunnel
* Origin exposure minimised
* Container directory structure established

### Next

The next major milestone is the containerised Teragram application platform.

Planned areas include:

* Dockerised application
* Private application networking
* Application data persistence
* Database integration
* Monitoring and health checks
* Deployment automation
* Infrastructure-as-code
* Application security controls

## Public Repository Security

The repository intentionally does **not** contain:

* passwords
* API tokens
* private keys
* Cloudflare credentials
* tunnel credentials
* Origin CA private keys
* internal IP addresses
* LAN network details
* machine identifiers
* SSH private keys
* production backup archives
* other deployment-specific secrets

Use placeholders when documenting configuration that would otherwise reveal sensitive deployment information.

## Project Goal

Teragram is being developed as a practical demonstration of secure, maintainable infrastructure and application engineering.

The goal is not simply to make the website work, but to build a platform that can be understood, secured, backed up, rebuilt, monitored, and extended over time.
