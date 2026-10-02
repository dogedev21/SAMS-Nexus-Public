# SAMS Nexus

> A purpose-built Discord operations and personnel management system designed for SAMS.

SAMS Nexus is a custom Discord bot and internal management platform built to centralize day-to-day operational workflows for the San Andreas Medical Services roleplay environment.

The system handles personnel requests, field training workflows, promotions, leave management, events, analytics, administrative actions, and other internal processes through a single modular Discord application.

This repository exists primarily as a **public showcase and technical reference for the project**.

## Project Status

**Active / Production**

SAMS Nexus is actively developed and used in a privately managed production environment.

The production bot, database, Discord configuration, credentials, infrastructure, and deployment environment are **privately hosted and are not publicly accessible**.

This repository should therefore be treated as a representation of the project and its architecture rather than a publicly hosted service.

---

## Overview

SAMS Nexus was built to replace a collection of manual Discord workflows with a centralized system capable of handling recurring administrative and personnel tasks.

Instead of relying on staff members to manually track requests, update messages, manage roles, maintain calendars, and coordinate internal processes, Nexus provides dedicated systems that automate much of that work.

The project is written primarily in **JavaScript / Node.js** and uses **discord.js** for Discord integration.

Persistent systems use a **MariaDB** backend.

---

# Core Systems

## Field Training Requests

The FTO request system manages training requests between personnel and available Field Training Officers.

It includes functionality for:

- Creating FTO requests
- Accepting training requests
- Tracking active training sessions
- Request cancellation
- Staff notifications
- Direct-message notifications
- Training threads
- Automatic request expiration and cancellation
- Request state tracking

The goal is to provide a consistent workflow from the initial request through completion.

---

## Personnel Promotions

Nexus includes a dedicated promotion management system for processing personnel rank changes.

The system handles:

- Promotion workflows
- Discord role updates
- Rank-role assignment
- Command-role assignment
- Promotion announcements
- Configurable promotion templates
- High Command and Low Command role mapping

Rank structures and command roles can be configured without rewriting the underlying promotion logic.

---

## LOA / ROA Management

One of the larger components of Nexus is its persistent Leave of Absence and Reduced Operational Activity system.

The system supports:

- LOA requests
- ROA requests
- Staff review workflows
- Approval and denial
- Revocation
- Extensions
- Request modification
- Status tracking
- Automatic role management
- Calendar synchronization
- Discord DM status cards
- Request history
- Review logging
- Timezone-aware dates
- Daylight Saving Time handling
- Request overlap validation
- Configurable duration limits

LOA and ROA information is persisted in MariaDB so requests survive application restarts.

Personnel receive continuously updated status information through Discord rather than relying solely on static staff messages.

---

## Event Management

Nexus contains an internal event management system designed for scheduled department activities.

Features include:

- Event creation
- Event storage
- Event calendars
- RSVP management
- Event reminders
- Scheduled cleanup
- Database persistence

This allows department events to be managed directly through Discord without maintaining a separate scheduling system.

---

## Administrative Tools

Nexus includes administrative interfaces and actions for authorized staff.

Administrative functionality is separated from normal personnel workflows to keep privileged actions organized and easier to audit.

Administrative actions can also be recorded through the project's logging infrastructure.

---

## Analytics

The analytics system provides operational counters and system statistics.

It is responsible for functionality such as:

- Activity counters
- Nexus request statistics
- Active session statistics
- Analytics displays
- Persistent analytics data
- Bot presence information

This gives staff a quick view of system activity directly from Discord.

---

## Logging

Nexus includes centralized logging utilities used throughout the project.

The logging system records relevant administrative and system activity while also providing dedicated error-reporting functionality.

This helps make production issues easier to diagnose without mixing application logic with logging code.

---

# Architecture

SAMS Nexus originally existed primarily as a larger monolithic Discord application.

The project has since been refactored into a modular architecture where major systems are separated into their own components.

```text
SAMS-Nexus/
│
├── index.js
├── config.js
├── state.js
│
├── systems/
│   ├── admin.js
│   ├── analytics.js
│   ├── events.js
│   ├── ftoRequests.js
│   ├── loa.js
│   └── promotions.js
│
├── shared/
│   ├── db.js
│   ├── interaction.js
│   ├── logger.js
│   └── time.js
│
└── Bot/
    └── legacy application files
```

### `index.js`

Primary application entry point.

Responsible for Discord client startup and routing incoming interactions to the appropriate Nexus system.

### `systems/`

Contains the major operational modules used by Nexus.

Each system manages its own workflow while sharing common infrastructure.

### `shared/`

Contains utilities shared between multiple systems, including:

- Database connectivity
- Interaction utilities
- Logging
- Time and Discord timestamp helpers

### `config.js`

Centralizes runtime configuration and environment-variable mapping.

### `state.js`

Maintains shared runtime state used across application modules.

---

# Technology

SAMS Nexus currently uses:

| Technology | Purpose |
|---|---|
| Node.js | Application runtime |
| JavaScript | Primary development language |
| discord.js 14 | Discord API integration |
| MariaDB | Persistent application storage |
| mysql2 | Node.js database connectivity |
| Luxon | Date and timezone handling |
| dotenv | Environment configuration |

---

# Design Philosophy

Nexus is designed around a few basic principles.

### Discord First

Personnel should be able to complete common department workflows without leaving Discord.

### Automation Where Appropriate

Repetitive administrative tasks such as role changes, request updates, reminders, and status tracking should be handled automatically whenever possible.

### Persistent State

Important personnel information should survive application restarts and deployments.

### Modular Systems

Large workflows are separated into independent modules rather than being maintained inside a single application file.

### Auditability

Administrative actions and workflow changes should be logged wherever practical.

### Production Reliability

The project is designed around its actual production environment rather than serving as a generic public Discord bot framework.

---

# Hosting

SAMS Nexus is **privately hosted**.

The production infrastructure is not exposed through this repository.

Private production components include:

- Discord bot credentials
- Database credentials
- Production environment variables
- Discord guild configuration
- Internal role IDs
- Internal channel IDs
- Database contents
- Server infrastructure
- Operational logs
- Deployment configuration
- Internal administrative information

The public repository does not provide access to the production Nexus instance.

---

# Public Repository Purpose

This repository is maintained publicly primarily for:

- Project documentation
- Portfolio/reference purposes
- Architecture visibility
- Development history
- Code examples
- Demonstrating the systems built for SAMS Nexus

It is **not intended to function as a public SaaS product or publicly operated Discord bot**.

There is no public Nexus instance that can be invited to external Discord servers.

---

# Self-Hosting

SAMS Nexus was built specifically around its production environment.

Although portions of the code may be technically capable of running elsewhere, the project should not be considered a plug-and-play public Discord bot.

A separate deployment would require configuration for items such as:

- Discord application credentials
- Guild-specific channels
- Guild-specific roles
- Rank structures
- Command structures
- MariaDB
- Database initialization
- Environment configuration
- Discord permissions
- Operational workflows

The official SAMS Nexus environment remains privately hosted.

---

# Security

Sensitive production information is intentionally kept outside the repository.

Secrets such as Discord bot tokens, database passwords, and production credentials are provided through environment configuration and are never intended to be committed to source control.

Anyone referencing this code should follow the same practice.

Credentials should **never** be hardcoded into the application or committed to Git.

---

# Database

Persistent Nexus systems use MariaDB.

The database is used for workflows where information must survive bot restarts or application deployments, including systems such as:

- Leave requests
- Reduced activity requests
- Events
- Request lifecycle data
- Persistent operational information

Database access is provided through a shared connection pool so individual Nexus systems can use a common database layer.

---

# Timezone Support

Nexus contains timezone-aware scheduling functionality.

This is particularly important for LOA, ROA, and scheduling systems where personnel may operate across different timezones.

The system supports common United States timezone abbreviations as well as canonical timezone regions and uses timezone-aware calculations to properly account for Daylight Saving Time.

Discord timestamps are also used where appropriate so dates and times render automatically in the viewer's local timezone.

---

# Legacy Code

The repository retains portions of the original Nexus implementation under the `Bot/` directory.

These files represent the earlier monolithic architecture used before Nexus was divided into dedicated systems.

The current application uses the modular root entry point and `systems/` architecture.

The legacy files remain primarily for compatibility and historical reference.

---

# Development

SAMS Nexus continues to evolve alongside the workflows it supports.

Development generally focuses on:

- Reducing repetitive staff work
- Improving existing workflows
- Increasing reliability
- Improving user interaction design
- Expanding personnel-management features
- Improving logging and auditability
- Refactoring older systems
- Improving persistence and recovery behavior

Features are primarily developed based on actual operational needs rather than a public product roadmap.

---

# Availability

SAMS Nexus is not currently offered as a publicly hosted service.

The production instance is reserved for its intended organization and Discord environment.

This repository being publicly visible does **not** imply that the hosted Nexus service, database, bot account, or supporting infrastructure is publicly available.

---

# Contributions

Because Nexus is built around a specific private production environment, outside contributions are not currently the primary development model for the project.

Issues, forks, and code references may still be useful depending on the purpose of this repository, but production development remains internally managed.

---

# Disclaimer

SAMS Nexus is an independently developed software project designed for use within its intended roleplay/community environment.

Names, organizations, workflows, and systems represented within the project may be fictional or specific to that environment.

This repository does not provide access to any private production system, Discord server, database, or infrastructure.

---

# Author

Developed and maintained by **dogedev21**.

GitHub: [@dogedev21](https://github.com/dogedev21)

---

<p align="center">
  <strong>SAMS Nexus</strong><br>
  Internal operations. One system.
</p>
