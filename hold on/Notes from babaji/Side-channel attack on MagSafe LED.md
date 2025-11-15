Breachforce talk Name - NaughtyMag: Making Macbook Blink Its Data Away
### Overview

- A side-channel attack that turns Apple’s MagSafe LED indicator into a data exfiltration device..

- What are Side Channel Attacks? Software can control LED status using SMC to indicate color change if battery over 80%

- The LED, usually meant to indicate charging status (amber/green), can be modulated to transmit data covertly
### Key Concepts

MagSafe Connection Points:

- Uses its own protocol
- Pinout:
	- Ground
	- Power
	- Adapter Sense
	
- 1-wire protocol: computer ↔ cable ↔ charger (powerbrick)

- All communicate with each other to negotiate power

- Also lets them control which connectors are manufacture supported

- Integrated Circuit DS24123: Can take command over 1-wire from Macbook and change LED status
> -The IC involved is not widely documented
### How Control Works

Charger Startup:

- Charger provides very low current initially
- Why negotiation works first - then fails
- Initial creators - which created the ability to change MagSafe charger color to another color 

https://apphousekitchen.com/aldente-overview/features/#control-magsafe-led

https://github.com/AppHouseKitchen/AlDente-Charge-Limiter

- Control of the MagSafe LED is software-driven, but routed through
	- The SMC (System Management Controller).
	- Can be manipulated using the SMC API, which documents key values for LED control.

Attackers can:

1. Use software tools or custom scripts (several emerged from GitHub issues).
2. Leverage I/O Kit on macOS to interface with the hardware.
### Exfiltration Method

LED controls can be toggled with precision  ​Requires:
	- Precise control of on/off timing
	- Understanding of data encoding methods

Encoding Challenges:

- Simple binary (e.g., 0000 or 1111) can lead to ambiguity in timing- based detection.
- Manchester Encoding may be needed to avoid repetition ambiguity

Morse code is a viable fallback for slower but clearer data transmission.
- Don't need rising/falling edge
- only need steady state
- New encoding to not be dependent on time
### Limitations

- Color masking is not feasible (LED has limited colors).
- Can be detected via High-Security Monitoring (HSM).
- Could be made stealthier by tuning antenna properties of the wire (convert power cable into low-range antenna).

### Counter Measures

- Channels require software side component
- Monitor end user devices
- Be aware of such potential attacks

### Related Concepts

Read Morris Mano - Digital Electronics

Why Manchester encoding can't work for discrete waves. Digital Electronics & Computer Architecture (background needed)

macOS IOKit, SMC APIs

### References 

#babajiwisdom 