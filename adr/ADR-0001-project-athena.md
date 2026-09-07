# Created on Fri, 7 Aug 2026

# ADR-0001: Establish Project Athena

## Status

Accepted

## Date

Fri, 7 Aug 2026

## Decision

Establish Project Athena as the engineering framework for designing, building, operating, and maintaining the Teragram Platform.

Project Athena will provide the methodology, documentation standards, infrastructure practices, and engineering principles that guide all future Teragram technology projects.

---

# Context

The Teragram ecosystem is growing from a collection of individual projects into a connected technology platform.

Current and future projects include:

- teragram.au — Personal technology portfolio and public presence.
- athena0x — Cybersecurity learning, research, and technical documentation.
- DramaQueen — Full-stack application development project.

As these projects evolve, a consistent approach is required for:

- Security.
- Infrastructure management.
- Automation.
- Documentation.
- Deployment.
- Recovery.
- Long-term maintenance.

Without a defined framework, systems risk becoming difficult to maintain, insecure, and dependent on individual memory.

---

# Problem

A repeatable engineering methodology is required to ensure that:

- New systems can be rebuilt.
- Decisions are documented.
- Security is considered from the beginning.
- Infrastructure changes are controlled.
- Operational knowledge is retained.
- Future projects follow consistent standards.

---

# Decision Drivers

The following principles guide this decision:

1. Secure by Design
2. Reproducible
3. Observable
4. Maintainable
5. Explainable

---

# Alternatives Considered

## Alternative 1: Continue building projects independently

### Benefits

- Faster initial development.
- Less upfront documentation.

### Disadvantages

- Knowledge becomes fragmented.
- Rebuilding systems becomes difficult.
- Security practices may become inconsistent.
- Technical debt accumulates.

### Decision

Rejected.

---

## Alternative 2: Use existing external platforms without creating a framework

Examples:

- Managed hosting platforms.
- Cloud-only solutions.
- Third-party deployment systems.

### Benefits

- Reduced infrastructure responsibility.
- Faster deployment.

### Disadvantages

- Less control.
- Reduced learning opportunity.
- Less understanding of underlying systems.

### Decision

Rejected as the primary approach.

External services may still be used where they provide value.

---

# Consequences

## Positive

- Creates a consistent engineering approach.
- Provides a foundation for Infrastructure as Code.
- Enables repeatable deployments.
- Improves security awareness.
- Creates professional-quality documentation.
- Builds transferable engineering skills.

## Negative

- Requires additional documentation effort.
- Initial progress may appear slower.
- Requires discipline to maintain standards.

---

# Implementation

Initial implementation includes:

- Creating GitHub repositories.
- Establishing documentation standards.
- Creating infrastructure documentation.
- Building repeatable server deployment processes.
- Applying security practices from the beginning.

---

# Verification

Success will be measured by:

- Documentation exists for major decisions.
- Systems can be rebuilt from documented processes.
- Infrastructure changes are version controlled.
- Security considerations are recorded.
- Operational procedures are repeatable.

---

# Review

This decision should be reviewed if:

- The platform architecture significantly changes.
- New infrastructure requirements emerge.
- Project scope expands beyond the current model.

---

# Author Notes

Project Athena begins as the foundation for a secure, reproducible, observable, maintainable, and explainable technology platform.

---

# Updated on Mon, 7 Sept 2026

# ADR-0001: Secure Layered Hosting Architecture

* **Status:** Accepted
* **Date:** 2026-09-07
* **Decision:** Use Cloudflare Tunnel → Apache HTTPS → Docker
* **Scope:** Teragram platform infrastructure

## Context

The Teragram platform is hosted on a Raspberry Pi and needs to provide public web access while minimising direct exposure of the origin server.

The platform also needs to support future application containers, databases, monitoring, backups, and additional services.

A simple direct Internet exposure model would increase the number of services and ports exposed by the origin and would make the Raspberry Pi itself a more obvious attack target.

The architecture therefore needs to provide:

* public HTTPS access
* origin protection
* minimal exposed services
* separation between the public web layer and application workloads
* a path toward containerisation
* reproducible configuration
* straightforward operational management

## Decision

The platform will use the following architecture:

```text
Internet
   │
   ▼
Cloudflare
   │
   ▼
Cloudflare Tunnel
   │
   ▼
Apache HTTPS
   │
   ▼
Private Docker network
   │
   ▼
Teragram application
```

Cloudflare Tunnel provides the connection between the public Cloudflare edge and the private origin.

Apache acts as the HTTPS origin/web layer.

Application workloads run inside Docker and should not be directly exposed to the Internet unless there is an explicit architectural requirement.

## HTTPS

HTTPS is used from Cloudflare to Apache.

A Cloudflare Origin CA certificate is used for the origin connection.

Cloudflare performs strict certificate verification.

This provides encrypted traffic between the Cloudflare edge and the origin while avoiding the requirement for the origin to be directly reachable from the public Internet.

## Consequences

### Positive

* Reduces direct Internet exposure of the Raspberry Pi.
* Avoids requiring public inbound access to the origin for normal web traffic.
* Provides a clear separation between edge, web server, and application layers.
* Supports future Dockerised services.
* Makes the intended security boundary easy to understand.
* Provides a scalable path toward additional applications and services.
* Keeps application ports private by default.

### Negative

* The platform depends on Cloudflare Tunnel for public access.
* Troubleshooting involves multiple layers: Cloudflare, tunnel, Apache, and application containers.
* Local tunnel configuration must be maintained securely.
* Origin certificate and tunnel credentials require secure backup and recovery procedures.

## Security Requirements

The implementation must follow these requirements:

1. No secrets are committed to the public repository.
2. Tunnel credentials remain on the server only.
3. Origin private keys remain outside source control.
4. Application containers do not publish ports unnecessarily.
5. Administrative services are not exposed publicly unless explicitly required.
6. Deployment-specific IP addresses and infrastructure identifiers are omitted from public documentation.
7. Configuration changes that materially affect security are documented.
8. Backups containing secrets are stored separately from the public repository.

## Alternatives Considered

### Direct port forwarding

**Rejected.**

Directly forwarding HTTP/HTTPS traffic to the Raspberry Pi would expose the origin network endpoint and increase the externally reachable attack surface.

### Direct application port exposure

**Rejected.**

Application services should remain behind the web/proxy layer unless there is a specific requirement to expose them.

### HTTP-only origin

**Rejected.**

The Cloudflare-to-origin connection should remain encrypted.

### Migrating the tunnel to remotely managed configuration

**Not selected.**

The current implementation intentionally retains local tunnel configuration to keep deployment configuration under server-side configuration management and Git-safe documentation.

## Future Review

This decision should be reviewed if:

* the hosting environment changes
* additional public services are introduced
* Kubernetes becomes part of production deployment
* the Cloudflare architecture changes
* application networking requirements change materially
* infrastructure-as-code replaces the current configuration approach
