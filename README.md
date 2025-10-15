# Analog_Layout
To provide comprehensive documentation, best practices, and workflow guidelines for analog IC layout design, including methodologies for translating SPICE netlists to physical layouts, integration with open-source tools, layout constraint handling, and PDK configuration references, thereby supporting accurate, efficient, and scalable analog layout development and education within the repository community.

# What is Analog Layout?
- Analog layout is the process of translating analog circuit schematics into their physical implementation on a silicon chip, focusing on the arrangement of transistors, resistors, capacitors, and interconnections using specialized geometric patterns for each layer of the IC.
- This discipline requires careful consideration of parasitic effects, electromagnetic interference, heat dissipation, and manufacturing constraints to ensure that real-world signal fidelity and performance are maintained.
- Analog layout typically involves manual placement and routing using computer-aided design tools, with extensive verification steps such as Design Rule Checking (DRC) and Layout Versus Schematic (LVS) to ensure the final layout matches the intended electronic behavior.

# Content
- [1. Tool and PDK Setup](#1-tools-and-pdk-setup)
  - [1.1 Tools Required](#11-tools-required)
  - [1.2 PDK Required](#12-pdk-required)
  - [1.3 Install and Setup EDA Tools](#13-install-and-setup-eda-tools)






# 1. Tools and PDK setup

## 1.1 Tools Required
For the simulation of circuits we will need the following tools.
- Spice netlist simulation - [[Ngspice](https://ngspice.sourceforge.io/)]
- Schematic Editor - [[Xschem](https://xschem.sourceforge.io/stefan/index.html)]

### Ngspice
![Diagram](Doc/nglogo.jpg)

[Ngspice](http://ngspice.sourceforge.net/devel.html) is the open source spice simulator for electric and electronic circuits. Ngspice is an open project, there is no closed group of developers.

[Ngspice Reference Manual](https://ngspice.sourceforge.io/docs/ngspice-html-manual/manual.xhtml): Complete reference manual in HTML format.

### Xschem
![Diagram](Doc/xschem_logo.png)

[Xschem](https://xschem.sourceforge.io/stefan/) is an open-source schematic capture tool for VLSI and electronics. It is designed to be lightweight, fast, and capable of handling large hierarchical circuits while remaining user-friendly.

[Xschem Reference Manual](https://xschem.sourceforge.io/stefan/xschem_man/xschem_man.html): Complete reference manual in HTML format.

### Magic_vlsi


## 1.2 PDK Required

A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use is [Google Skywater 130nm PDK](https://skywater-pdk.readthedocs.io/en/main/).

![Diagram](Doc/Skywater130.png)

Device Details: [docs](https://skywater-pdk.readthedocs.io/en/main/rules/device-details.html)

## 1.3 Install and Setup EDA Tools
## Windows Subsystem for Linux (WSL) for Open Source EDA tools

Windows Subsystem for Linux (WSL) is a feature of Windows that allows you to run a Linux environment on your Windows machine, without the need for a separate virtual machine or dual booting. With native X11 (graphics) support on WSL2, the latest WSL, in **Winodws 10 version 2004+ (Build 19041+)** or **Windows 11**, you can now run GUI apps including all the open-source EDA tools.

Now we will share instructions for installing WSL2 on Winodws 10/11 and install the EDA tools on a **Ubuntu 24.04** distribution.

## Install WSL
- **Prerequisites**: Winodws 10 version 2004+ (Build 19041+) or Windows 11
- If you have a previous WSL installed without your knowledge or, you've installed in from the _Microsoft Store_, it's best you **uninstall** it using the Windows "Add/remove Programs" app and/or `wsl --uninstall`
- Open PowerShell or Windows Command Prompt in **ADMINISTRATOR** mode by right-clicking and selecting "Run as administrator"
- In the PowerShell type `wsl --list --online`
  - This will list all the available distributions online. 
- To install a particular distribution (Distro) say `Ubuntu-24.04` (The name has to be exactly as printed in the above command):
  - `wsl --install -d Ubuntu-24.04`
- It's important to update the WSL now by typing the following in the Powershell:
  - `wsl --update`
- And shut it down: `wsl --shutdown`. Note: it will automatically start when the WSL distro selected from the Windows menu.

## Launching Ubuntu 24.04. 

- `Press Windows Key → select Ubuntu 24.04.`
- Update the system
```
  sudo apt update && sudo apt upgrade -y
```
- Clone the Github Repository
```
cd ~
git clone https://github.com/silicon-vlsi/SI-2025-AnalogIC.git

```
- Copy & make install scripts executable
```
cp ~/SI-2025-AnalogIC/install*.sh .
chmod +x install*.sh
```
- Install dependencies & EDA tools
```
./install-libs.sh
./install-eda.sh
```
- Add EDA environment variables
```
cat ~/SI-2025-AnalogIC/bashrc-eda >> ~/.bashrc
source ~/.bashrc
```
- Then type this command: 
``` 
tree -L 2
```
**Directory Structure** after installation should look like this:
```bash
share
└── pdk
cad
├── eda-magic
├── eda-netgen
├── eda-ngspice
└── eda-xschem
work
└── xschem
.xschem/
└── simulations
```
### PDK Linking with Magic:

```
vim ~/.magicrc
```
- Inside the file save this:
```
tech load $HOME/share/pdk/sky130A/libs.tech/magic/sky130A.tech
addpath $HOME/share/pdk/sky130A/libs.ref/sky130_fd_pr/mag
```


