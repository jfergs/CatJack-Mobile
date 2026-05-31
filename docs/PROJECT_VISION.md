# CatJack Mobile Project Vision

CatJack Mobile is the field operator experience for the CatJack ecosystem. It
should make iPhone and iPad useful as serious radio operation surfaces, not just
remote displays.

## Product Goal

Give an operator a portable interface for:

- Radio status and control.
- APRS situational awareness.
- Field logging.
- POTA/SOTA activation workflows.
- Offline operation.
- Future digital-mode operation once audio/timing paths are proven.

## Relationship To CatJack

CatJack Mobile should reuse CatJack's platform concepts:

- Radio abstraction layer.
- Capability-driven controls.
- Receive/read-only first for uncertain paths.
- Explicit preflight for write/transmit.
- Local data ownership and export.

CatJack Server remains the preferred source of radio policy, APRS services, and
shared persistence when available. Mobile may run direct USB workflows, but it
must honor the same safety rules.

## Relationship To CatJacker

CatJacker is the field bridge/appliance path for mobile devices. It should solve
transport problems that phones and tablets cannot always solve directly:

- USB serial exposure.
- Audio device bridging.
- TNC/KISS access.
- Local Wi-Fi/Bluetooth operation.
- Optional embedded CatJack Server.

Mobile should treat CatJacker as a trusted transport/appliance, not as the owner
of radio safety policy.

## Field-First Principles

- Offline-first views for logs, references, and recent APRS state.
- Large, readable controls for field use.
- Fast access to station, park, summit, and radio state.
- Clear receive/transmit separation.
- No hidden background transmit behavior.
- Simple export and recovery paths.

## Roadmap Themes

1. Radio status and safe controls.
2. CatJack Server connectivity.
3. Direct USB capability probe.
4. APRS receive views.
5. Logging and POTA/SOTA sessions.
6. CatJacker integration.
7. Offline maps/cache.
8. Audio foundation.
9. Digital-mode receive experiments.
10. Controlled transmit workflows only after preflight and audit logging.
