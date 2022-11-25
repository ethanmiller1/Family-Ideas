Laptops should have the following capabilities:
- [ ] Connect to 4 external monitors through a single docking station (requires Thunderbolt port).
- [ ] Charging and docking occurs through single Thunderbolt port.
- [ ] Lightweight, easy to hold with one hand ([Surface Pro](https://www.microsoft.com/en-us/d/surface-laptop-4/946627FB12T1/1RBQ) is really good here).
- [ ] Bluetooth 5.0.
- [ ] 2 USB-A ports.
- [ ] 2 slots for memory (to upgrade to 32GB).


# HP EliteBook x360 1040 G6

## [Specs](https://support.hp.com/au-en/document/c06400841#AbT4)

### Ports/slots

| Specification                                                             | Features              |
|---------------------------------------------------------------------------|-----------------------|
| (2) USB 3.1 Type-C with Thunderbolt support (supports Power Delivery 3.0) | 40 Gbps, `45 - 100 W` |
| (2) USB 3.1 Gen 1 (1 charging)                                            | 10 Gbps               |
| (1) HDMI 1.4                                                              | 10.2Gbps              |
| (1) AC power                                                              |                       |
| (1) Headphone/microphone combo jack                                       |                       |

### Graphics

| Specification | Description            |
|---------------|------------------------|
| Integrated    | Intel UHD Graphics 620 |
| Supports      | HD decode              |
|               | Direct X12 (DX12)      |
|               | HDMI 1.4b              |

According to [Intel specification](https://ark.intel.com/content/www/us/en/ark/products/124967/intel-core-i5-8250u-processor-6m-cache-up-to-3-40-ghz.html "ark.intel.com"), you can connect up to <mark class="hltr-yellow">three monitors</mark> maximal with up to 4K resolutions (limited to 24 fps if using HDMI). Please note however that the number of connected display possible also depends on the physical slot available from the manufacturer.

![[Pasted image 20221123194317.png]]

# HP EliteBook 840 G3

## [Specs](https://support.hp.com/us-en/document/c05259054)

### Graphics

| Processor Graphics                           | [Intel® HD Graphics 520](https://ark.intel.com/content/www/us/en/ark/products/88190/intel-core-i56300u-processor-3m-cache-up-to-3-00-ghz.html) |
|----------------------------------------------|------------------------|
| Max Resolution (HDMI)                        | 4096x2304@24Hz         |
| Max Resolution (DP)                          | 4096x2304@60Hz         |
| Max Resolution (eDP - Integrated Flat Panel) | 4096x2304@60Hz         |
| # of Displays Supported                      | 3                      |

### Ports/slots

| Specification                                     | Features        |
|---------------------------------------------------|-----------------|
| (1) USB 3.1 Gen 1 charging                        |                 |
| (1) USB 3.1 Gen 1                                 | 10 Gbps         |
| (1) USB **Basic** Type-C                              | Charging output |
| (1) DisplayPort                                   | 25.92 Gbps      |
| (1) VGA                                           | 691.2 Mbps      |
| (1) RJ-45/Ethernet                                |                 |
| (1) Docking connector                             |                 |
| (1) Headphone/microphone combo                    |                 |
| (1) AC                                            |                 |
| (1) External SIM                                  |                 |
| (1) SD media card reader, supports SD, SDHC, SDXC |                 |

The Basic USB-C port does [not support Alt mode](https://h30434.www3.hp.com/t5/Notebook-Hardware-and-Upgrade-Questions/usb-type-c-in-hp-elitebook-745-g3-support-hp-convert-usb-c/td-p/7319384) for video out, [nor Power Delivery](https://h30434.www3.hp.com/t5/Notebook-Hardware-and-Upgrade-Questions/can-someone-charge-a-hp-EliteBook-840-g3-with-a-type-C/td-p/8280903) for charging your machine. It cannot be used with a Type-C docking station.

#### What is alt mode?

Alt Mode is <mark class="hltr-yellow">a functional extension of USB-C which enables the USB connection to carry non-USB signals</mark>. Alt Mode(s) are optional capabilities that are unique to the USB-C connector or port that allow technologies, like DisplayPort and Thunderbolt 3, to be transmitted. If a port says that it is a **Basic** USB, that means it only supports USB signals.

* [What is USB-C DisplayPort (DP Alt Mode)?](https://www.benq.com/en-us/knowledge-center/knowledge/usb-c-introduction-what-is-dp-alt-mode.html)
* [Tech Talk: Using USB-C and DisplayPort over Alt Mode](https://www.startech.com/en-us/blog/tech-talk-using-usb-c-and-displayport-over-alt)