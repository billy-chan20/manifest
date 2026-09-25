# Getting Started Guide
## Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355)
Release 1.1

September 2026

# Introduction

This document provides instructions for building and loading Android\* 16 on Intel® Core™ Processor (14th Gen) (code named Raptor Lake-S Refresh), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355 (Code named Twin Lake) for Edge Platforms.

>**Note:**
>The versions of the Android Common Kernel and AOSP open-source software components referenced in this release represent the Intel-validated baseline for the platform. Customers are encouraged to evaluate and integrate updates to these open-source components as they become available from the open-source community.

You are recommended to review the release information before proceeding
with this Getting Started Guide. For release information, notes, and
references, refer to the following documents:

* Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355) Release Notes (Published in [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/master/README.md))

# Terminology

| Term                  | Description                                                            |
| --------------------- | ---------------------------------------------------------------------- |
| adb                   | Android Debug Bridge                                                   |
| AOSP                  | Android Open-Source Project                                            |
| AVF                   | Android Virtualization Framework                                       |
| BIOS                  | Basic Input/Output System                                              |
| BM                    | Bare Metal refers to an Android system that runs without a hypervisor. |
| BSP                   | Board Support Package                                                  |
| CRB                   | Customer Reference Board                                               |
| EC                    | Engineering Candidate                                                  |
| Git                   | Git — Version control system                                           |
| HDA                   | High-Definition Audio                                                  |
| IFWI                  | Intel Firmware Interface                                               |
| ISO                   | ISO image — Disk image format                                          |
| ISV                   | Independent Software Vendor                                            |
| LTS                   | Long-Term Support                                                      |
| NIC                   | Network Interface Card                                                 |
| NVMe                  | Non-Volatile Memory Express                                            |
| OS                    | Operating System                                                       |
| OSV                   | Operating System Vendor                                                |
| PCH‑IO                | Platform Controller Hub — I/O Configuration                            |
| Raptor Lake-S Refresh | Intel® Core™ Processor (14th Gen)                                      |
| RDC                   | Resource and Documentation Center                                      |
| RVP                   | Reference Validation Platform                                          |
| SATA                  | Serial ATA (Serial Advanced Technology Attachment)                     |
| SELinux               | Security-Enhanced Linux                                                |
| TCC                   | Intel® Time Coordinated Computing                                      |
| TEE                   | Trusted Execution Environment                                          |
| Trusty                | Secure operating system that provides a TEE for Android                |
| Twin Lake             | Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355            |
| UEFI                  | Unified Extensible Firmware Interface                                  |
| USB                   | Universal Serial Bus                                                   |
| VMX                   | Virtual Machine Extensions                                             |
| VT-d                  | Virtualization Technology for Directed I/O                             |

## Intended Audience

This document is intended for OSVs/ISVs interested in using Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355) to enable their customers.

## Customer Support

Contact your Intel representative for support or submit issues to
[premiersupport.intel.com](http://premiersupport.intel.com/).

# Overview and Prerequisites

Android\* BSP is a reference implementation used for testing hardware feature enablement. This document provides step-by-step instructions for building the Android Bare Metal image and installing it on the Intel® Core™ Processor (14th Gen), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355 platforms.

## Requirements

### **Build Host Machine**
* A **64-bit development workstation** running the Ubuntu\* 22.04 (Jammy Jellyfish) operating system.
* **Python version 3.6 or later**. This requirement aligns with the latest repo command released by Google.
* At least **400GB of free disk space** on the workstation is required to check out the source code and store build artifacts.

### **Supported Intel® Platforms for Edge Platforms**
* The target system must contain the latest supported silicon from one of the following platforms:
  - Intel® Core™ Processor (14th Gen) (code named Raptor Lake-S Refresh)
  - Intel® Core™ 3 Processor N355 (code named Twin Lake)
  - Intel® Processor N150 (code named Twin Lake)
  - Intel® Processor N250 (code named Twin Lake)
* A minimum of **400 GB of storage**. 
* Flashed with the latest IFWI corresponding to the target platform:
  - **Raptor Lake-S Refresh**: Use Reference UEFI BIOS/IFWI version 7117_51. See [Document Number: 852225](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=852225)
  - **Twin Lake (N355, N250, N150)**: Use Reference UEFI BIOS/IFWI version 7113_51. See [Document Number: 919389](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=919389) 
* **High-speed network** connectivity.

### Notes:

1. Although Android* is typically built using a GNU/Linux* or macOS* operating system, Intel recommends building the Android images on Ubuntu* 22.04. For setup instructions for other operating systems, refer to the [**“Establishing a Build Environment”**](https://source.android.com/docs/setup/start/requirements) section on the AOSP website.

2. Ensure that all Android build prerequisites are met before starting the build process.


## Set up the Build Environment

The Android source code is distributed across multiple Git\* repositories. The
repo tool simplifies the process of managing and synchronizing those repositories. Refer to the
[Git Setup for Build Environment](#git-setup-for-build-environment) if you need to set up Git on your build machine.

1.  Create a local bin/ directory, download the repo tool to that directory, and make the binary executable with the following commands:

```bash
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
export PATH=~/bin:$PATH
```

2.  Install the following required packages on your 64-bit Ubuntu 22.04 LTS development workstation before the building the Android BSP:
```bash
sudo apt-get update
sudo apt-get install -y wget openjdk-8-jdk git ccache automake \
   lzop bison gperf build-essential zip curl \
   zlib1g-dev g++-multilib python3-networkx \
   libxml2-utils bzip2 libbz2-dev libbz2-1.0 \
   libghc-bzlib-dev squashfs-tools pngcrush \
   schedtool dpkg-dev liblz4-tool make optipng maven \
   libssl-dev bc bsdmainutils gettext python3-mako \
   libelf-dev sbsigntool dosfstools mtools efitools \
   python3-pystache git-lfs python-is-python3 flex clang libncurses5 \
   fakeroot ncurses-dev xz-utils cryptsetup-bin \
   apt-transport-https ca-certificates curl lsb-release \
   rsync vim python-six kmod glslang-tools \
   software-properties-common cpio python3-pip ninja-build \
   cutils cmake pkg-config xorriso mtools libjson-c-dev file

sudo pip3 install meson==1.8.3 mako==1.1.0 dataclasses pycryptodome ply==3.11 

# OneAPI Integration support has been added
# but it has dependencies which need to be installed on the build environment

wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB  \
   | gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null && \
   echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" \
   | sudo tee /etc/apt/sources.list.d/oneAPI.list

sudo apt-get update 

sudo apt install -y \
  intel-oneapi-ipp-devel-2021.10 \
  intel-oneapi-mkl-devel-2021.1.1 \
  intel-oneapi-ipp-devel-32bit-2021.10 \
  intel-oneapi-mkl-devel-32bit-2021.1.1
```

> **Note:**
> If you encounter network connectivity issues, you can use the following commands
> to bypass using network proxy settings. To disable proxy settings:

```bash
unset no_proxy
unset NO_PROXY
export no_proxy=localhost
export NO_PROXY=localhost
```
In some cases, the system may use the system default no_proxy configuration. To override this behavior, set a dummy no_proxy value.
After updating the no_proxy settings, run apt commands using the -E option:

```bash
sudo -E apt update
sudo -E apt install … 
```

## Download and Build the Source Code

This section outlines the procedures for downloading the Android source code using the specified manifest and for building the Android BSP.

The release manifest is available in the [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/master/stable-build/A16/) repository.

The manifest for this release is **BM_BSP_2026_Q3_V2_A16.xml**

1.  Download the manifest: **BM_BSP_2026_Q3_V2_A16.xml**

```bash 
mv BM_BSP_2026_Q3_V2_A16.xml ~/.
```
2.  Create a working directory.
```bash 
mkdir ~/rpl-android-bm
cd ~/rpl-android-bm
```
3.  Download the source code from the manifest. For the latest on the branch:
```bash
# For the latest on the branch:
repo init -u https://github.com/edge-aosp-bsp/manifest.git  

# copy the manifest to .repo/manifests
mkdir .repo/manifests
cp ~/BM_BSP_2026_Q3_V2_A16.xml .repo/manifests/.

repo init -u https://github.com/edge-aosp-bsp/manifest.git -m BM_BSP_2026_Q3_V2_A16.xml

# Sync the repositories
repo sync -c --force-sync -j16
repo forall -c git lfs pull

```

> **Note:**
> The `repo sync` process may take an extended amount of time to complete.
> This behavior is expected and should not be interpreted as a system crash.
>
> You can use the `-j` flag to specify the number of parallel processes for
> `repo sync` to speed up the build. Adjust the value based on the number of
> available CPU cores, and remove the flag if you encounter issues. For example, `repo sync -c --force-sync -j16`

## Build Instructions

### Trusty and AVF Feature Configuration

AVF and Trusty cannot be enabled at the same time. Choose the build that matches your platform and required feature:

| Build             | Trusty   | AVF      | Supported Platforms                                                                                            | Env var (before `lunch`)     | Make flag          |
| ----------------- | -------- | -------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------- | ------------------ |
| **Default Build** | Disabled | Disabled | Intel® Core™ Processor (14th Gen), Intel® Core™ 3 Processor N355, Intel® Processor N150, Intel® Processor N250 | `export ENABLE_TRUSTY=false` | `ENABLE_AVF=false` |
| **Trusty Build**  | Enabled  | Disabled | Intel® Core™ Processor (14th Gen) only                                                                         | `export ENABLE_TRUSTY=true`  | `ENABLE_AVF=false` |
| **AVF Build**     | Disabled | Enabled  | Intel® Core™ Processor (14th Gen) only                                                                         | `export ENABLE_TRUSTY=false` | `ENABLE_AVF=true`  |

> **Note:**
> If you are building for Intel® Core™ 3 Processor N355, Intel® Processor N150, or Intel® Processor N250, use the **Default Build** — the AVF Build and Trusty Build are not available on these platforms.
>
> Each build below sets `lunch caas-userdebug` as the build target. Use `caas-userdebug` for development and debugging, including root access and debug tools. Use `caas-user` for a release-oriented configuration with reduced debugging capability.

### Default Build (Trusty Disabled, AVF Disabled)

```bash
# Export the Trusty flag before `lunch`
export ENABLE_TRUSTY=false

# Prepare build environment
source build/envsetup.sh

# Build target can be caas-user or caas-userdebug
lunch caas-userdebug

# Start the build
make flashfiles BASE_LINUX_INTEL_LTS2024_KERNEL=true ENABLE_AVF=false -j16
```

### Trusty Build (Trusty Enabled, AVF Disabled)
Supported only on Intel® Core™ Processor (14th Gen).

**Step 1: Copy the Trusty binary**
```bash
cp lk.bin ~/rpl-android-bm/vendor/intel/fw/trusty-release-binaries/
```
> **Note:** Contact your Intel representative for access to the Trusty source code and build procedure.

**Step 2: Build with Trusty enabled**
```bash
# Export the Trusty flag before `lunch`
export ENABLE_TRUSTY=true

# Prepare build environment
source build/envsetup.sh

# Build target can be caas-user or caas-userdebug
lunch caas-userdebug

# Start the build
make flashfiles BASE_LINUX_INTEL_LTS2024_KERNEL=true ENABLE_AVF=false -j16
```

### AVF Build (Trusty Disabled, AVF Enabled)
Supported only on Intel® Core™ Processor (14th Gen).

**Disclaimer:** AVF capability in the Intel Android 16 BSP is based on Google’s Android 16 AOSP implementation, has limited maturity for x86 platforms. For x86 devices, AVF operates using non-protected virtual machines, does not support protected virtual machines, lacks CTS/VTS coverage, and may not receive Google security updates for AVF-related x86 code paths. Accordingly, Intel makes no representation or warranty regarding security isolation, certification compliance, the availability of AVF-related security updates, or the continued availability of future AOSP or Google support for these features.

```bash
# Export the Trusty flag before `lunch`
export ENABLE_TRUSTY=false

# Prepare build environment
source build/envsetup.sh

# Build target can be caas-user or caas-userdebug
lunch caas-userdebug

# Start the build with AVF enabled
make flashfiles BASE_LINUX_INTEL_LTS2024_KERNEL=true ENABLE_AVF=true -j16
```

### Build Output
The generated build output files are available at the following directory:

```bash
find out -name *.tar.gz
out/target/product/caas/caas-releasefile-userdebug.iso.tar.gz
out/target/product/caas/caas-releasefiles-userdebug.tar.gz

# Note:	The files are available at:
# ~/rpl-android-bm/out/target/product/caas/
```

# Android\* Image Flashing and Boot-Up

This section describes the steps required to configure the BIOS and prepare the USB drive for flashing the image to the board.

## BIOS Settings 

Verify that the BIOS settings match the values in the following table. These values are the default settings for the supported IFWI versions.

Press the appropriate BIOS setup key (for example F2, Del, or F12) during startup to access the BIOS Setup menu.

### BIOS Configuration

| Name                       | Menu                                                     | Setting     |
| -------------------------- | -------------------------------------------------------- | ----------- |
| Intel (VMX) Virtualization | Intel Advanced Menu → CPU Configuration                  | Enabled     |
| VT-d                       | Intel Advanced Menu → System Agent (SA) Configuration    | Enabled     |
| Intel® TCC Mode            | Intel Advanced Menu → Intel® Time Coordinated Computing  | Disabled    |
| #AC Split Lock             | Intel Advanced Menu → Intel® Time Coordinated Computing  | Disabled    |
| OnBoard NIC                | Intel Advanced Menu → PCH‑IO configuration → EFI Network | OnBoard NIC |


> **Note:**
> The steps may vary depending on the BIOS.
>
> Intel® TCC Mode and #AC Split Lock are disabled by default because they are intended for real-time/deterministic workloads and are not required for standard Android BSP bring-up; leave them disabled unless your use case specifically requires TCC.

## Flash Image to USB Drive

There are two steps to flash **caas-flashfile-\<\$variant\>.iso.zip** to the system. 

### Step 1: Flash Image to the USB Drive

On a Windows\* machine, use Rufus or similar tool to create a bootable USB stick from **caas-flashfile-\<\$variant\>.iso.zip** to USB drive. The Rufus app can be downloaded from <https://rufus.ie/en/>

First, extract the caas-flashfile-\<\$variant\>.iso.zip file.

#### Select the ISO Image to Flash

<p align="center">
  <img src="./media/image1.png" alt="Select the ISO Image to Flash"/>
</p>

#### Example of Flashing in Progress

<p align="center">
  <img src="./media/image2.png" alt="Example of Flashing in Progress">
</p>

#### Flashing Completed

<p align="center">
  <img src="./media/image3.png" alt="Flashing Completed">
</p>

Alternatively, on Ubuntu, you can also use the dd command.

**[IMPORTANT NOTICE]** Replace the /dev/sdc in the following example with the target USB device node name.

```bash
unzip caas-flashfile-userdebug.iso.zip
dd if=./caas-flashfile-userdebug.iso of=/dev/sdc bs=1024M  
#(takes 1-3 minutes depending on USB speed)

```

### Step 2: Boot the System into Android

1. Insert the USB drive into the board.
2. Press **F2** while booting the device.
3. Select the USB drive to boot as shown below. Navigate to:  
   
   **Boot Maintenance → Boot Option Menu → Change Boot Order**
4. Set the USB drive as the first boot option. Save the changes (**Fn + F4**) and exit the BIOS (select **Continue**).

#### BIOS Settings to Change Boot Order

![BIOS Settings to Change Boot Order](./media/image4.png)

5. Press UP arrow, PG UP, RIGHT, or HOME to proceed with installation.

#### Installer Screen

![Installer Screen](./media/image5.png)

6. This action starts the Android installation process. 

**Android Installation Progress**

![Android Installation Progress](./media/image6.png)

7. After the installation completes, remove the USB drive and reboot the system. The device completes the installation and
boots into Android.

#### Android Home Screen

![Android Home Screen](./media/image7.png)

> **Note:**
> You can use either an NVMe or SATA as the storage device.

## Git Setup for Build Environment
Git must be set up on your build machine to run repo init. Use the command below as a guideline:  

\# Setup git config with your name and email ID. Add proxy settings if behind a firewall  

```bash
cd /home/$USER  
vi /home/$USER/.gitconfig  
```

\# Append the following lines to .gitconfig file  
```
[user]  
    email = <your email>  
    name = <your name>  
[http]  
    proxy = <http_proxy>  
[https]  
    proxy = <https_proxy>  
```
Create a symbolic link for Python 3 in the ‘/usr/bin’ directory.
sudo ln -sf /usr/bin/python3 /usr/bin/python

# Developer Guide for Static Location Service

This section provides guidance for application developers to read or write the device's static configured location.

## Location Provider

The Location provider name to be used in LocationManager API calls while reading the location:

```java
"static"
```

---

## Read the Location

It can be used by applications to determine the location of the device.

### Required Android Permission

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```

### Sample Code

```java
LocationManager lm = (LocationManager)getSystemService(LocationManager.class);

Location location = lm.getLastKnownLocation("static");

if (location != null) {

    double lat = location.getLatitude();
    double lon = location.getLongitude();

    Log.d("STATIC_PROVIDER", "Lat=" + lat + " Lon=" + lon);
}
```

---

## Write / Update the Location

It is used by manageability applications to statically configure the device location at the time of provisioning.

> This feature is supported only for privileged/system applications.

### Required Permission

```xml
<uses-permission android:name="android.permission.LOCATION_HARDWARE" />
```

### Sample Code

```java
IBinder binder = ServiceManager.getService("location_control");

ILocationControlService service = ILocationControlService.Stub.asInterface(binder);

service.setHalLocation(12.9716, 77.5946);
```

---
# Developer Guide for Validating Trusty Enablement

Trusty is a secure operating system that provides a Trusted Execution Environment (TEE) for Android and supports security services, including Gatekeeper and KeyMint. In this release, Trusty is supported only on Intel® Core™ Processor (14th Gen). Use the steps below to confirm that Trusty is correctly enabled on the flashed device.

> **Note:**
> Run these checks using `adb` after the device has fully booted into Android. The image must be built with Trusty enabled (see [Trusty Build](#trusty-build-trusty-enabled-avf-disabled)).

## Step 1: Verify the Trusty HAL Properties

Confirm that the Gatekeeper and Keystore hardware-selection properties are set to Trusty.

Check the Gatekeeper property:

```bash
adb shell getprop ro.hardware.gatekeeper
```

Expected output:

```
trusty
```

Check the Keystore property:

```bash
adb shell getprop ro.hardware.keystore
```

Expected output:

```
trusty
```

## Step 2: Verify the Trusty Device Nodes Are Present

Verify that the Trusty IPC and log device nodes are exposed by the kernel:

```bash
adb shell ls -la /dev/trusty*
```

Expected output (device numbers may vary):

```
crw-rw---- 1 system drmrpc 246,   0 2026-09-16 18:18 /dev/trusty-ipc-dev0
crw------- 1 root   root    10, 259 2026-09-16 18:18 /dev/trusty-log0
```

The check passes when both `/dev/trusty-ipc-dev0` and `/dev/trusty-log0` are listed.
## Step 3: Confirm the KeyMint and Gatekeeper Services Are Backed by Trusty

Verify that the Trusty-backed KeyMint and Gatekeeper HAL services are running:

```bash
adb shell ps -ef | grep -iE "keymint|gatekeeper"
```

Example output (PIDs, start times, and formatting may vary):

```
nobody         369     1 0 23:18:01 ?     00:00:00 android.hardware.security.keymint-service.rust.trusty --dev /dev/trusty-ipc-dev0
system         612     1 0 23:18:09 ?     00:00:00 android.hardware.gatekeeper-service.trusty --dev /dev/trusty-ipc-dev0
system         728     1 0 23:18:10 ?     00:00:00 gatekeeperd /data/misc/gatekeeper
```

The Trusty-specific KeyMint and Gatekeeper HAL executables are the primary indicators that Trusty is enabled. Both `android.hardware.security.keymint-service.rust.trusty` and `android.hardware.gatekeeper-service.trusty` must be present to confirm that the two HAL services are backed by Trusty. If `android.hardware.gatekeeper-service.nonsecure` appears instead, Gatekeeper is running without Trusty backing.

The `gatekeeperd` process is expected, but its presence alone does not prove that Trusty is enabled. Verify that the Trusty-specific HAL executables shown above are also running.

> **Note:** If any of the checks above fail, verify that the target platform is Intel® Core™ Processor (14th Gen), export `ENABLE_TRUSTY=true` before running `lunch`, and pass `ENABLE_AVF=false` to the `make` command. In this BSP configuration, Trusty and AVF are mutually exclusive; a build with AVF enabled will not expose Trusty.

---
# Developer Guide for Validating Android Virtualization Framework (AVF) Enablement

On this Intel x86_64 BSP, AVF is configured to support non-protected VMs using KVM. Protected VMs are not supported. Use the steps below to confirm that AVF is correctly enabled on the flashed device.


> **Note:**
> This guide assumes the image was built using the **AVF Build** which is supported only on Intel® Core™ Processor (14th Gen) (see [Trusty and AVF Feature Configuration](#trusty-and-avf-feature-configuration)). Run these checks using `adb` after the device has fully booted to Android. Ensure  Intel® (VMX) Virtualization and Intel® VT-d are **Enabled** in the BIOS (see the BIOS Configuration section).

## Step 1: Verify the Hypervisor Capability Properties

Confirm that the platform advertises the expected KVM hypervisor capabilities to the Android framework:

```bash
adb shell getprop ro.boot.hypervisor.vm.supported
adb shell getprop ro.boot.hypervisor.protected_vm.supported
adb shell getprop ro.boot.hypervisor.version
```

Expected values:

| Property                                    | Expected Value | What does it mean?                        |
| ------------------------------------------- | -------------- | ------------------------------------------ |
| `ro.boot.hypervisor.vm.supported`           | `1`            | Non-Protected VMs are supported            |
| `ro.boot.hypervisor.protected_vm.supported` | `0`            | Protected VMs are not supported            |
| `ro.boot.hypervisor.version`                | `kvm`          | This BSP identifies KVM as its hypervisor. |

These properties advertise the platform’s hypervisor capabilities. However, these properties alone do not confirm that a VM can be launched successfully.
## Step 2: Confirm the KVM Device Node Is Present

Verify that the KVM device node is exposed by the kernel:

```bash
adb shell ls -l /dev/kvm
```

The command should list the `/dev/kvm` character device. If the device node is missing, verify that the image was built using the AVF Build configuration with Trusty disabled, that Intel® VMX virtualization is enabled in the BIOS, and that the KVM and KVM-Intel kernel components were initialized successfully. Check the kernel logs for KVM initialization errors if the problem persists.

## Step 3: Verify the AVF User-Space Components

Verify that the `com.android.virt` APEX and the `vm` tool are available:

```bash
adb shell ls /apex/com.android.virt/bin/vm
```

The `vm` binary should be listed, confirming that the AVF APEX and command-line tool are installed on the image. This check alone does not confirm that KVM is usable or that a VM can be started.

## Step 4: Query AVF Using the `vm` Tool

Verify the available AVF binaries, and then run the `vm` info command to confirm that the framework reports virtualization support. The following shows example output from the validated image; VFIO status, assignable devices, and debug-policy values may vary by configuration.

```bash
adb shell ls /apex/com.android.virt/bin/

crosvm
early_virtmgr
fd_server
vfio_handler
virtmgr
virtualizationservice
vm
vmnic
```

```bash
adb shell /apex/com.android.virt/bin/vm info

Only non-protected VMs are supported.
Hypervisor version: kvm
/dev/kvm exists.
/dev/vfio/vfio exists.
VFIO-platform is not supported.
Assignable devices: []
Available OS list: ["microdroid"]
Debug policy: Ok(DebugPolicy { log: false, ramdump: false, adb: false })
```

The command should return successfully and report that non-protected VM support is available, the hypervisor version is kvm, /dev/kvm exists, and microdroid is listed as an available OS. A successful vm info result confirms that AVF is configured and the virtualization service is responsive. 

> **Note:**
> If any of the steps above fail, confirm that the image was built with the AVF Build (Trusty disabled, AVF enabled) and that the required BIOS virtualization settings are enabled. Because Trusty and AVF are mutually exclusive, a build with Trusty enabled will not expose AVF. 

---
## Reference Documents

| Documentation on GitHub                                                                                                                                                                                        | Document No./Location                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Android* 16 Base BSP Reference Release for Edge Platforms (supporting Intel® Core™ Processor (14th Gen), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355) Release Notes | [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/master/README.md)         |
| Android Manifest File (supporting Intel® Core™ Processor (14th Gen), Intel® Processor N150 & N250, Intel® Core™ 3 Processor N355)                                                          | [GitHub](https://github.com/edge-aosp-bsp/manifest/blob/master/stable-build/A16/) |

Log in to the Resource and Documentation Center ([rdc.intel.com](https://www.intel.com/content/www/us/en/resources-documentation/developer.html)) to search for and download the document numbers listed in the following table. Contact your Intel field representative for access.

> **Note:**
> Third-party links are provided as a reference only. Intel does not control or audit third-party benchmark data or the websites referenced in this document. You should visit the referenced website and confirm whether the referenced data are accurate. 


| Documentation on Intel RDC                                                                                                                                                                              | Document No./Location                                                                                                    |     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | --- |
| 13th Gen Intel® Core™ Desktop Processors (Code named Raptor Lake-S) and Intel® Core™ Processor (14th Gen) (Code named Raptor Lake-S Refresh) for Edge Platforms Reference UEFI BIOS/IFWI (ver. 7117_51) | [852225](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=852225) |     |
| Intel® Processor N Series, Intel® Core™ i3-N305 Processor, Intel Atom® x7000,x7000RE&x7000C,x7000FE Processor Series, Intel® Processor N150&N250, Intel® Core™ 3 Processor N355 for Edge Applications (IPU2026.3) Firmware Best Known Configuration (BKC)   | [919389](https://www.intel.com/content/www/us/en/secure/design/confidential/software-kits/kit-details.html?kitId=919389) |     |


# Disclaimer

You may not use or facilitate the use of this document in connection with any infringement or other legal analysis concerning Intel products described herein. You agree to grant Intel a non-exclusive, royalty-free license to any patent claim thereafter drafted which includes subject
matter disclosed herein.

No license (express or implied, by estoppel or otherwise) to any intellectual property rights is granted by this document.

All information provided here is subject to change without notice. Contact your Intel representative to obtain the latest Intel product specifications and roadmaps.

The products described may contain design defects or errors known as errata which may cause the product to deviate from published specifications. Current characterized errata are available on request.

Copies of documents which have an order number and are referenced in this document may be obtained by calling 1-800-548-4725 or visiting the [Intel Resource and Documentation Center](https://www.intel.com/content/www/us/en/resources-documentation/developer.html). 

Intel technologies\' features and benefits depend on system configuration and may require enabled hardware, software or service activation. Performance varies depending on system configuration. No product or component can be absolutely secure. Check with your system
manufacturer or retailer or learn more at [intel.com](http://intel.com/).

The Bluetooth® word mark and logos are registered trademarks owned by Bluetooth SIG, Inc. and any use of such marks by Intel Corporation is under license.

© Intel Corporation. Intel, the Intel logo, and other Intel marks are trademarks of Intel Corporation or its subsidiaries. Other names and brands may be claimed as the property of others.
