## Smart Injection Molding Cell – TwinCAT 3

Industrial automation portfolio project implementing the PLC control,
digital twin, diagnostics, interlocks and machine-vision interface of an
injection-molding cell using Beckhoff TwinCAT 3 and IEC 61131-3
Structured Text.

## Project Status

### PLC Core
- Machine operating modes
- Automatic startup sequence
- Automatic injection-molding cycle
- Graceful production stop
- Controlled shutdown
- Extruder control
- Mold control
- Barrel temperature PID control
- Analog signal scaling
- Process and motion interlocks
- Emergency-stop simulation
- Automatic hopper filling
- Hopper timeout diagnostics
- Alarm management
- Plant / digital-twin simulation
- Fault injection for commissioning

### Quality Control
- Generic industrial vision handshake
- Ready / Trigger / Busy / Result / Acknowledge
- PASS / FAIL classification
- Defect-code handling
- Vision offline detection
- Vision timeout handling
- Good / rejected part counters
- Reject-rate calculation

## Architecture

```text
                    Operator / HMI
                         |
                         v
                  FB_Machine
                         |
                         v
              FB_InjectionSequence
                 /       |       \
                /        |        \
               v         v         v
        FB_Extruder   FB_Mold   FB_Vision
               \         |         /
                \        |        /
                 v       v       v
                   FB_Interlocks
                         |
                         v
                  Physical Outputs
                         |
                         v
                FB_PlantSimulation

Additional subsystems:
- FB_TemperatureControl
- FB_Hopper
- FB_AlarmManager
- Analog input processing
- Production statistics


## Machine States

INITIAL
   |
   v
STARTUP
   |
   v
STOPPED
   |
   v
RUN
   |
   +------> STOPPED
   |
   v
SHUTDOWN
   |
   v
INITIAL

Emergency Stop can transition the machine to MODE_ESTOP.

## Production Sequence

INJECT
  |
  v
COOLING
  |
  v
DOSING
  |
  v
OPEN MOLD
  |
  v
EJECT
  |
  v
VISION TRIGGER
  |
  v
VISION WAIT
  |
  v
SORT PART
  |
  v
CLOSE MOLD
  |
  v
COMPLETE

## Digital Twin

The project contains a software plant simulation so the controller can
be developed and tested without a physical injection-molding machine.

The simulation models:

Screw position
Mold position
Hopper level
Barrel temperature
Mold temperature
Injection pressure
End-position sensors
Analog sensor signals
Hopper level switches

Fault-injection variables are provided for commissioning and validation.

##Vision Interface

The current PLC implementation provides a generic machine-vision
interface independent of a specific camera manufacturer.

The interface supports:

Camera ready
Inspection trigger
Busy status
Result valid
Part present
PASS / FAIL result
Defect code
Width
Height
Angle
Product ID
Result acknowledgement
Communication fault
Inspection timeout

The next project phase will replace the simulated inspection backend
with Beckhoff TwinCAT Vision and real image processing.

##Validation

The following behaviors have been tested using the digital twin:

Cold startup
Heating to operating temperature
Mold initialization
Automatic production
Multi-cycle operation
Controlled stop after current cycle
Shutdown and cooling
Emergency-stop behavior
Motor and motion interlocks
Hopper low-level timeout
Hopper fill timeout
Alarm handling
Vision PASS inspection
Vision FAIL inspection
Production quality statistics

## Technology

Beckhoff TwinCAT 3
IEC 61131-3 Structured Text
PLC state machines
PID control
Industrial interlocks
Digital-twin simulation
Machine-vision handshake
Industrial diagnostics

##Roadmap
 
 PLC machine controller
 Digital twin
 Interlocks
 PID temperature control
 Hopper control
 Alarm management
 Vision handshake simulation
 PASS / FAIL production statistics
 TwinCAT Vision integration
 Image-based defect inspection
 HMI
 Cycle-time analytics
 Final documentation and demo video