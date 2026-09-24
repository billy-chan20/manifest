# Release Notes
## Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250)
Release 1.1 (MR1 release)

September 2026

# 1.0 Introduction

This document provides release specific information about the Android* 16 Base BSP reference release supported on Intel® Core™ Processor (14th Gen) (code named Raptor Lake-S Refresh), Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250 (Code name: Twin Lake) running Android* 16 in a bare-metal OS environment.

>**Note:**
>The versions of the Android Common Kernel and AOSP open-source software components referenced in this release represent the Intel-validated baseline for the platform. Customers are encouraged to evaluate and integrate updates to these open-source components as they become available from the open-source community.

For instructions on building and loading Android* OS, refer to the Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250) Getting Started Guide (Published in [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/BM_BSP_2026_Q3_V2_A16/Getting_Started.md)).

# Terminology

| Term | Description |
|------|-------------|
| ADB | Android Debug Bridge |
| AOSP | Android Open Source Project |
| AVB | Android Verified Boot |
| AVF | Android Virtualization Framework |
| BSP | Board Support Package |
| CODEC | Coder-Decoder |
| CRB | Customer Reference Board |
| DP | DisplayPort |
| EC | Engineering Candidate |
| HDMI | High-Definition Multimedia Interface |
| IFWI | Integrated Firmware Image |
| ISV | Independent Software Vendor |
| NVMe | Non-Volatile Memory Express |
| RDC | Resource and Documentation Center |
| RVP | Reference Validation Platform |
| Raptor Lake-S R | Intel® Core™ Processor (14th Gen) |
| Twin Lake |Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250 |
| TEE | Trusted Execution Environment |

## Intended Audience

This document is intended for OSVs/ISVs interested in using Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250) to enable their customers.

## Customer Support

Contact your Intel representative for support or submit an issue to
[premiersupport.intel.com](http://premiersupport.intel.com/).


# 2.0 Best-Known Configuration

This section shows the compatible hardware and software configuration for this release.

## Hardware Configuration

### RVP: SR14 Raptor Lake-S RVP
#### Processors
      Intel® Core™ i3 processor 14100T
      Intel® Core™ i5 processor 14500T
      Intel® Core™ i7 processor 14700T

### CRB: Intel® Core™ 3 processor codenamed Twin Lake
#### Processors
      Intel® Core™ 3 Processor N355
      Intel® Processor N150
      Intel® Processor N250

# Release Information

This section contains general release information for BSP.

### Release 1.1 (MR1 release) - Current
### [Release 1.0](https://github.com/edge-aosp-bsp/manifest/tree/BM_BSP_2026_Q3_V1_A16)
### [Engineering Candidate 2 (EC2)](https://github.com/edge-aosp-bsp/manifest/tree/BM_BSP_2026_Q2_V1_A16)
### [Engineering Candidate 1 (EC1)](https://github.com/edge-aosp-bsp/manifest/tree/BM_BSP_2026_Q1_V1_A16)

---


## Release 1.1 (MR1 release)

## Software Configuration
1. Release 1.1 (MR1 release) Manifest File: [GitHub - BM_BSP_2026_Q3_V2_A16.xml](https://github.com/edge-aosp-bsp/manifest/blob/master/stable-build/A16/BM_BSP_2026_Q3_V2_A16.xml)
2. UEFI Reference BIOS:
   - Release Notes & Package for RPL [852225](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=852225)
   - Release Notes & Package for TWL [919389](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=919389)

## Release Version
| Type | Description |
|------|-------------|
| Release Version | Release 1.1 (MR1 release) |
| Build Target | caas-userdebug, caas-user |
| RPL Tested Hardware | SR14 Raptor Lake-S DDR5 UDIMM 1DPC RVP and Intel® Core™ Processor (14th Gen) |
| TWL Tested Hardware | Twin Lake Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250 |
| Android Version | android-16 |
| Kernel Version | 6.12.92 |

## Product Features
#### List of Product Features

| Feature Category | Feature | Availability |
|------------------|---------|--------------|
| Connectivity | Intel Wi-Fi* 6 and 7 | Yes |
| Connectivity | Bluetooth® 5.3 and 5.4 | Yes |
| ADB | ADB over Ethernet | Yes |
| Media | Video playback | Yes |
| Display | eDP | Yes |
| Display | DP over USB Type-C | Yes |
| Display | Dual Display | Yes |
| Audio | Audio - Onboard CODEC | Yes |
| Audio | Audio - USB 3.1 | Yes |
| Audio | Audio - USB-C | Yes |
| Audio | Audio - Bluetooth® | Yes |
| USB | USB-C | Yes |
| USB | USB 3.2 | Yes |
| USB | USB 2.0 | Yes |
| Ethernet | Ethernet | Yes |
| I/O | USB | Yes |
| I/O | Serial Port | Yes |
| Storage | NVMe | Yes |
| Touch | eDP Touch | Yes |
| Camera | USB | Yes |
| Boot | Fast Boot / Secure Boot / AVB | Yes |
| OTA | OTA Enabled | Yes |
| Location | Static location service via API | Yes |
| Security | Security-Trusty-TEE | Yes |
| Virtualization | AVF | [1] See the AVF disclaimer. |

[1]Disclaimer: AVF capability in the Intel Android 16 BSP is based on Google’s Android 16 AOSP implementation, has limited maturity for x86 platforms. For x86 devices, AVF operates using non-protected virtual machines, does not support protected virtual machines, lacks CTS/VTS coverage, and may not receive Google security updates for AVF-related x86 code paths. Accordingly, Intel makes no representation or warranty regarding security isolation, certification compliance, the availability of AVF-related security updates, or the continued availability of future AOSP or Google support for these features.

## Reference Documents

| Documentation on GitHub | Document No./Location |
|---------|------------------------|
|Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Core™ 3 Processor N355, Intel® Processor N150, and Intel® Processor N250) - Getting Started Guide |  [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/BM_BSP_2026_Q3_V2_A16/Getting_Started.md) |
| Android Manifest File | [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/master/stable-build/A16/BM_BSP_2026_Q3_V2_A16.xml) |

Log in to the Resource and Documentation Center
([rdc.intel.com](https://www.intel.com/content/www/us/en/resources-documentation/developer.html))
to search for and download the document numbers listed in the following
table. Contact your Intel field representative for access.

> **Note:**
> Third-party links are provided as a reference only. Intel does not control or audit third-party benchmark data or the websites referenced in this document. You should visit the referenced website and confirm whether the referenced data are accurate. 


| Documentation on Intel RDC | Document No./Location |
|---------|------------------------|
| 13th Gen Intel® Core™ Processor and Intel® Core™ Processor (14th Gen) (Code named Raptor Lake‑S/S Refresh) for Edge Platforms Reference UEFI BIOS/IFWI IPU 2026.3 (ver 7117.51) |  [852225](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=852225) |
| Intel® Processor N150&N250, Intel® Core™ 3 Processor N355 for Edge Applications (IPU2026.3) |  [919389](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=919389) |

# Disclaimer

You may not use or facilitate the use of this document in connection
with any infringement or other legal analysis concerning Intel products
described herein. You agree to grant Intel a non-exclusive, royalty-free
license to any patent claim thereafter drafted which includes subject
matter disclosed herein.

No license (express or implied, by estoppel or otherwise) to any
intellectual property rights is granted by this document.

All information provided here is subject to change without notice.
Contact your Intel representative to obtain the latest Intel product
specifications and roadmaps.

The products described may contain design defects or errors known as
errata which may cause the product to deviate from published
specifications. Current characterized errata are available on request.

Copies of documents which have an order number and are referenced in
this document may be obtained by calling 1-800-548-4725 or visiting the
[Intel Resource and Documentation
Center](https://www.intel.com/content/www/us/en/resources-documentation/developer.html).

Intel technologies\' features and benefits depend on system
configuration and may require enabled hardware, software or service
activation. Performance varies depending on system configuration. No
product or component can be absolutely secure. Check with your system
manufacturer or retailer or learn more at
[intel.com](http://intel.com/).

The Bluetooth® word mark and logos are registered trademarks owned by
Bluetooth SIG, Inc. and any use of such marks by Intel Corporation is
under license.

© Intel Corporation. Intel, the Intel logo, and other Intel marks are
trademarks of Intel Corporation or its subsidiaries. Other names and
brands may be claimed as the property of others.

