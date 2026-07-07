
---
title: "PNSN Aircell Switch"
draft: false
tags:
  - 
---
Check engineering Notebook on PNSN Drive


## Current Objectives

1. Finalize Schematic/Design
    
2. Ltspice Simulation
    
3. Breadboarding
	
4. Find Enclosure for new circuit (Could it use SP enclosure?)
    
5. Build & Manufacture new circuit
    


## Open Tasks

- [ ] **Schematic Design:**
	- [ ] Set a valid UV-OV range for Aircell and MPPT
		- [x] MPPT Range = 30V-6V
		- [x] Aircell Range = 30V-6V 
		- [ ] Hysteresis calculations (Probably not needed. Breadboarding test)
		- [ ] OV & UV Filtering (Probably not needed. Breadboarding test)
	- [x] Select P-Channel MOSFETS
		- [ ] SH8J62 will burn up don't use
		- [x] Alternative of [IPB120P04P4L03ATMA2](https://www.digikey.com/en/products/detail/infineon-technologies/IPB120P04P4L03ATMA2/11657627) found
		- [ ] Perhaps through-hole alternative that doesn't cost $20 per device
	- [ ] Inrush Current & Input voltage droop additions
		- [ ] determine if IDM of PMOS is too low and we need Current limiting circuit
			- [ ] If so, put schotkky network on one or both input lines
	- [ ] Select Vout Capacitor (to prevent voltage sag)
	- [ ] Determine if transient protection is needed
	- [ ] Add input and output short protections
	- [x] Configure for dual supply operation
		- [ ] Should we include monitoring network? (use 3rd set as a "flag" for valid operation)
	- [ ] Add external EN and SHDN controls
## Documentation
**What are we designing?**
		Aircells should be floating/disconnected except when LVD (MPPT Load Voltage = 0V) occurs 
		

**What should the general structure of this load switch look like?**
	As abstract and high level as possible this switching structure should look like:
		AGM/SLA Battery -> MPPT -> ==Aircell Switch== -> Power Out  
		                                                       ^  
		                                                Aircell Bank

**How should we build this switch?**
	Using the LTC4417 IC in conjunction with power PMOS
	
**LTC4417 Notes:**
Predefined hysterisis (switching threshold) is 30mV when grounded. Should we consider smth? higher? Should we add a filtering cap instead?

Mosfet breaksdown at -30V
Mosfet max current at 4.5A

Calculating Inrush currents:
When will the aircell be switching?
	Turning off:
		Aircell voltage: Unknown
		MPPT Voltage: LVD (11V)
	Turning on:
		Aircell voltage: Unknown
		MPPT Voltage: LVR (12.1V)
	

Inrush occurs when switching low to high voltage.

Capacitor on Aircell Output line is needed. Why? Switching from say 12V AGM to 15V Aircell results in a current surge due to charging thinks like voltage regulator and radio internal capacitors to higher voltage level. Guides shown on page 16 of datasheet. We will probably be fine with a simple cap over diode + cap + resistor circuit as we know the exact points at whcih voltages switch.

Steps:

Design Surge current (TVS protection). Add filtering caps? 

Disconnect inrush current circuit. If current draw goes above 3A, then we design inrush protection circuit. Otherwise, ignore.


Transient voltage suppersion not needed. We are dealing with voltages lower than the max ratings of pmos and also short and low frequency signals

TVS diode (bidirectional) on input and (unidirectional) on output

**For dual supply operation:**
Ground V3, OV3, UV3, VS3 and G3 pins of the unused channel. However, if you want to be able to create a separate pin to act as flag for ov/uv region (I think its 5V if within an OV/UV region and GND if outside of all regions).


**Sandpoint for Mickey:**
x2 BB_box (wall mounted box for storing pref. 2 batteries)= 1
x1 Prostar power panel
x3 900 MHz Yagi antennas (Larger than 6 inches, look for high gain but don't grab the huge ones)
video and photo documentation
x4 sections of aluminum angle (for solar panels)
50' roll of 1 1/4" flex conduit.

## Meeting History
Meet with Ben every Monday?