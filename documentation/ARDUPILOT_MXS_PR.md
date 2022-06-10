# Introduction
This pull request provides support for Sagetech MXS transponders.

## Getting And Building this Code
Clone the Sagetech Avionics fork source code and switch to the branch for this pull request.
```
git clone --recursive https://github.com/Sagetech-Avionics/ardupilot.git
cd ardupilot
git switch pr/sagetech_MXS
```

From there the typical [Ardupilot instructions for building from source](https://github.com/ArduPilot/ardupilot/blob/master/BUILD.md) can be followed, starting from the "Basic Usage" section.

## Notes

ADSB_TYPE must be set to "4" in order for MXS parameters to appear. 

This code has been tested on a CubeOrange and CUAV-X7+ with an MXS connected through Telem2.

Thank you very much to @magicrub for your help in submitting this pull request and getting me acquainted with the Ardupilot code base. I'd appreciate any feedback to improve this code.

What follows is a very initial attempt at documentation of the functionality.

---
![SagetechLogo](https://github.com/Sagetech-Avionics/Sagetech-Avionics.github.io/raw/main/images/sglogo.png)
# Sagetech MXS Transponder with Ardupilot

## Required Hardware
- Sagetech MXS Certified Transponder
- Autopilot Board (i.e. CubeOrange, CUAV-X7+)
- GPS

## Example Connection
![mxs_ardupilot_connection](https://github.com/Sagetech-Avionics/Sagetech-Avionics.github.io/raw/main/images/mxs_ardupilot_connection.png)

By default, this guide is assuming that you are connecting the MXS transponder to the autopilot board through the MXS COM1 port via a RS232 to TTL adapter. Note that the MXS COM ports can be configured to use RS232 or RS422 using a connection harness or control box. Use the appropriate configuration and cable.

## Step 1: Configure Serial Parameters
Identify which serial port you will be connecting the MXS to. Configure the serial parameters as follows where 'x' refers to the serial port number (i.e. 2 for telem2):
- __SERIALx_PROTOCOL__: Change the SERIALx_PROTOCOL parameter to 35 for ADSB.
- __SERIALx_BAUD__: Change the SERIALx_BAUD parameter to 57 for for a 57600 baud rate.
- __SRx_ADSB__: Change the SRx_ADSB parameter to 2 for a 2Hz streaming rate.

## Step 2: Configure ADSB Parameters
- __ADSB_TYPE__: Set the ADSB_TYPE parameter to "4" for Sagetech MXS.
- __ADSB_ICAO_ID__: Set the unique vehicle identification number of your aircraft. If set to 0, a random value will be generated. Transponder must be in OFF mode to set the ICAO.
- __ADSB_EMIT_TYPE__: ADSB classification of vehicle type. Default values is 14 (UAV)
- __ADSB_LEN_WIDTH__: Aircraft length and width dimension options in Length and Width in meters. In most cases, use a value of 1 for smallest size.
- __ADSB_RF_SELECT__: Set ADSB_RF_SELECT to "3" for Tx and Rx capabilities. Set to "0" to put the MXS in OFF mode.
- __ADSB_SQUAWK__: Squawk/Transponder (Mode 3/A) code that is broadcasted to ATC that is usually assigned by your ATC for a given flight. In the USA/Canada the default squawk code is for VFR which is 1200. Most parts of Europe and Australia use 7000. If an invalid octal number is set then it will be reset to 1200.
- __ADSB_LIST_MAX__: Limits the number of vehicles which will be stored. Set to 0 to disable limit.
- __ADSB_RADIUS_MAX__: Limits the radius in meters around the aircraft that vehicles will be detected and stored. Set to 0 to disable limit.
- __ADSB_OPTION__ : Setting bit 3 disables MXS automatic configuration and assumes manual configuration of the MXS. 

## Optional: Manual Configuration
More detailed manual configuration can be done through the Sagetech Mission Control software prior to connecting the autopilot for more in-depth control.
1. Start with the Autopilot disconnected from the MXS.
2. Connect to the MXS through COM 0 with baud rate 230400 with Mission Control.
3. Manually set the MX COM 1 port baud rate to 57600.
4. Set any other desired settings through the installation configuration panel (i.e. ICAO, emitter category, aircraft size, etc.) and send the configuration.
5. Power on Autopilot and connect to it with Mission Planner (or another GCS)
6. Configure the Autopilot ADSB settings
    - Ensure that ADSB_TYPE is set to "4"
    - Set ADSB_RF_SELECT to "0" to keep the MXS in OFF mode
    - Ensure that ADSB_OPTION is set to "8" (or set bit 3) to enable manual configuration
7. Connect the Autopilot to the MXS through its COM 1 port.
8. Power cycle the Autopilot, and connect to it with Mission Planner.
The MXS should be configured correctly, and the correct settings should appear in the ADSB parameter tree.

Note: When changing MXS ICAO, the MXS needs to be in OFF mode. This means that ADSB_RF_SELECT must be set to "0".

## Further Documentation
Further Sagetech Documentation and projects can be found on our GitHub. Some useful links can be found below.
- [MXS Host Interface Control Document](https://github.com/Sagetech-Avionics/sagetech-mxs-sdk/blob/main/doc/pdf/ICD02373_MXS_Host_ICD.pdf)