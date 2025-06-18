# NecroEndercon - Quality-Focused 3D Printer Configuration

## Overview
**NecroEndercon** is a precision-oriented 3D printer configuration based on Klipper firmware, designed for **quality over speed**. This configuration has been specifically optimized for high-precision stepper motors and robust hardware to deliver exceptional print quality.

### Key Specifications
- **Mainboard**: BigTreeTech (BTT) Octopus EZ Max
- **Stepper Drivers**: EZ TMC2209 (4x)
- **Probe**: Beacon3D Eddy Current Probe
- **Kinematics**: Cartesian with dual Z-axis
- **Focus**: Quality and precision over speed

## Hardware Configuration

### Stepper Motors (Premium Quality)

#### X/Y/E Axis Motors: E3D MY-1704HSM168RE
- **Step Angle**: 0.9° (400 steps/revolution - high precision)
- **Current Rating**: 1.68A per phase
- **Holding Torque**: 4.4 kg·cm (61 oz-in)
- **Resistance**: 1.65Ω per phase
- **Inductance**: 2.8mH per phase
- **Benefit**: Superior precision and surface finish quality

### Hotend and Extruder Setup

#### E3D Revo 6 Hotend
- **Heater**: 40W cartridge heater
- **Sensor**: PT1000 temperature sensor
- **Max Temperature**: 300°C capability
- **Nozzle**: 0.4mm (standard)
- **Configuration**: Bowden tube setup

#### Bondtech Extruder
- **Type**: BMG/LGX compatible
- **Gear Ratio**: 3:1 reduction
- **Drive**: Remote (not direct drive)
- **Filament**: 1.75mm compatible

#### Z Axis Motors: 17HS19-2004S1 (Dual Setup)
- **Step Angle**: 1.8° (200 steps/revolution)
- **Current Rating**: 2.0A per phase (each motor)
- **Holding Torque**: ~4-5 kg·cm (strong Z-axis control)
- **Configuration**: Shared single TMC2209 driver via 3A/3B connections
- **Benefit**: Reliable, high-torque Z-axis operation

### BTT Octopus EZ Max Motor Assignments

| Position | Motor | Stepper Pins | TMC2209 UART | Current Setting |
|----------|-------|--------------|--------------|-----------------|
| **M1** | X-axis | PF13/PF12/!PF14 | PC4 | 1.4A (83% of rated) |
| **M2** | Y-axis | PG0/PG1/!PF15 | PD11 | 1.4A (83% of rated) |
| **M3A/B** | Z1 & Z2 | PF11/PG3/!PG5 | PC6 | 1.6A (80% of rated) |
| **M4** | Extruder | PG4/PC1/!PA0 | PD3 | 1.4A (83% of rated) |

### Endstops and Sensors
- **X Endstop**: ^PG6
- **Y Endstop**: ^PG9  
- **Z Endstop**: Virtual (Beacon3D probe)
- **Hotend Thermistor**: PF4
- **Bed Thermistor**: PC3

### Heaters
- **Hotend Heater**: PA2
- **Bed Heater**: PA0

## Quality-Focused Configuration

### Motion Settings (Optimized for Quality)
```ini
max_velocity: 200          # Conservative for quality
max_accel: 2000           # Smooth acceleration
max_accel_to_decel: 1000  # Gentle deceleration
max_z_velocity: 10        # Precise Z movement
max_z_accel: 100          # Smooth Z acceleration
square_corner_velocity: 5.0 # Precise corners
```

### TMC2209 Driver Optimization
- **StealthChop**: Disabled on X/Y/E for precision, enabled on Z for quiet operation
- **Interpolation**: Enabled for smoother operation
- **Current Settings**: Conservative for thermal safety and motor longevity
- **Sensorless Homing**: Configured with proper SGTHRS values

### Dual Z-Motor Configuration
- **Shared Driver**: Both Z motors controlled by single TMC2209 (M3A/B)
- **Automatic Synchronization**: Electrical synchronization via parallel wiring
- **Z-Tilt Compensation**: Probe-based leveling compensates for mechanical differences
- **Precision Tolerances**: 0.005mm retry tolerance for superior bed leveling

## Configuration Files

### Core Configuration
- **`printer.cfg`** - Main configuration with quality-focused settings
- **`printer_base.cfg`** - BTT Octopus EZ Max board configuration
- **`dual_z_motors.cfg`** - Dual Z-motor setup with precision macros
- **`input_shaper.cfg`** - Resonance compensation with quality/speed modes

### Specialized Modules
- **`beacon3d_probe.cfg`** - Beacon3D eddy current probe configuration
- **`fan_control.cfg`** - Intelligent fan control system
- **`leveling.cfg`** - Comprehensive bed leveling procedures
- **`pid_tuning.cfg`** - Temperature control and PID optimization
- **`testing.cfg`** - Diagnostic and testing procedures
- **`macros.cfg`** - Print management and utility macros

## Print Quality Modes

### Quality Mode (Default)
- **Acceleration**: 1500 mm/s²
- **Velocity**: 150 mm/s
- **Corner Velocity**: 3.0 mm/s
- **Use**: Maximum quality, finest details

### Standard Mode
- **Acceleration**: 2000 mm/s²
- **Velocity**: 200 mm/s
- **Corner Velocity**: 5.0 mm/s
- **Use**: Balanced quality and speed

### Speed Mode
- **Acceleration**: 2500 mm/s²
- **Velocity**: 250 mm/s
- **Corner Velocity**: 8.0 mm/s
- **Use**: Faster printing when quality allows

## Installation and Setup

### 1. Firmware Installation
```bash
# Flash Klipper firmware for STM32F446 to BTT Octopus EZ Max
# Note the USB serial ID for configuration
```

### 2. Configuration Deployment
```bash
# Copy all .cfg files to your Klipper config directory
# Update serial port in printer_base.cfg
```

### 3. Motor Calibration Sequence
```gcode
# 1. Home all axes
G28

# 2. Test motor operation
TEST_Z_MOTORS

# 3. Calibrate dual Z setup
DUAL_Z_LEVEL

# 4. Set quality mode
SET_QUALITY_MODE
```

## Key Macros and Features

### Print Management
- **`START_PRINT TEMP=200 BED_TEMP=60`** - Optimized print start
- **`END_PRINT`** - Complete print finish sequence
- **`PAUSE`** / **`RESUME`** - Advanced pause/resume with temperature control

### Quality Control
- **`SET_QUALITY_MODE`** - Ultra-precise printing
- **`SET_STANDARD_MODE`** - Balanced performance
- **`SET_SPEED_MODE`** - Faster printing when needed

### Calibration
- **`DUAL_Z_LEVEL`** - Precision dual Z leveling
- **`SHAPER_CALIBRATE`** - Input shaper calibration for 0.9° motors
- **`Z_OFFSET_CALIBRATE`** - Precise Z offset calibration

### Diagnostics
- **`TEST_Z_MOTORS`** - Test dual Z motor operation
- **`CHECK_Z_SYNC`** - Verify Z-axis synchronization
- **`TEST_RESONANCES_X`** / **`TEST_RESONANCES_Y`** - Resonance testing

## Motor Specifications Summary

| Component | Application | Key Specs | Notes |
|-----------|-------------|------------|-------|
| E3D MY-1704HSM168RE | X/Y/E Motors | 0.9°, 1.68A, 4.4 kg·cm | Ultra-High Precision |
| 17HS19-2004S1 | Z1/Z2 Motors | 1.8°, 2.0A, 4-5 kg·cm | High Torque |
| E3D Revo 6 | Hotend | 40W, PT1000, 300°C | Quick-Change Nozzles |
| Bondtech Extruder | Filament Drive | 3:1 Ratio, Bowden | Precise Extrusion |

## Advantages of This Configuration

### Superior Print Quality
- **0.9° X/Y motors** provide 2x resolution of standard 1.8° motors
- **Conservative motion settings** eliminate vibrations and artifacts
- **Precision Z-axis** ensures perfect layer adhesion

### Robust Hardware
- **High-torque motors** prevent skipped steps under load
- **Thermal-safe current settings** ensure long motor life
- **Quality TMC2209 drivers** provide smooth, quiet operation

### Intelligent Control
- **Shared Z-driver** simplifies wiring while maintaining precision
- **Automatic bed leveling** compensates for mechanical variations
- **Multiple quality modes** for different printing needs

## Troubleshooting

### Z-Axis Issues
```gcode
# If Z motors lose synchronization
EMERGENCY_Z_ALIGN

# Test Z motor operation
TEST_Z_MOTORS

# Check synchronization
CHECK_Z_SYNC
```

### Quality Issues
```gcode
# Recalibrate input shaper for 0.9° motors
SHAPER_CALIBRATE

# Switch to quality mode
SET_QUALITY_MODE

# Verify resonance frequencies
TEST_RESONANCES_X
TEST_RESONANCES_Y
```

## Contributing

This configuration is optimized for quality-focused 3D printing with premium hardware. Contributions should maintain the focus on print quality and precision over speed.

## License

Open source configuration for the 3D printing community.

---

**NecroEndercon** - Where precision meets performance in 3D printing. 