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
  - [4.6 Procedure to extract the layout](#4.6-Procedure-to-extract-the-layout)
  - [4.7 Procedure to do LVS (Layout vs Schematic)](#4.7-Procedure-to-do-LVS-(Layout-vs-Schematic))
- [5. KLayout](#5-KLayout)
  - [5.1 About KLayout](#5.1-About-KLayout)
  - [5.2 Installation and setup](#5.2-Installation-and-setup)
  - [5.3 Install SKY130-pdk and setup to Klayout](#5.3-Install-SKY130-pdk-and-setup-to-Klayout)
  - [5.4 Inverter layout using Pcell.](#5.4-Inverter-layout-using-Pcell.)
  - [5.5 DRC (Design Rule check)](#5.5-DRC-(Design-Rule-check))
  - [5.6 LVS (Layout vs Schematiic)](#5.6-LVS-(Layout-vs-Schematiic))
- [6. BGR introduction](#6-BGR-introduction)
  - [6.1 BGR Principle](##6.1-BGR-Principle)
  - [6.2 Types of BGR](##6.2-Types-of-BGR)
  - [6.3 Self-biased Current Mirror based BGR](##6.3-Self-biased-current-mirror-based-bgr)
- [7. Design and Prelayout Simulation](#7-Design-and-Prelayout-Simulation)
  - [7.1 Design Requirements](##7.1-Design-requirements)
  - [7.2 Device Data sheet](##7.2-Device-data-sheet)
  - [7.3 Circuit Design](##7.3-Circuit-design)
  - [7.4 Wrting spice netlist and prelayout simulation](##7.4-writing-spice-netlist-and-prelayout-simulation)
- [8. Layout Design](#8-Layout-Design)
  - [8.1 Getting started with magic](##8.1-Getting-started-with-magic)
  - [8.2 Basic cell design](##8.2-basic-cell-design)
  - [8.3 Blocks Design](##8.3-blocks-design)
  - [8.4 Top level design](##8.4-top-level-design)
  - [8.5 Layout Vs. Schematic(LVS)](##8.5-Layout-Vs.-Schematic(LVS))
  - [8.6 Parasitic extraction(PEX)](##8.6-Parasitic-extraction(PEX))
  - [8.7 Post-layout Simulation(PLS)](##8.7-Post-layout-simulation(PLS))


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
- Add your pdk path below.
  Ex:- tech load /home/umahe/share/pdk/sky130A/libs.tech/magic/sky130A.teck
       add path /home/umahe/share/pdk/sky130A/libs.ref/sky130_fd_pr/mag
```
tech load $HOME/share/pdk/sky130A/libs.tech/magic/sky130A.tech
add path $HOME/share/pdk/sky130A/libs.ref/sky130_fd_pr/mag
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
- The grid is a square predefined with 'X' dimensions in the Layout window To change the grid dimensions: Go to window option -> Select a Grid Dimension Go to Options and Click on Toolbar The toolbar contains all design essentials necessary for a layout for example nwell, pwell, pdiffusion layer, polysilicon, pdcontact etc.

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
 - Some basic magic tool operations.
```
g : grid on/off
z : zoom in
Shift + z : zoom out

Draw a box : 
  1. Left click + Right click of the mouse : pointer will be at a grid point
  2. Right click : a blank box will be created from the pointed point to the point where right click occured
 
Fill a box with a layer:
  1. Draw a box
  2. Select a layer from the tool manager
  3. Middle click the mouse button
  
  or 
  
  1. Draw a box
  2. Write "paint <layer name>" in the tkcon.tcl window


Delete a layer:
  1. Draw a box where you want to delete a layer
  2. Write "erase <layer name>" in the tkcon.tcl window
 
Delete an area:
  1. Draw a box where you want to delete an area
  2. Press 'a'
  3. Press 'd'

u : undo
r : rotate
m : move
c : copy
```



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
 - Select the box in layout window using left and right buttons of mouse (press left click on one edge and right click on another edge) with the minimum dimensions of 25λ*23λ (keep device length and widths as per schematic), select the N-well from the tool bar or type “paint nwell” in console window.
 - Click on DRC option which is present in top of the layoutwindow.
 - There it will shows the drc errors.
 - If you want to read the DRc msg, then clickon drc option and click on update.
 - There it will display the DRC msg if any DRC error present in the layout.
 - Check the drc continuously after adding new layer.

   <img width="1088" height="556" alt="image" src="https://github.com/user-attachments/assets/0ae08fbb-638b-4eb3-9c4e-01ef58026c6f" />
   
  <p align="center"><b>Fig:- Create Nwell</b></p>
  
 - Select a smaller region within the nwell of 12λ*8λ to make a P=diffusion layer then select the p-diffusion by clicking on scroll-wheel on the mouse or type “paint pdiff” in console window.

   <img width="1094" height="557" alt="image" src="https://github.com/user-attachments/assets/a3d30437-b7a9-4334-8ab3-63fe276b34b4" />

  <p align="center"><b> Fig:- Create p-diffusion inside nwell</b></p>

 - Select area to create polysilicon and select polysilicon by clicking scroll wheel of mouse or type “paint poly” in console window.

   <img width="1090" height="559" alt="image" src="https://github.com/user-attachments/assets/bb02e4da-292f-416b-aa08-b2b9ffabd7a3" />


<p align="center"><b>Fig:- Creating polisilicon</b></p>

 - To make Nmos, select the n-diffusion layer from the toolbar or type “paint ndiff” in console window.
 - Then select polysilicon from the toolbar to make gate terminal or type “paint poly” in console window.

   <img width="1089" height="765" alt="image" src="https://github.com/user-attachments/assets/2a9c1ace-9ad6-4834-90b5-2f965dbd6f4c" />

<p align="center"><b>Fig:- Create NMOS</b></p>


 - Select the area at the diffusion and click on “locali” from the toolbar or type “paint locali” in console window.
 - Repeat the above step for all source and drain terminals.
 - Then select small area inside the locali and select pdcontact for pdiffusion or type “paint pdc” in console window and select ndcontact for ndiffusion or type “paint        ndc” in console window.
   
  <img width="1094" height="915" alt="image" src="https://github.com/user-attachments/assets/87ec28d6-1cc2-41ca-898b-753ec7f55def" />

<p align="center"><b>Fig:- Create local contact to connect base layers to metal (i.e., locali)</b></p>

 - To create Body terminal of the pmos select Ntap from the tool bar or type “paint ntap” in console window. Ntap should be inside the nwell.
 - To create body terminal of the nmos select ptap from the tool bar or type ”paint ptap”.
 - Then draw locali (inter connect) layer on top of the Ntap and Ptap layers by selecting from the toolbar or type “paint li” in console window.
 - Then draw Ptap contact on top of ptap layer by selecting from toolbar or type “paint ptapc” in console window.
 - Draw Ntap contact on top of the Ntap layer or type “paint Ntapc” in console window.

<img width="1442" height="948" alt="image" src="https://github.com/user-attachments/assets/1a8fb8c3-6a15-49cb-a586-c5f0e57dbcf5" />

<p align="center"><b>Fig:- Created body terminals of both transistors</b></p>

  <img width="1448" height="953" alt="image" src="https://github.com/user-attachments/assets/a1311c23-a4a4-449e-9e76-819392dee97d" />

<p align="center"><b>Fig:- Ntap and Ptap with (inter connect) locali layer.</b></p>



 - Connect source of PMOS to the Ntap (vdd) with locali layer by selecting from toolbar or type “paint li” in console window.
 - Connect source of NMOS to the Ptap (gnd) with locali layer by selecting from toolbar or type “paint li” in console window.
 - To make gate contact extend polysilicon on gate then draw locali on top of polysilicon, then draw poly contact on top of locali layer or type “paint polyc” in console       window.
 - Make sure the poly contact should be enclosed with polysilicon.


   <img width="1443" height="947" alt="image" src="https://github.com/user-attachments/assets/1a8e53bb-c580-429f-8592-08902579f9f1" />

<p align="center"><b>Fig:- Created Gate contact and Source terminals are connected to body.</b></p>


 - Select the area on top of body terminals and gate contact then draw metal1 layer by selecting from toolbar or type “paint m1” in console window.
 - Draw locali contact on top of metal1 to connect metal with locali layer or type “paint mcon” in console window.
 - Connect both drain terminals by using metal1 and metal contact.
 - Select area inside the gate contact and type “label input-name” in console window.
 - Repeat the above step for vdd, gnd, and output with their respective names.


   <img width="1445" height="949" alt="image" src="https://github.com/user-attachments/assets/05a8f152-7831-4c94-bff8-90d45ab04b08" />

<p align="center"><b>Fig:- Connect metal1 to the gate and body terminals.</b></p>


   <img width="1448" height="949" alt="image" src="https://github.com/user-attachments/assets/3742433d-1f19-4a53-aafc-3f7c217d8040" />

<p align="center"><b>Fig:- Connected drain terminals with metal1 for output.</b></p>


 - port the labels by using syntax “port label-name make port-direction”.
   **"Ex: port vdd make inout."**
 - Make sure DRC should clean up to here, then layout designing is over.
 - To check LVS or to see design parameters we need to extract the layout.

## 4.6 Procedure to extract the layout:

 - To extract layout type “Extract all” in console window. It will be extracted to “.ext” file.
 - To extract layout parasetics type "-c 1fF" in console window.
 - To convert .ext file to spice net list type “ext2spice lvs”.
 - Then type “ext2spice” it will extract all layers In layout.
 - To check the extracted file go to ubuntu terminal and type “ls”. It shows list of files.

  <img width="990" height="339" alt="image" src="https://github.com/user-attachments/assets/c2c72eb0-93a3-4bea-8ab4-ad7956f47b66" />

 - To open the spice file type “vim file-name.spice”. in ubuntu terminal.
 - The spice file contains device parameters like length, width, parasitics, etc.

## 4.7 Procedure to do LVS (Layout vs Schematic)

 - Save the schematic and click on simulation tab and select lvs then click on lvs netlist + top level is a subckt.

  <img width="1263" height="709" alt="image" src="https://github.com/user-attachments/assets/1e46f68a-aaa4-407a-8998-09c299d6fdf8" />


 - After generating the schematic netlist we need to compare it with layout “.spice” file.
 - For that we need to add both file paths in the terminal.
 - The syntax is netgen lvs “ .xschem (schematic) path” “ .spice (layout) path” pdk file path.
   **Ex:- netgen lvs "/home/umahe/.xschem/simulations/inv.spice inv" "/home/umahe/inv.spice inv"/home/umahe/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl**
 - This will check both schematic netlist and layout spice file and produce the output.
 - If layout matches with the schematic it will show “Circuit matches uniquely.”

  <img width="677" height="409" alt="image" src="https://github.com/user-attachments/assets/84165cc1-b614-4c80-941c-a0ca6693751f" />


<p align="center"><b>Fig:- LVS Result window.</b></p>

 - If both are not matched, change the parameters according to the schematic and check the lvs again.
 - This ends the Layout design when both lvs and drc clean.


# 5. KLayout
## 5.1 About KLayout

 <img width="225" height="225" alt="image" src="https://github.com/user-attachments/assets/3a4f11d2-8599-4cd0-a793-7018a4ebb0d6" />

[KLayout](https://www.klayout.de/doc/manual/basic.html) KLayout itself is a layout viewer and editor primarily used for integrated circuit design, and it does not directly produce or use HTML documents for its core functionality of layout editing.

[KLayout reference file](https://www.klayout.de/doc/manual/index.html) Complete manual for reference.

## 5.2 Installation and setup
 - Type **Klayout** in web browser.
 - Click on **Get Klayout** which is present at the top of window.
   <img width="1695" height="758" alt="Screenshot 2025-10-28 122537" src="https://github.com/user-attachments/assets/fbf1208f-1167-4c27-bdf8-5b9b0cf069a7" />

 - Select updated ubuntu version of klayout in below list and download.
 - Open Ubuntu terminal and use the commands to install Klayout.

```
  sudo apt update
```
 - Then

```
  sudo apt install klayout
```

 - After Installing klayout Install python file also.

```
  
  sudo apt install python3-pip
```
   
**Important:** The pymacros for Sky130 are currently not compatible with the latest version of gdsfactory. The solution is to install an earlier version (before `gdsfactory.typings` change). But there is another problem: installing an older version of gdsfactory also installs the latest possible versions of the dependencies. This is a problem for `pydantic` because it wasn't pinned to a 1.x version and installing a 2.x version will break gdsfactory.

 - Therefore, the solutions is to install pydantic<2 first:

   ```
   pip3 install pydantic==1.10
   ```

   - And afterwards gdsfactory:

  ```
  pip3 install gdsfactory==6.33.0
   
  ```
 - **Note:** If you are using a Linux distribution that discourages the installation of system-wide Python packages through pip, you need to pass `--break-system-packages`.


## 5.3 Install SKY130-pdk and setup to Klayout

 - Type `klayout -e` in Ubuntu terminal to open klayout in editor window.
 - Click on **tools** option, then select **manage packages**.
 - Select package and give right click on your mouse.
 - Click on mark all and click on Apply then ok.
 - After Installing all packages close the packages window then click settings icon in editor window and select  `SKY130` or `SKY130A-el`

   <img width="1379" height="942" alt="Screenshot 2025-10-28 145905" src="https://github.com/user-attachments/assets/02f03f90-6dd3-43d6-a475-667ce4200370" />

 - open new layout from the file menu.
 - It will open layout editor window with layers present in sky130 pdk.


## 5.4 Inverter layout using Pcell.
  
 - Open new layout editor window from the file menu.
 - Give **Top cell** name and **Initial layer** name then click on ok.
 - To take Pcells (NMOS and PMOS) click on **instance** icon on top of the window.
 - Select SKY130-pdk library and click on search icon.
 - It will open new window, in that select **pfet** device and click ok.

   <img width="1918" height="1077" alt="Screenshot 2025-10-28 152408" src="https://github.com/user-attachments/assets/1b2b85a1-e6aa-4694-9dfb-56ee6bb19847" />

 - For NMOS select **nfet** decive and click ok.

   <img width="1918" height="1077" alt="Screenshot 2025-10-28 152450" src="https://github.com/user-attachments/assets/5d526aff-6a6e-437c-b7ae-0d9575c2da1a" />


 - For body terminal of the Pmos select **licon_difftap$T$nsdm_li$T** and click ok.

   <img width="1919" height="1079" alt="Screenshot 2025-10-28 152624" src="https://github.com/user-attachments/assets/dbc15fd5-2bd8-45f2-845f-1cb78202d2e3" />

 - For body terminal of the nmos select **licon_difftap$T$psdm_li$T** and click ok.

   <img width="1914" height="1077" alt="Screenshot 2025-10-28 152602" src="https://github.com/user-attachments/assets/bb805a4d-f983-4616-9b84-ac350a14d7c8" />

 - Change the device parameters According to the schematic.
 - To change parameters (length and width) select device and press **'Q'**, then change the values.
 - Repeat the procedure for all devices.
 - Then select **met1(m1)** from the layers and click on **path** icon.
 - Connect the all terminals with metal as per the schematic.
 - 

   <img width="1919" height="1075" alt="Screenshot 2025-10-28 160319" src="https://github.com/user-attachments/assets/6ab857d5-1547-4c54-a107-a81c3d24651e" />

<p align="center"><b> Fig:- Inverter Layout</b></p>



## 5.5 DRC (Design Rule check):


 - Click on **Tools** option, then **SKY130**, then click on DRC.
 - It will open the DRC result window, at right side of the window we can see the errors present in our layout design.
 - Make sure clean all DRC errors, if any occurs.
 - To highlight DRC in layout, select error and clickon 

   <img width="1917" height="1066" alt="Screenshot 2025-10-30 093818" src="https://github.com/user-attachments/assets/ca8644ef-130f-46c6-964a-7021dc6ae00c" />

   <p alighn="center"><b> Fig:- DRC result </b></p>



 ## 5.6 LVS (Layout vs Schematiic):

 - To check lvs we need **schematic .spice** file and **layout gds** file.
 - The procedure to create schematic .spice file is mentioned above.
 - Make sure the schematic .spice file and layout gds file is in same path.
 - Then click on tools in klayout window, select sky130 and click on lvs.
 - It will compare both files and proce the result window.

   <img width="921" height="530" alt="Screenshot 2025-10-28 124655 - Copy" src="https://github.com/user-attachments/assets/5897ac2a-9b8f-4b40-b408-06a854864862" />
 
   <p align="center"><b> Fig:- LVS Result </b></p>

 - Clear the errors in layout if any LVS error occured, and run again.




# 6 BGR_sky130
This github repository is for the design of a Band Gap Reference Circuit (BGR) using Google-skywater130 PDK.

<img width="560" height="218" alt="image" src="https://github.com/user-attachments/assets/e1816764-3036-45f0-bf62-43ff0d1719ab" />



### Why BGR 
- A battery is unsuitable for use as a reference voltage source.
  - voltage drops over time
- A typical power supply is also not suitable 
  - noisy output and/or residual ripple.
- A voltage reference IC used buried Zener diode, 
  - Discrete design required additional components and high frequency filtering circuits due to higher thermal noise.
  - Low voltage Zener  diode is not available

**Solution**
- A Bangap reference which can be integrated in bulk CMOS, Bi-CMOS or Bipolar technologies without the use of  external components.

### Features of BGR
- Temp. independent voltage reference circuit widely used in Integrated Circuits
- Produces constant voltage regardless of power supply variation, temp. Changes and circuit loading
- Output voltage of 1.2v (close to the band gap energy of silicon at 0 deg kelvin)
- All applications starting from analog, digital, mixed mode, RF and system-on-chip (SoC).

### Applications of BGR
- Low dropout regulators (LDO)
- DC-to-DC buck converters
- Analog-to-Digital Converter (ADC)
- Digital-to-Analog Converter (DAC)

## 6.1 BGR Introduction

### 6.1.1 BGR Principle
The operation principle of BGR circuits is to sum a voltage with negative temprature coefficient with another one exhibiting opposite temperature dependancies. Generally semiconductor diode behave as CTAT i.e. Complement to absolute temp. which means with increase in temp. the voltage across the diode will decrease. So we need to find a PTAT circuit which can cancel out the CTAT nature i.e. with rise in temp. the voltage across that device will increase and thus we can get a constant voltage reference with respect to temp.

<img width="749" height="455" alt="image" src="https://github.com/user-attachments/assets/8e88e1fd-402c-42f2-b21e-036523d4e256" />

#### 6.1.2 CTAT Voltage Generation
Usually semiconductor diodes shows CTAT behaviour. If we consider constant current is flowing through a forwrard biased diode, then with increase in temp. we can observe that the voltage across the diode is decreaseing. Generally, it is found that the slope of the V~Temp is -2mV/deg Centigarde.

<img width="668" height="249" alt="image" src="https://github.com/user-attachments/assets/5c1aff86-503c-4519-b8d5-7f0ced92a97c" />

#### 6.1.3 PTAT Voltage Generation
<img width="263" height="505" alt="image" src="https://github.com/user-attachments/assets/2ddca0c0-d3a0-43bd-9a05-989cc30b5f30" />


From Diode current equation we can find that it has two parts, i.e. 

- Vt (Thermal Voltage) which is directly proportional to the temp. (order ~ 1)
- Is (Reverse saturation current) which is directly proportional to the temp. (order ~ 2.5), as this Is term is in denominator so with increase in temp. the ln(Io/Is) decreases which is responsible for CTAT nature of the diode.

So to get a PTAT Voltage generation circuit we have to find some way such that we can get the Vt separated from Is.

To get Vt separated from Is we can approach in the following way

<img width="364" height="331" alt="image" src="https://github.com/user-attachments/assets/53087095-55e5-49ce-9cbd-d11a38de5345" />



In the above circuit same amount of current I is flowing in both the branches. So the node voltage A and B are going to be same V. Now in the B branch if we substract V1 from V, we get Vt independent of Is.

<img width="514" height="315" alt="image" src="https://github.com/user-attachments/assets/9f6f7763-b5f9-4cb3-9583-6c59b2133765" />

Now

```
V= Combined Voltage across R1 and Q2 (CTAT in nature but less sloppy)
V1= Voltage across Q2 (CTAT in nature but more sloppy)
V-V1= Voltage across R1 (PTAT in nature)
```
From above we can see that the voltage V-V1 is PTAT in nature, but it's slope is very less as compared to the CTAT, so we have to increase the slope. In order to increase the slope we can use multiple BJTs as diode, so that current per individual diode will be less and it the slope of V-V1 will increase.

<img width="804" height="405" alt="image" src="https://github.com/user-attachments/assets/fde752ca-5726-4eec-bcd1-71c593247fb0" />


### 6.2 Types of BGR
Architecture wise BGR can be designed in two ways

- Using Self-biased current mirror  
- Using Operational-amplifier 

Application wise BGR can be categorized as
- Low-voltage BGR
- Low-power BGR
- High-PSRR and low-noise BGR
- Curvature compensated BGR

We are going to design our BGR circuit using Self-biased current mirror architecture.

### 6.3 Self-biased current mirror based BGR

The Self-biased current mirror based constitute of the following components.

- CTAT voltage generation circuit
- PTAT voltage generation circuit
- Self-biased current mirror circuit
- Reference branch circuit
- Start-up circuit

#### 6.3.1 CTAT Voltage generation circuit
The CTAT Voltage generation circuit consist of a BJT connected as a diode, which shows CTAT nature as explained above.

<img width="183" height="243" alt="image" src="https://github.com/user-attachments/assets/01247cbb-df21-4afe-bdad-8c0f26ee2a1c" />


#### 6.3.2 PTAT Voltage generation circuit
The PTAT Voltgae generation circuit consist of **N** BJTs connected with a series resistance. The operation principle is explained above.

<img width="393" height="399" alt="image" src="https://github.com/user-attachments/assets/75354812-fb9c-4723-b903-b7df2b07964c" />


#### 6.3.3 Self-Biased Current Mirror Circuit
The Self-biased current mirror is a type of current mirror which requires no external biasing. This current mirrors biases it self to the desired current value without any external current source reference. 

<img width="358" height="301" alt="image" src="https://github.com/user-attachments/assets/2550ca8a-416c-4c19-9282-92743d7d1ec4" />


#### 6.3.4 Reference Branch Circuit
The reference circuit branch performs the addition of CTAT and PTAT volages and gives the final reference voltage. We are using a mirror transitor and a BJT as diode in the reference branch. By virtue of the mirror transistor in the reference branch the same amount of current flows through it as of the current mirror branches. Now from the PTAT circuit branch we are getting PTAT voltage and PTAT current. The same PTAT current is flowing in the reference branch. But the slope of PTAT voltage is much more smaller than that of slope of CTAT voltgae. In order to make increase the voltage slope we have to increase the resistance (current constant, so V increases with increase in R). Now across the high resistance we will get our constant reference voltage which is the result of CTAT Voltage + PTAT Voltage.

<img width="137" height="386" alt="image" src="https://github.com/user-attachments/assets/87616d6e-7386-4aed-84f0-b098042b9b48" />


#### 6.3.5 Start-up circuit
The start-up circuit is required to move out the self biased current mirror from degenerative bias point (zero current). The start-up circuit forecefully flows a slow amount of current through the self-biased current mirror when the current is 0 in the current mirror branches, as the current mirror is self biased this small current creats a disturbance and the current mirror auto biased to the desired current value.

<img width="521" height="434" alt="image" src="https://github.com/user-attachments/assets/c335ea81-cd0d-4d67-af54-38659da36a5c" />


#### 6.3.6 Complete BGR Circuit
Now by connecting all above components we can get the complete BGR circuit.

<img width="751" height="490" alt="image" src="https://github.com/user-attachments/assets/3e45cb87-2ade-4779-b054-02e792d8567a" />

Advantages of SBCM BGR:

- Simplest topology
- Easy to design 
- Always stable

Limitations of SBCM BGR:

- Low power supply rejection ratio (PSRR)
- Cacode design needed to reduce PSRR
- Voltage head-room issue
- Need start-up circuit

## 7. Design and Pre-layout Simulation
For the real-time circuit design we are going to use sky130 technology PDK. Before we design the complete circuit we must know what are our design requirements. The design requirements are the design guidelines which our design must satisfy.

### 7.1 Design Requirements
- Supply voltage = 1.8V
- Temperature: -40 to 125 Deg Cent.
- Power Consumption < 60uW
- Off current < 2uA
- Start-up time < 2us
- Tempco. Of Vref < 50 ppm

Now, we have to go through the device data sheet to find the appropriate devices for our design. 

After thoroughly going through the device data sheet we selected the following devices for our design.
### 7.2 Device Data Sheet
***1. MOSFET***
| Parameter | NFET | PFET |
| :-: | :-: | :-: |
| **Type** | LVT | LVT |
| **Voltage** | 1.8V | 1.8V |
| **Vt0** | ~0.4V | ~-0.6V |
| **Model** | sky130_fd_pr__nfet_01v8_lvt | sky130_fd_pr__pfet_01v8_lvt |

***2. BJT (PNP)***
| Parameter | PNP | 
| :-: | :-: | 
| **Current Rating** | 1uA-10uA/um2 | 
| **Beta** | ~12 |
| **Vt0** | 11.56 um2 | 
| **Model** | sky130_fd_pr__pnp_05v5_W3p40L3p40 |

***3. RESISTOR (RPOLYH)***
| Parameter | RPOLYH | 
| :-: | :-: | 
| **Sheet Resistance** | ~350 Ohm | 
| **Tempco.** | 2.5 Ohm/Deg Cent |
| **Bin Width** | 0.35u, 0.69u, 1.41u, 5.37u | 
| **Model** | sky130_fd_pr__res_high_po |

### 7.3 Circuit Design

**1. Current Calculation**

- Max. power Consumption < 60uW
- Max Total Current = 60 uW/1.8V=33.33uA
- So, we have chosen 10uA/branch, (3*10=30uA)
- Start-up current 1-2 uA

**2. Choosing Number of BJT in parallel in Branch2**
- Less number of BJT: require less resistance value but matching hampers
- More number of BJT: requires higher resistance value but gives good matching
- So a moderate number have chosen (8 BJT) for better layout matching and moderate resistance value.  

**3. Calculation of R1**
- R1= Vt* ln (8)/I =26 mv *ln(8)/10.7uA=5 KOhm
- R1 size: W=1.41um, L=7.8um, Unit res value: 2k Ohm
- Number of resistance needed: 2 in series and 2 in parallel (2+2+(2||2))

**4. Calculation of R2**
- Current through ref branch:I3=I2=Vt*ln(8)/R1
- Voltage across R2: R2*I3=R2/R1(Vt*ln(8))
- Slope of VR2= R2/R1 (ln(8)*115uv)/Deg Cent.
- Slope of VQ3=-1.6mV/Deg cent
- Adding both and equating to zero, R2 will be around 33k Ohm
- Number of resistance needed: 16 in series and 2 in parallel (2+2...+2+ (2||2))

**5. SBCM Design (Self-biased Current Mirror)**

***A. PMOS Design in SBCM***
- Make both the MP1 and MP2 well in Saturation 
- To reduce channel length modulation used L=2um
- Finally the size is **L=2u, W=5u and M=4**

***B. NMOS Design in SBCM***
- Make both the MN1 and MN2 either in Saturation or in deep sub-threshold
- We have made it in deep sub-threshold 
- To reduce channel length modulation used L=1um
- Finally the size is **L=1u, W=5u and M=8**

#### 7.3.1 Final Circuit

<img width="822" height="518" alt="image" src="https://github.com/user-attachments/assets/9e62faab-83b9-438a-bac4-1207423307fa" />


### 7.4 Writing Spice netlist and Pre-layout simulation
As we are not using any schematic editor we have to write the spice netlist and simulate it using Ngspice.

**Steps to write a netlist**
- Create a file with ***.sp*** extension, open with any editor like gvim/vim/nano.
- The 1st line of the Spice netlist is by default a comment line.
- To write a valid netlist we must include the library file (with absolute path) and mention the cornmer name (tt, ff or ss). Ex- 
```
.lib "/home/<path-to-lib>/sky130.lib.spice" tt
```
- Now, if we are using the **sky130_fd_pr__pnp_05v5_W3p40L3p40** model, we have to include the that file also.
```
.include "/home/<path-to-model>/sky130_fd_pr__model__pnp.model.spice"
```
- Syntax for independent voltage/current source is:
```
Vxx n1 n2 dc 1.8 : *Vxx* - Voltage source, *n1* - Node-1 of voltage source, *n2* - Node-2 of voltage source, *dc* - Type (can be dc/ac) *1.8* - value
Ixx n1 n2 dc 10u : *Ixx* - Current source, *n1* - Node-1 of current source, *n2* - Node-2 of current source, *dc* - Type (can be dc/ac) *1.8* - value
```
- Syntax for DC simulation
```
.dc temp -40 125 5 : Simulate for temp varying from -40 to 125 with 5 dec cent step
.dc Vs1 0 1.8 0.01 : Simulate for Vs1 varying from 0V to 1.8V with 0.01V step
```


## 4. Layout Design

Now after getting our final netlist, we have to design the layout for our BGR. Layout is drawning the masks used in fabrication. We are going to use the Magic VLSI tool for our layout design.

### 4.1 Getting started with Magic

Magic is an open source VLSI layout editor tool. To launch magic open terminal and write the following command.
```
$ magic -T /home/<path for sky130A.tech>/sky130A.tech 
```

Now it will open up two windows, those are `tkcon.tcl` and `toplevel`. Now let's discuss some basic magic tool operations.
```
g : grid on/off
z : zoom in
Shift + z : zoom out

Draw a box : 
  1. Left click + Right click of the mouse : pointer will be at a grid point
  2. Right click : a blank box will be created from the pointed point to the point where right click occured
 
Fill a box with a layer:
  1. Draw a box
  2. Select a layer from the tool manager
  3. Middle click the mouse button
  
  or 
  
  1. Draw a box
  2. Write "paint <layer name>" in the tkcon.tcl window


Delete a layer:
  1. Draw a box where you want to delete a layer
  2. Write "erase <layer name>" in the tkcon.tcl window
 
Delete an area:
  1. Draw a box where you want to delete an area
  2. Press 'a'
  3. Press 'd'

u : undo
r : rotate
m : move
c : copy
```
Now device wise we have the following devices in our circuit.
- PFETS
- NFETS
- Resistor Bank
- BJTs

Now in order to design faster we should follow the hierarchical design manner. i.e we will design one cell then we will instance that to another level and do placement and routing.

In our design we have 3 hierarchies. Those are 

1. Hierarchy-1 (Basic Cells) : NFET, PFET, BJT, Resistor
2. Hierarchy-2 (Blocks of similar cells): NFETS, PFETS, PNP10, RESBANK, STARTERNFET
3. Hierarchy-3 (Top Level): TOP

Now let's start with all leaf cell designs.
### 4.2 Basic Cell Design

#### 4.2.1 Design of NFET
In our circuit we are using LVT type NFETs. So we have to draw all the valid layers for the lvt nfet as per our desired sizes.

In our design we have used two different size nfets:

1. W=5 L=1 [mag file](/BGR_Layout/Nfet.mag)

<p align="center">
 <img width="267" height="657" alt="image" src="https://github.com/user-attachments/assets/f0b183ac-74a2-400a-b062-56bde238d517" />

</p>

2. W=1 L=7 [mag file](/layout/nfet1.mag)

<p align="center">
  <img width="815" height="228" alt="image" src="https://github.com/user-attachments/assets/ac5008eb-25d5-45b5-956b-637f288d4b4c" />

</p>

#### 4.2.2 Design of PFET
In our circuit we are using LVT type PFETs. So we have to draw our PFET using all valid layers for lvt pfet. In our design we have one size pfet i.e W=5 L=2 [mag file](/layout/pfet.mag)

<p align="center">
  <img width="351" height="619" alt="image" src="https://github.com/user-attachments/assets/206b4466-b419-493c-b147-275b40979b21" />

</p>

#### 4.2.3 Design of Resistor
In our desing we are using poly resistors of W=1.41 and L=7.8. So we have to create the magic file choosing the appropriate layers for the Resistor. [mag file](/layout/res1p41.mag)

<p align="center">
 <img width="134" height="635" alt="image" src="https://github.com/user-attachments/assets/6889fe4a-2023-40c3-954e-46c2082960d0" />

</p>

#### 4.2.4 Dessing of PNP (BJT)
In our design we are using PNP having emitter 3.41 * 3.41 uM.So we can use the valid layers to design our PNP. [mag file](/layout/pnpt1.mag)

<p align="center">
 <img width="624" height="621" alt="image" src="https://github.com/user-attachments/assets/11ca2a12-cbdb-411f-b039-19f202e66ee2" />

</p>

### 4.3 Blocks Design

#### 4.3.1 Design of NFETs
We have created a layout by putting all the nfets in one region. We have placed the nfets in such a way that it follows common centroid matching. Also used some dummies to avoid Diffusion break and for better matching and noise protection. Also added one guard ring for enhance performance. [mag file](/layout/nfets.mag)

<p align="center">
  <img width="701" height="623" alt="image" src="https://github.com/user-attachments/assets/c1f95650-ca40-4fbc-b9fd-127e7cc53553" />

</p>

#### 4.3.2 Design of PFETs
We have created a PFETs block by putting all the pfets together, with matching arrangement, also added the guardring. [mag file](/layout/pfets.mag)

<p align="center">
  <img width="754" height="234" alt="image" src="https://github.com/user-attachments/assets/fd1b5e6b-8b73-44c2-9460-ecbff01a6184" />

</p>

#### 4.3.3 Design of RESBANK
We have cretaed the layout of the RESBANK by putting all resistors together, with proper matching arrangemment and soe extra dummies and a guardring. [mag file](/layout/resbank.mag)
<p align="center">
 <img width="648" height="619" alt="image" src="https://github.com/user-attachments/assets/fe9d7075-4399-48b0-b969-390d449a6678" />

</p>

#### 4.3.4 Design of PNP10
We have created the layout by putting all the PNPs together, with appropriate matching, and used dummies to enhance noise performance. [mag file](/layout/pnp10.mag)
<p align="center">
  <img width="752" height="446" alt="image" src="https://github.com/user-attachments/assets/98951ad7-58a1-487e-94f4-57a2e91701f3" />

</p>

#### 4.3.5 Design of STARTERNFET
We placed the the two w=1, l=7 NFETs together with a guardring to desingn the STATRTERNFET. [mag file](/layout/starternfet.mag)
<p align="center">
<img width="771" height="249" alt="image" src="https://github.com/user-attachments/assets/f2d2ad8f-1915-4748-8740-bf67410b412b" />

</p>

### 4.4 Top level design
To obtain the top level design, we have placed all the blocks together, routed it. [mag file](/layout/top.mag)
<p align="center">
  <img src="Images/layout/top.png">
</p>

## 5. LVS and Post-layout simulaton

### 5.1 LVS

LVS stands for Layout vs. Schematic which essentially means to compare the Layout extrated netlist with the Designed spice netlist.

In this project we are going to use Netgen to perform the LVS. 

To start with netgen we can type the command ```netgen``` on the terminal. It will open up the netgen window.

[Magic]:                http://opencircuitdesign.com/magic/
[Ngspice]:              http://ngspice.sourceforge.net
[Netgen]:               http://opencircuitdesign.com/netgen/
[NGSpiceMan]:           http://ngspice.sourceforge.net/docs/ngspice-html-manual/manual.xhtml















































