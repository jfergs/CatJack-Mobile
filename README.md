# CatJack Mobile

CatJack Mobile is the native iOS and iPadOS operator client for the CatJack
ecosystem. It is the field-first interface for radio control, APRS, mapping,
logging, POTA/SOTA workflows, and future digital modes.

CatJack Mobile is not a replacement for CatJack Server. It is a client and,
where iOS/iPadOS allows direct device access, a mobile radio-control runtime that
uses the same CatJack safety model.

## Ecosystem Role

```text
CatJack Mobile
  |
  +--> Direct USB radio operation where iOS/iPadOS exposes the device path
  |
  +--> CatJack Server API over local/remote network
  |
  +--> CatJacker bridge/appliance for USB radios, field Wi-Fi, BLE, or embedded server
```

Relationship definitions:

- **CatJack** is the core radio platform and server.
- **CatJack Mobile** is the native iPhone/iPad operator client.
- **CatJacker** is the bridge/appliance that can expose USB radios and optional
  embedded CatJack services to mobile devices.

## Architecture

CatJack Mobile should be organized around capability-driven services:

- Radio session manager.
- CatJack Server client.
- Direct USB transport where supported.
- CatJacker transport integration.
- APRS views and receive/transmit safety gates.
- Logging and activation workflows.
- Offline cache for field use.
- Future audio/digital-mode pipeline.

See:

- [Project Vision](docs/PROJECT_VISION.md)
- [Architecture](docs/ARCHITECTURE.md)

## Direct USB Operation

The direct USB path is for iPhone/iPad operation without a laptop or appliance
when the OS and adapter expose the required radio interfaces.

Direct USB should:

- Detect attached radio transports without assuming write capability.
- Query radio identity before write-capable controls.
- Use the CatJack radio abstraction model for capabilities.
- Keep memory writes, transmit, tuning, and APRS transmit behind explicit safety
  gates.
- Fall back to CatJack Server or CatJacker when iOS does not expose the required
  serial/audio path.

## CatJacker Integration

CatJacker gives CatJack Mobile a stable appliance/bridge path:

- iPhone/iPad connects to CatJacker over Wi-Fi, LAN, Bluetooth, or USB.
- CatJacker bridges to the radio's USB CAT/audio/TNC interfaces.
- CatJacker may eventually host CatJack Server locally for field operation.
- Mobile remains the operator UX, while CatJacker owns transport reliability.

## APRS

APRS support should follow CatJack's receive-only-first model:

- Packet log.
- Station/object table.
- Map overlays.
- Messages and bulletins.
- APRS-IS receive through CatJack Server or CatJacker-hosted service.
- KISS/TNC receive through CatJacker or direct USB where available.
- Transmit only after explicit callsign, path, channel, PTT, queue, retry, and
  audit-log design.

## Logging And POTA/SOTA

CatJack Mobile should own the field UX for:

- Quick QSOs.
- POTA/SOTA activation/session context.
- Park/summit references.
- Offline-first logs.
- Export/sync to common logging formats later.
- Radio state capture from CatJack when available.

## Future Digital Modes

Digital modes are future work and depend on proven audio, timing, and transport:

- Audio input/output routing.
- Time sync awareness.
- JS8/FT8 style workflow exploration.
- Strict separation between receive/decode and transmit.
- Shared safety model with CatJack Server.

## Capability Matrix

| Capability | Mobile role | CatJack dependency | CatJacker dependency |
| --- | --- | --- | --- |
| Radio status | Primary UX | Server/RAL source | Optional transport |
| Direct USB CAT | Native runtime | Protocol/safety model | None |
| Remote CAT | API client | Required | Optional bridge/server host |
| APRS map/table | Primary mobile UX | Service/data source | Optional KISS/APRS transport |
| Logging/POTA/SOTA | Primary owner | Storage/API later | None |
| Audio waterfall | Future mobile UX | DSP model/reference | Audio bridge optional |
| Digital modes | Future UX/runtime | Safety/API model | Audio/timing bridge optional |

## Safety

Mobile controls should be capability-driven. Do not show write-capable or
transmit-capable controls simply because the UI can render them. The active
radio/session must prove support and the operator must explicitly enable
dangerous paths.
