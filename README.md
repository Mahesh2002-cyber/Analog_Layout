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
- [2. Brief about complimentary CMOS Transistors](#2-Brief-about-complimentary-CMOS-Transistors)
  - [2.1 Schematic of CMOS Inverter](#1.1-Schematic-of-CMOS-Inverter)
  - [2.2 Layout view(top_view) of NMOS and PMOS transistors](#1.2-Layout-view(top_view)-of-NMOS-and-PMOS-transistors)
  - [2.3 Physical Layout view of CMOS Inverter](#1.3-Physical-Layout-view-of-CMOS-Inverter)
- [3. Schematic drawing in xschem](#3-Schematic-drawing-in-xschem)
  - [3.1 Steps to make the schematic in xschem](#3.1-Steps-to-make-the-schematic-in-xschem)
  - [3.2 Generating netlist of a schematic](#3.2-Generating-netlist-of-a-schematic)
- [4. Magic_vlsi Layout](#4-Magic-vlsi-Layout)
  - [4.1 About Magic_vlsi](#4.1-About-Magic_vlsi)
  - [4.2 Magic Setup](#4.2-Magic-Setup)
  - [4.3 Basic functions in MAGIC.](#4.3-Basic-functions-in-MAGIC)
  - [4.4 Lambda Based Design Rules](#4.4-Lambda-Based-Design-Rules)
  - [4.5 Steps to make Layout for CMOS Inverter in Magic tool.](#4.5-Steps-to-make-Layout-for-CMOS-Inverter-in-Magic-tool)


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
![Diagram](Doc/magic_vlsi.png)

[Magic_vlsi](http://opencircuitdesign.com/magic/)The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.

[Magic_vlsi Reference Manual](http://opencircuitdesign.com/magic/techref/maint2.html)Complete Reference manual for your reference.

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

# 2. Brief about complimentary CMOS Transistors
- Pull-up network consists of P-type device.
- PMOS device should connect to vdd always.
- Pull-down network consists of n-type devices.
- NMOS device should connect to gnd always.
<img width="1509" height="1071" alt="image" src="https://github.com/user-attachments/assets/ed8e2426-3526-447e-b57d-b2545bed5b80" />

- CMOS Inverter Consists of a PMOS and a NMOS connected in following fashion.
- Stick diagram explains the positioning of P-diffusion, N-diffusion and
  polysilicon positioning.
## 2.1 Schematic and stick Diagram
<img width="1146" height="511" alt="image" src="https://github.com/user-attachments/assets/c2df450b-3639-4e3f-ac54-5587ef346952" />

## 2.2 Layout view (top_view) of NMOS and PMOS Transistors
<img width="1245" height="811" alt="image" src="https://github.com/user-attachments/assets/232c2136-3f02-474e-a105-99fc07677834" />

## 2.3 Physical Layout view of CMOS Inverter
<img width="1146" height="866" alt="image" src="https://github.com/user-attachments/assets/c9ac189a-acba-4f47-8dcb-56d4b605f998" />

# 3 Schematic drawing in xschem

## 3.1 Steps to make the schematic in xschem
- Open terminal in ubuntu, Type “cd work/xschem”.
- Type ”xschem”, It opens the xschem window.
- Go to file select create new window/tab.
- Then press “shift+I” to get instance (devices).
- Select your xschem path in that select technology file “sky130_fd_pr”.

  <img width="719" height="355" alt="image" src="https://github.com/user-attachments/assets/7e28a9bd-e7f2-4bcc-bdb5-c1bf51566d07" />

- Then select “pfet3_01v8.sym”, for PMOS device

  <img width="719" height="353" alt="image" src="https://github.com/user-attachments/assets/6eb26a7a-eb57-4aac-9450-cc2b8b2c910c" />

- Click on schematic editor window.
- Again press “shit+i” select “nfet3_01v8.sym” for NMOS.
- Select the device and press "Q" it will show the properties of a device, then change the device length and widths as per requirement.
- Repeate the above step for all devices.

  <img width="725" height="354" alt="image" src="https://github.com/user-attachments/assets/4fdeca55-9551-4b3b-81e8-c143b6a8a627" />

- Connect the terminals as per circuit diagram. Use “w” for wire or select the tip of the terminal and drag.
- Give the pins as per signal flow.
- To get pins press “shift+i” choose device library, in that select ipin, for input.

  <img width="724" height="417" alt="image" src="https://github.com/user-attachments/assets/d0dba855-1f76-4c67-b074-e50e32dbf9f3" />

- Same for output pin press “shift+i” choose device library, in that select opin.
- Repeate the above step for all pins.

  <img width="723" height="422" alt="image" src="https://github.com/user-attachments/assets/af95a0a4-332c-4e2b-a86a-182bdd115d30" />

- Connect all the pins, Then give a pin name(label) by selecting the pin and press “Q”.
- Here new window will pop-up, give a pin name and click on ok.

  <img width="682" height="320" alt="image" src="https://github.com/user-attachments/assets/05a301ed-cb4d-403b-9b94-ddab54772659" />

- Repeat the above step for all pins.
- Make all connections as per circuit diagram.
- Make sure all connections should be proper.
- Don’t make any short’s and open connections.

  <img width="1260" height="682" alt="image" src="https://github.com/user-attachments/assets/dc91ab2a-540b-4c34-ae99-8f8019cc4231" />

- After completing the connections save the schematic with file filename.
## 3.2 Generating netlist of a schematic
- Click on Simulations, then select lvs, then click on Lvs netlist.

  <img width="1262" height="680" alt="image" src="https://github.com/user-attachments/assets/7f0666dd-38a5-4fdb-ab91-429c270d9504" />

- Click on Netlist. It will create a netlist of your schematic.

  <img width="1443" height="1020" alt="image" src="https://github.com/user-attachments/assets/e2e44aa0-09c0-4415-b131-16f0e097d883" />

- If all connections are made properly then the netlist will blink in green colour.
- Here it ends the schematic design of your circuit.

# 4. Magic_vlsi Layout

## 4.1 About Magic_vlsi

- MAGIC has its backronym as Manhattan Artwork Generator for Integrated Circuits.
- MAGIC is a venerable and easy to use VLSI layout tool.
- Magic features real-time design rule checking, something that some costly commercial VLSI design software packages don't feature Magic is based on "scalable CMOS" style of design using "lambda-based" dimensions.
- This Layout tool helps to identify the hidden parasitics in the design.
- MAGIC is available on Linux. For Windows, additional installations are required.

## 4.2 Magic Setup
- Recommended to use a MOUSE.
- Magic contains 2 windows: Layout window (Designing) and Console Window (Commands) In the Layout window,
- Press "g" to view grid
- The grid is a square predefined with 'X' dimensions in the Layout window To change the grid dimensions: Go to View -> Select a Grid Dimension Go to Options and Click on Toolbar The toolbar contains all design essentials necessary for a layout for example nwell, pwell, pdiffusion layer, polysilicon, pdcontact etc.

  <img width="1452" height="841" alt="image" src="https://github.com/user-attachments/assets/95dfc403-601f-4262-94e3-c68ae1279723" />

  <p align="center"><b>Fig.Layout window.</b></p>

   <img width="990" height="356" alt="image" src="https://github.com/user-attachments/assets/b3751d08-d639-41db-bd2a-443e1592e55a" />

<p align="center"><b>Fig.Console window.</b></p>

 - To check the Technology File: Go to Options.
 - Open Tech Manager - Technology should be “sky130A”.

   <img width="388" height="213" alt="image" src="https://github.com/user-attachments/assets/f6f83ece-10de-472d-98a4-7bf711246002" />


## 4.3 Basic functions in MAGIC.
It’s consider that whole area is “pwell”.
 - To select a square in Magic:-
   Use left-mouse click to select bottom left vertex and right-mouse click at the diagonally opposite vertex of required cell block to select it.
 - To paint an area inside a square:-
   After selecting the box, from the tool-bar select the material to fill in the box by clicking on the scroll-wheel on the mouse.
   If you don’t have a mouse, write paint<material name> in the console window.
 - To erase an area inside a square:-
   To erase material from the selected box, click the Scroll-wheel on the mouse over an empty area(P-well).
   If you don’t have a mouse, write erase in the console window.

## 4.4 Lambda Based Design Rules.
 - Lambda(λ) = 90nm (By default equivalent to 1 grid size) Lambda is a scale factor used to define the minimum technology geometry. Layout items are aligned toa grid which represents a basic unit of spacing determined by the <technology file>.
 - Minimum Permissible dimensions of the following parameters in terms of ‘λ’.
 - N-well = 12λ
 - P-diffusion = 3 λ
 - N-diffusion = 3 λ
 - Channel length = 2 λ
 - Width of NMOS = 4 λ
 - Width of PMOS = 8 λ
 - Contact = 4 λ

## 4.5 Steps to make Layout for CMOS Inverter in Magic tool.
 - Open the magic tool by typing “magic” in ubuntu terminal.
 - Select the box in layout window using left and right buttons of mouse with the minimum dimensions of 25λ*23λ (keep device length and widths as per schematic), select the    N-well from the tool bar or type “paint nwell” in console window.

   <img width="1088" height="556" alt="image" src="https://github.com/user-attachments/assets/0ae08fbb-638b-4eb3-9c4e-01ef58026c6f" />
  <p align="center"><b>Fig:- Create Nwell</b></p>
  
   




