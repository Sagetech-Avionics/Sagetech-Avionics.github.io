# Sagetech MXS PX4 Support Driver
This pull request provides support for Sagetech MXS transponders.

## Notes

This code has been tested on a Cubepilot CubeOrange board, and CUAV X7+.

Thank you very much to Ryan Johnston and @dakejahl as well as others in the PX4 Slack for your assistance. Feedback on this pull request would be very appreciated.

## Getting And Building this Code

Clone the Sagetech Avionics fork source code and switch to the branch for this pull request.

```
git clone --recursive https://github.com/Sagetech-Avionics/PX4-Autopilot.git
cd PX4_Autopilot
git switch pr-sagetech-mxs
```

To build the code follow the typical PX4 build instructions for your flight controller.

---

# Using Sagetech MXS Transponder with PX4

![SagetechLogo](https://github.com/Sagetech-Avionics/Sagetech-Avionics.github.io/raw/main/images/sglogo.png)

## Required Hardware

- Sagetech MXS Certified Transponder
- Flight Controller Board (i.e. CubeOrange or CUAV X7+)
- GPS

By default, this guide is assuming that you are connecting the MXS transponder to your flight controller through the MXS COM 1 port via a serial to TTL adapter.

## Example Connection

![mxs_px4_connection](https://github.com/Sagetech-Avionics/Sagetech-Avionics.github.io/raw/main/images/mxs_px4_connection.png)

## Step 1: Configure Serial Parameters

Identify which serial port you will be connecting the MXS to.

- __MXS_SER_PORT__: Select the port on your board that the MXS is connected to. (example: TELEM 2).
- __SER_MXS_BAUD__: Select the baud rate you plan on communicating to the MXS with. By default the baud is 57600.

Reboot of the flight controller is required in order for changes to these settings to take effect.

## Step 2: Configure the MXS

The following parameters allow configuration of the MXS before use.

- __MXS_TARG_PORT__: Select the port on the MXS that is connected to your flight controller (example: COM1).
- __MXS_EXT_CFG__: MXS external configuration. If enabled, MXS must be configured externally through Sagetech Mission Control Software. (see [Manual Configuration](#manual-configuration) section) By default this is disabled, and conducts automatic configuration of the MXS.

Reboot of the flight controller is required in order for changes to these settings to take effect.

## Step 3: Configure ADSB Parameters

The following parameters allow configuration of the ADSB-out information sent from your vehicle identifying it and its capabilities to other vehicles with ADSB capability.

- __ADSB_EMERGC__: Set the emergency type. Default: NoEmergency.
- __ADSB_ICAO_ID__: Set the unique vehicle identification number of your aircraft. If set to 0, a random value will be generated. Transponder must be in OFF mode to set the ICAO.
- __ADSB_EMIT_TYPE__: ADSB classification of vehicle type. Default values is 14 (UAV)
- __ADSB_LEN_WIDTH__: Aircraft length and width dimension options in Length and Width in meters. In most cases, use a value of 1 for smallest size.
- __ADSB_MAX_SPEED__: The maximum speed in knots of the vehicle.
- __ADSB_SQUAWK__: Squawk/Transponder (Mode 3/A) code that is braodcasted to ATC that is usually assigned by your ATC for a given flight. In the USA/Canada the default squawk code is for VFR which is 1200. Most parts of Europe and Australia use 7000. If an invalid octal number is set then it will be reset to 1200.
- __ADSB_LIST_MAX__: The number of vehicles to keep track of. Default: 25.
- __ADSB_ICAO_SPECL__ : Special ICAO ID to attempt to track at all times.
- __ADSB_IDENT__: Set the IDENT bit in ADSB-out messages for 8 seconds.

Reboot of the flight controller is required in order for changes to these settings to take effect.

## Usage

If __MXS_EXT_CFG__ is disabled, if all of the hardware is correctly connected, after step 3, the user can simply open up QGroundControl and see ADSB traffic if there is a valid GPS fix.

### Custom Commands

Custom commands may be sent to the driver during runtime in order to change certain configurations using the following commands:
 - `sagetech_mxs flightid <flightid>` : The user may change the flight ID in flight by providing an 8-character `<flightid>` using this command.
 - `sagetech_mxs opmode <opmode>` : The user can control the transponder operating mode. Valid opmodes include: ['off', 'on', 'stby', 'alt'] or numerical values 0-3. 
 - `sagetech_mxs squawk <squawk>` : The user may set the squawk code to an octal value between 0 and 7777.
 - `sagetech_mxs ident`: This command sends the ident bit for 8 seconds in ADSB-out messages (an alternative to using the parameter).

## Manual Configuration

Manual configuration can be done by enabling __MXS_EXT_CFG__ in step 2.

In this mode the MXS can be configured using Sagetech Mission Control software for a more custom configuration.

1. Follow steps 1 and 2 setting __MXS_EXT_CFG__ to enabled.
2. Disconnect the MXS from the flight controller and connect to the MXS with Mission Control.
3. Set desired settings through the installation configuration panel (i.e. ICAO, emitter category, aircraft size, etc.) in Mission Control.
    > __NOTE:__ Ensure that the baud rate for the relevant COM port matches the __SER_MXS_BAUD__ parameter.
4. Reconnect the MXS to the flight controller and open QGroundControl.
5. Reboot the flight controller.

If all steps were followed the settings set in Mission Control should appear in the Transponder parameter tree.

## Further Documentation
Further Sagetech Documentation and projects can be found on our GitHub. Some useful links can be found below.
- [MXS Host Interface Control Document](https://github.com/Sagetech-Avionics/sagetech-mxs-sdk/blob/main/doc/pdf/ICD02373_MXS_Host_ICD.pdf)