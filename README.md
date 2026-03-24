# Windows Tablet Hardware Telephony

This repository defines a practical path to turn a Windows tablet into a custom cellular telephony device using an external LTE modem and a user-space telephony stack.

The project target is not "make Windows act like a built-in phone." The target is narrower and more realistic:

- Use a Windows tablet such as a Surface as the host
- Attach a cellular modem that supports LTE data and voice on T-Mobile U.S.
- Place and receive cellular calls from a desktop application
- Support SMS and LTE data as part of the same stack
- Keep the hardware footprint small enough to mount cleanly behind or within a tablet enclosure

## Core Conclusion

The hardware is viable. The modem layer can handle calls, SMS, and data.

The missing piece is software. Windows 11 will expose the modem as serial ports, a network interface, and sometimes an audio device, but it will not provide a native desktop dialer or a complete voice-call control flow for this setup. That gap is what this project is meant to fill.

## Recommended Starting Point

Use a SIM7600-class LTE modem, not a 5G module.

Preferred variants:

- `SIM7600A-H` for best T-Mobile U.S. alignment
- `SIM7600G-H` as a workable fallback when the `A-H` variant is difficult to source

Preferred form factors:

- Mini PCIe modem plus Mini PCIe-to-USB adapter with SIM slot
- M.2 modem plus M.2-to-USB adapter only if the exact carrier-compatible variant is easier to obtain

Avoid 5G-first modules for this project. They are commonly optimized for data and carrier-specific provisioning rather than straightforward voice support.

## Project Documents

- `docs/project-brief.md`: scope, goals, and non-goals
- `docs/architecture.md`: system architecture and component boundaries
- `docs/hardware.md`: recommended hardware, BOM, and physical integration notes
- `docs/repo-layout.md`: proposed repository and solution structure
- `docs/software-stack.md`: services, state model, and Windows integration strategy
- `docs/roadmap.md`: phased execution plan with risk ordering

## First Milestone

The first useful milestone is not a polished app. It is a narrow proof of control:

1. Detect the modem reliably on Windows.
2. Send AT commands.
3. Detect `RING` and caller ID.
4. Place and hang up a call.
5. Verify stable audio routing.

If those five items work, the rest becomes normal product engineering instead of speculation.