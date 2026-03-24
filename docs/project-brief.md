# Project Brief

## Objective

Build a Windows-tablet telephony stack that provides:

- Outgoing cellular calls
- Incoming cellular calls
- SMS send and receive
- LTE data connectivity
- A desktop UI suitable for a tablet form factor

The intended end state is a Windows tablet that behaves like a phone from the user's point of view, while internally running a custom telephony stack on top of a USB-attached modem.

## Product Framing

This is not a native Windows telephony feature.

This project is better understood as:

- A modem integration project
- A call-control project
- An audio-routing project
- A small desktop communications product

## What The PDF Established

- SIM800/SIM900-style legacy boards are not the right path for T-Mobile U.S. voice use.
- SIM7600-class LTE modules are the practical baseline.
- Incoming calls are possible if VoLTE is active and the application monitors modem events.
- Windows will not provide a built-in dialer or native call UX for an external modem in this configuration.
- The main engineering difficulty is not issuing AT commands. It is the cross-layer integration between modem control, call state, audio, and user experience.

## Success Criteria

The project is successful when a test tablet can:

- Register on T-Mobile LTE
- Hold an active data session
- Receive an incoming call and notify the UI
- Answer and end calls from the application
- Place an outgoing call from the application
- Route call audio through a predictable Windows audio path
- Recover cleanly after disconnects, poor signal, or modem resets

## Non-Goals For V1

- Native carrier provisioning workflows
- eSIM support
- 5G voice support
- Tight OS-level integration with Windows shell calling features
- Full PBX features such as IVR, call queuing, or SIP interoperability
- Generalized support for every modem family

## Constraints

- Must work on Windows 11
- Must target T-Mobile U.S. first
- Must prioritize compact hardware integration suitable for a tablet
- Must survive real-world carrier and USB edge cases

## Working Assumptions

- The modem presents one or more COM ports for AT control.
- The modem exposes LTE data as a network interface such as NDIS or MBIM.
- Voice support depends on the exact modem variant, firmware, and carrier provisioning.
- Audio may be exposed as a USB audio function on some hardware, but must be treated as a project risk until verified on the chosen modem and adapter combination.