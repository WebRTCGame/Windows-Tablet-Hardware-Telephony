# Software Stack

## V1 Software Shape

Build this as two Windows components:

- A local telephony service
- A tablet-oriented desktop client

That split keeps modem ownership, state, and reconnect handling away from the UI.

## Proposed Responsibilities

### Telephony Service

- Detect the modem and open the AT port
- Expose modem health and registration state
- Manage the call state machine
- Handle incoming-call events and caller ID
- Send SMS and collect inbound SMS
- Coordinate audio session setup
- Persist logs and diagnostics

### Tablet Client

- Dial pad
- Incoming call popup
- In-call controls
- SMS views
- Signal and registration indicators
- Audio device selection and diagnostics page

## Suggested Internal Modules

### `ModemSession`

- Open and close ports
- Send AT commands
- Read unsolicited events
- Reconnect when the device disappears

### `CallManager`

- `Dial(number)`
- `Answer()`
- `Reject()`
- `HangUp()`
- Event translation into UI-friendly states

### `SmsManager`

- Read message storage
- Send outbound messages
- Raise inbound message events

### `AudioManager`

- Enumerate audio devices
- Detect modem audio exposure if present
- Bind audio route for call sessions
- Recover after device churn

### `DeviceMonitor`

- Watch USB and COM-port changes
- Update service state without crashing active sessions

## Recommended Language Direction

For a Windows-first product, C# is the most practical primary language.

Reasons:

- Good Windows API access
- Strong async and event-driven patterns
- Reasonable UI options for tablet-focused desktop apps
- Easier packaging and maintainability than a prototype-only scripting stack

Python remains useful for modem experimentation and command discovery, but it should not be the default implementation language for the finished product.

## Expected Command Categories

The modem layer will need to handle at least:

- Device identity and readiness
- SIM status
- Network registration
- Signal strength
- Outgoing dial and hangup
- Incoming-call events
- Caller ID
- SMS send and receive
- Audio-related configuration where supported by the modem

## Service API Shape

The UI should not talk AT commands directly. It should consume a stable local contract such as:

- `GetStatus()`
- `GetSignal()`
- `Dial(number)`
- `AnswerCall()`
- `RejectCall()`
- `EndCall()`
- `ListMessages()`
- `SendMessage(number, text)`
- `GetAudioDevices()`
- `SetAudioRoute(inputId, outputId)`

## Logging Requirements

Capture logs for:

- Raw unsolicited modem events
- Command latency and failures
- Call-state transitions
- Audio device changes
- USB disconnects and reconnects

Without this, debugging carrier and device edge cases will take far longer than necessary.