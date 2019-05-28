# Sentinel Simulator Installer - Automated Installation

## Automatically Download, Extract and Install Latest or Specific Version

[![release](https://img.shields.io/github/release/robertpeteuil/sentinel-installer.svg?colorB=2067b8)](https://github.com/robertpeteuil/sentinel-installer)
[![bash](https://img.shields.io/badge/language-bash-89e051.svg?style=flat-square)](https://github.com/robertpeteuil/sentinel-installer)
[![license](https://img.shields.io/github/license/robertpeteuil/sentinel-installer.svg?colorB=2067b8)](https://github.com/robertpeteuil/sentinel-installer)

---

The **sentinel-install** script automates the process of downloading and installing HashiCorp Sentinel Simulator.  It provides an ideal method for installing on new hosts, installing updates and downgrading if necessary.

This script detects the latest version, OS and CPU-Architecture and allows installation to local or system locations.  Optional parameters allow installing a specific version and installing to /usr/local/bin without prompting.

Options:

- `-i VERSION`:  Install specific version
- `-a`:          Automatically use `sudo` to install to /usr/local/bin
  - allows for unattended installation via scripts or CD tools
  - can be set as default behavior by uncommenting line 14 (`sudoInstall=true`)
  - sudo password may be required unless NOPASSWD is enabled
- `-c`:          leave binary in working directory (for CI/DevOps use)
- `-h`:          help
- `-v`:          display version

This installer is similar to my [Terraform Installer](https://github.com/robertpeteuil/terraform-installer) and [Packer Installer](https://github.com/robertpeteuil/packer-installer)

## Install

Download Installer

``` shell
curl -LO https://raw.github.com/robertpeteuil/sentinel-installer/master/sentinel/simulator-install.sh
chmod +x sentinel-install.sh
```

## Use

### Run local installer

``` shell
./sentinel-install.sh

# usage: sentinel simulator-install.sh [-i VERSION] [-a] [-c] [-h] [-v]
#      -i VERSION : specify version to install in format '' (OPTIONAL)
#      -a : automatically use sudo to install to /usr/local/bin
#      -c : leave binary in working directory (for CI/DevOps use)
#      -h : help
#      -v : display sentinel-install.sh version
```

### Express install latest version via `iac.sh` or `https://iac.sh` (my bootstrap server)

``` shell
curl iac.sh/sentinel | sh
```

## System Requirements

- System with Bash Shell (Linux, macOS, Windows Subsystem for Linux)
- `unzip` - Sentinel Simulator downloads are in zip format
- `curl` or `wget` - script will use either one to retrieve metadata and download
- `jq` - required to get latest version from hashicorp downloads

## Script Process Details

- Determines Version to Download and Install
  - Uses Version specified by `-i VERSION` parameter (if specified)
  - Otherwise determines Latest Version using `jq`
- Calculates Download URL based on Version, OS and CPU-Architecture
- Verifies URL Validity before Downloading in Case:
  - VERSION incorrectly specified with `-i`
  - Download URL Format Changed on Sentinel Simulator Website
- Determines Install Destination
  - Performed before Download/Install Process in case user selects `abort`
- Installation Process
  - Download, Download SHA, Verify SHA of zip, Extract, Install, Cleanup and Display Results

### CPU Architecture Detection

CPU architecture is detected for each OS accordingly:

- Linux / Windows (WSL since this is a Bash script)
  - detected with `lscpu` or by inspecting `/proc/cpuinfo`
- macOS - uses Default Arch `amd64` as it's the only version available on macOS
- Default Value - `amd64`

### License

Apache 2.0 License - Copyright (c) 2019    Robert Peteuil
