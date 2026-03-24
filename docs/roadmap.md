# Roadmap

## Phase 0: Hardware Selection

Goal:

- Freeze one exact modem, one adapter, one antenna set, and one T-Mobile SIM plan for development

Deliverables:

- Final part list
- Variant verification notes
- Bench-test hookup plan

Exit criteria:

- Windows detects the modem consistently
- AT command channel is reachable
- Data attachment is visible

## Phase 1: Modem Control Proof

Goal:

- Prove reliable command and event handling on Windows

Deliverables:

- COM port detection
- AT command wrapper
- Registration and signal query
- Raw event logger

Exit criteria:

- Commands succeed repeatedly after reconnects
- Unsolicited events are captured without blocking the command loop

## Phase 2: Voice Call Core

Goal:

- Place and receive calls from software

Deliverables:

- Dial flow
- Ring detection
- Answer and reject flow
- Hang-up handling
- Call-state machine

Exit criteria:

- Outgoing and incoming calls both work on the target modem and SIM
- Failure modes such as `NO CARRIER` and busy state are surfaced correctly

## Phase 3: Audio Path Validation

Goal:

- Establish a stable audio design

Deliverables:

- Verified audio route on the chosen hardware
- Fallback strategy if the modem audio function is unreliable
- In-call device selection controls

Exit criteria:

- Speech is intelligible in both directions
- Device reconnect does not leave the app in a broken audio state

## Phase 4: Messaging And UI

Goal:

- Turn the modem controller into a usable tablet product

Deliverables:

- Dialer UI
- Incoming call popup
- SMS inbox and compose flow
- Signal and registration indicators

Exit criteria:

- A user can make and receive calls without using a terminal window

## Phase 5: Hardening

Goal:

- Survive real-world carrier and device behavior

Deliverables:

- Recovery from unplug and replug
- Retry policies
- Better error reporting
- Persisted logs and support bundle

Exit criteria:

- Long-run stability on the target tablet
- Repeatable startup and reconnect behavior

## Risk Order

Do not build the final UI first.

The real sequence is:

1. Variant and carrier validation
2. AT transport reliability
3. Incoming and outgoing call control
4. Audio routing
5. UX polish

## Expected Engineering Weight

From the extracted Q&A, a realistic first-pass effort is on the order of tens of hours, not a quick wiring project. The complexity is concentrated in integration and recovery logic, especially around audio and carrier behavior.

## Immediate Next Steps

1. Lock the modem SKU and adapter SKU.
2. Record exact Windows device enumeration for the bench hardware.
3. Build the Phase 1 modem-control prototype.
4. Prove incoming and outgoing calls before investing in the final tablet enclosure.