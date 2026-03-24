# Repository Layout

## Intended Structure

As implementation starts, the repository should grow into a small product workspace rather than a single application folder.

```text
.
|-- docs/
|-- hardware/
|   |-- bom/
|   |-- photos/
|   `-- measurements/
|-- src/
|   |-- Telephony.Service/
|   |-- Telephony.UI/
|   |-- Telephony.Core/
|   `-- Telephony.Modem/
|-- tools/
|   |-- BenchConsole/
|   `-- LogCapture/
`-- tests/
    |-- Telephony.Core.Tests/
    `-- Telephony.Modem.Tests/
```

## Folder Roles

### `docs/`

- Product requirements
- Architecture notes
- Hardware decisions
- Bring-up procedures

### `hardware/`

- Exact part numbers
- Purchase notes
- Mounting sketches
- Photos of bench and tablet integration
- Signal and antenna placement notes

### `src/Telephony.Service/`

- Background modem host
- Device discovery
- Call and SMS orchestration
- Local API for the UI

### `src/Telephony.UI/`

- Dial pad
- Call notifications
- In-call controls
- SMS experience
- Diagnostics views

### `src/Telephony.Core/`

- Shared domain models
- Call state machine
- Common contracts and events
- Logging abstractions

### `src/Telephony.Modem/`

- Serial transport
- AT command handling
- Response parsing
- Modem-specific capabilities

### `tools/BenchConsole/`

- Fast bring-up tool for hardware validation
- Manual AT command testing
- Raw event monitoring

This tool should exist before the full UI. It is the quickest way to prove modem behavior and isolate field issues.

### `tools/LogCapture/`

- Gather service logs
- Export diagnostics packages
- Support long-run validation

### `tests/`

- Parser tests for modem responses
- State-machine tests for call flow
- Regression tests for disconnect and reconnect behavior

## Build Order

The recommended implementation order is:

1. `Telephony.Modem`
2. `Telephony.Core`
3. `BenchConsole`
4. `Telephony.Service`
5. `Telephony.UI`

That order tracks risk correctly. It prevents UI work from masking unresolved modem behavior.