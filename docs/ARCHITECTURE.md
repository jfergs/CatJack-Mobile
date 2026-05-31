# CatJack Mobile Architecture

CatJack Mobile is a native iOS/iPadOS application. Its architecture should keep
radio transports, server APIs, APRS, logging, and future digital modes separate
so each capability can mature without forcing unsafe coupling.

## High-Level Architecture

```text
SwiftUI operator views
  |
  +--> Radio session state
  +--> APRS views
  +--> Logging/POTA/SOTA views
  +--> Settings and capability panels
          |
          v
Service layer
  |
  +--> CatJack Server API client
  +--> Direct USB transport adapter
  +--> CatJacker transport adapter
  +--> Offline cache
  +--> Future audio/digital pipeline
```

## Core Modules

| Module | Responsibility |
| --- | --- |
| Radio Session | Active radio, capabilities, status, selected transport. |
| Server Client | CatJack Server REST/WebSocket API calls. |
| USB Transport | Direct device access where iOS/iPadOS permits. |
| CatJacker Client | Bridge/appliance discovery and transport. |
| APRS | Packets, stations, map overlays, messages, bulletins. |
| Logging | QSOs, activation context, exports, offline queue. |
| POTA/SOTA | Park/summit references, activation workflow, spots later. |
| Safety | Preflight state, transmit/write gating, audit prompts. |
| Cache | Offline state for logs, APRS, maps, and recent sessions. |

## Transport Strategy

Mobile should support three operation modes:

1. **CatJack Server mode:** connect to a CatJack Server running on desktop,
   remote host, or embedded appliance.
2. **CatJacker mode:** connect to a CatJacker bridge/appliance that exposes
   radio transports or hosts CatJack Server.
3. **Direct USB mode:** use iOS/iPadOS device access when the radio path is
   available directly.

The UI should present capabilities from the active session rather than assuming
all modes support the same controls.

## APRS Architecture

APRS should be receive-first:

- Consume packets/stations/messages/bulletins from CatJack Server where
  available.
- Use CatJacker/direct USB KISS paths later when exposed.
- Cache recent packets and station state for field continuity.
- Keep beacon/message/digipeater/iGate transmit absent until safety and queueing
  are designed.

## Logging And Activation Architecture

Logging/POTA/SOTA are primary mobile workflows:

- Store local draft logs offline.
- Attach radio state when available.
- Track activation metadata separately from raw QSO rows.
- Export/sync later through explicit operator action.

## Future Digital Modes

Digital modes should be isolated behind an audio/timing service boundary:

- Receive/decode experiments first.
- Explicit transmit gating.
- Shared safety model with CatJack.
- CatJacker audio bridge support where mobile direct audio is not viable.

## Ecosystem Integration

CatJack Mobile depends on ecosystem contracts documented in the CatJack repo:

- `docs/ECOSYSTEM_OVERVIEW.md`
- `docs/RADIO_ABSTRACTION_LAYER.md`
- `docs/CATJACK_SERVER.md`
- `docs/desktop-command-contract.md`
- `docs/aprs-roadmap.md`
