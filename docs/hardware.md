# Hardware

## Recommended Baseline

Target a SIM7600-class LTE modem with T-Mobile-compatible bands and voice support.

Preferred order:

1. `SIM7600A-H` for North America and T-Mobile voice alignment
2. `SIM7600G-H` as fallback when sourcing is easier

## Form Factor Recommendation

For a tablet build, the cleanest option is:

- Mini PCIe modem
- Mini PCIe-to-USB adapter with SIM slot
- Two low-profile LTE antennas using U.FL or IPEX connectors

Reasons:

- Easier to source the correct North America modem variant
- Better compatibility with existing WWAN-style adapter boards
- Cleaner Windows USB integration than improvised hobby boards
- Easier SIM access when the adapter includes the slot

M.2 remains viable, but only if the exact modem variant and adapter chain are confirmed before purchase.

## Surface vs 2-in-1 Laptop for mPCIe SIM7600A-H Integration

When the selected modem is `SIM7600A-H` (LTE Cat 4, North America, mPCIe), the main constraints are physical volume, internal metal interference, and reliable USB/serial behavior.

### Good: Business 2-in-1 laptop (e.g. HP Elite x360 / Dell Latitude 5330 2-in-1)

- Usually ship with factory WWAN slots and approved antenna feedthrough, meaning the internal RF board and routing are already engineered for cellular modules.
- Thicker chassis than tablet, making it easier to fit a mini PCIe-to-USB adapter and keep 2x low-profile antennas in plausible locations.
- More ports (USB-A/C, HDMI, expansion) for benching and debug tools without relying on dongles.
- Better internal cooling and power headroom; less risk of thermal throttling under sustained data/voice sessions.
- Often have WWAN module carrier compatibility documented (e.g. Sierra/Intel/U-blox in OEM parts list), a valuable precedent for carrier/VoLTE success.

### Caution: Surface-like tablet

- Very thin profile (9-10mm). Installing an external module stack often requires a custom adhesive rear pack or housing extension; risk of poor mechanical integration.
- Limited USB-C ports, no USB-A; debug/probing requires hubs.
- No factory WWAN slot unless specifically the OEM LTE model; even with that, the module may be soldered or proprietary.
- Internal antennas are tuned for OEM WWAN; retrofit with an external mPCIe module can be tricky to maintain signal quality.

### Recommendation for this project

- Primary hardware integration target should be a 2-in-1 laptop with WWAN/WWAN slot or known internal modem compatibility.
- Prove `SIM7600A-H` via an external mini PCIe + U.FL antenna setup first on a bench laptop.
- If the Surface must be used, treat it as a second-stage retrofit with a custom mechanical system (back adhesive tray or small stacking adapter) and depend on USB-C for data, effective heat management, and antenna placement.

## SIM7600A-H mPCIe-specific notes

- Ensure you get the `SIM7600A-H` variant with quoted bands for T-Mobile (B2/4/12/66/71 where applicable).
- Use adapter boards that preserve the mPCIe USB enumeration for the modem, expose a microSIM/nanoSIM slot, and supply 5V with 2A+ capability to avoid undervoltage during transmission.
- Validate the adapter’s pin mapping (the few cheap adapters sometimes map the antenna pins incorrectly or expose only networking without AT control). The best is a board advertised as "LTE WWAN modem adapter" with explicit PASS-THROUGH for mPCIe antenna ports.
- Antenna strategy: MAIN and AUX LTE in the optional case; if possible include GNSS for the SIM7600A-H module to maximize carrier registration reliability.

## Update to Hardware Validation Checklist

- Modem variant exactly matches intended bands and VoLTE requirements, preferably SIM7600A-H on the verified board.
- Adapter exposes required USB functions and SIM routing and keeps the mPCIe signal integrity.
- Voice support is documented or field-verified for the chosen modem + carrier.
- COM ports enumerate cleanly on Windows 11 and persist after sleep/plug/unplug.
- Data interface appears reliably after boot and reconnect.
- Audio interface behavior is confirmed on the exact hardware combination (USB audio or modem-poly configured path).

## Avoid

- SIM800/SIM900-class legacy GSM boards
- 5G modules marketed mainly for data gateways
- Raspberry Pi HAT-style modem stacks for the final tablet build
- Generic low-cost boards with unclear carrier band support

## Practical BOM

### Required

- SIM7600A-H or SIM7600G-H mini PCIe modem
- Mini PCIe-to-USB adapter with SIM slot and stable power design
- Two LTE antennas
- USB cable suitable for both data and power
- T-Mobile SIM with VoLTE-capable plan

### Optional

- GPS antenna if location matters
- Powered USB hub for early bench testing
- Thin enclosure or adhesive-backed mounting layer
- Short U.FL extension pigtails if antenna placement needs more flexibility

## Cost Envelope

Based on the extracted Q&A, realistic hardware cost is roughly:

- Modem and adapter: `$60-$110`
- Antennas: `$10-$30`
- Cables and power accessories: `$10-$30`
- SIM and initial service: variable

Working estimate for a first build:

- Hardware subtotal: about `$120-$180`

The engineering effort will dominate cost much more than the BOM.

## Physical Integration Notes

- Mount antennas away from large metal shielding where possible.
- Treat U.FL connections as semi-permanent, not high-cycle connectors.
- Ensure antennas are connected before powering the modem.
- Keep the full modem stack thin enough to sit behind a tablet or inside a rear shell without protruding awkwardly.

## Hardware Validation Checklist

- Modem variant exactly matches intended bands
- Adapter exposes required USB functions and SIM routing
- Voice support is documented or field-verified for the chosen modem family
- COM ports enumerate cleanly on Windows 11
- Data interface appears reliably after boot and reconnect
- Audio interface behavior is confirmed on the exact hardware combination