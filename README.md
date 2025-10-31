# Industrial_Controller

**Industrial-grade Embedded Controller** for a robotic drywall taping end-effector, based on STM32F407VG, FreeRTOS, and EtherCAT.

## Overview

This repository contains the firmware and design documentation for a high-performance embedded controller used in a robotic drywall taping system. The system handles tape feed, compound flow, force feedback, and communicates with a 6-axis robot via EtherCAT.

Key features:

* Real-time control loops (PID) for tape speed and pressure
* Force/Torque sensing via RS-422 (ATI Mini45)
* Distance sensing via ToF (VL53L4CD)
* Inertial sensing via IMU (ICM-42688-P)
* Motor drivers (BTS7960) for tape and pump motors and servo
* Safety features (E-stop, watchdog, fault monitoring)
* EtherCAT communication for industrial integration
* SD logging and USB DFU firmware update

---

## Getting Started

### Prerequisites

* STM32CubeIDE or equivalent
* STM32CubeMX for peripheral configuration
* ST-Link debugger
* Development board or custom PCB with STM32F407VG

### Setup

1.  Clone the repository:

    ```bash
    git clone [https://github.com/siddharth-roy27/Industrial_Controller.git](https://github.com/siddharth-roy27/Industrial_Controller.git)
    cd Industrial_Controller
    ```

2.  Open the `.ioc` file in STM32CubeIDE (or CubeMX) to configure peripherals.

3.  Generate the HAL code and import it into your IDE workspace.

4.  Build and flash the firmware using ST-Link.

---

## How to Use

On power-up, the MCU initializes and starts FreeRTOS tasks:

* `MotorControl`: Control loop for tape and pump motor
* `SensorUpdate`: Reads IMU, ToF, and flow sensors
* `SafetyMonitor`: Monitors fault conditions and E-stop
* `CommsEtherCAT`: Exchanges EtherCAT PDOs with robot master
* `Logger`: Logs data to SD or USB (planned)

Commands are sent from the robot controller through EtherCAT:

* `START_TAPE(speed, pressure)`
* `STOP_TAPE()`
* `SET_FLOW(rate)`
* `GET_STATUS()`
* `RESET_FAULT()`

Faults (overcurrent, low flow, communication loss, etc.) are handled by stopping the motors, signaling an error, and awaiting reset.

---

## Architecture

The system's high-level block diagram and PCB design files are referenced from the `images/` folder.

![Industrial Controller Architecture Block Diagram](images/architecture block diagram.png)

---

## Documentation

Detailed design documentation is split across the **hardware** and **firmware** directories.

### Hardware Design (Images & Files)

Visualization files for the electronic design are referenced from the `images/` folder. Note that links with spaces are enclosed in parentheses to ensure proper rendering in Markdown:

* [`Schematic Design (PNG)`](images/SCH_Schematic1_1-P1_2025-10-31.png) — Detailed image of the main schematic page.
* [`PCB Design Schematic (PNG)`](images/PCB design Schematic.png) — General visualization of the PCB circuit schematic.
* [`PCB 3D View 1 (PNG)`](images/PCB 3d (1).png) — First 3D view of the assembled PCB.
* [`PCB 3D View 4 (PNG)`](images/PCB 3d (4).png) — A different perspective 3D view.

### Firmware Design

Documentation detailing system logic and communication remains in the `firmware/Docs/` folder:

* [`System Block Diagram`](images/system block diagram.png) — High-level overview of system components and data flow.
* [`Fault Block Diagram`](images/fault block diagram.png) — Visualizing the fault monitoring and handling process.
* [`state_machine_diagram.md`](firmware/Docs/state_machine_diagram.md) — Describes system states (IDLE, TAPING, CLEANING, FAULT).
* [`control_loops.md`](firmware/Docs/control_loops.md) — Explains PID loops and feedback logic.
* [`fault_handling_table.md`](firmware/Docs/fault_handling_table.md) — Lists all faults and recovery actions.
* [`comms_protocol.md`](firmware/Docs/comms_protocol.md) — Defines EtherCAT commands and PDO mapping.

---

## Development Workflow

* Use Git for version control
* Maintain feature branches for new modules
* Use STM32CubeMX for peripheral or pin changes
* Develop with FreeRTOS for modular task management
* Keep all code in `Core/Inc` and `Core/Src` well-documented
* Update design documents in `Docs/` as the system evolves
* Commit and push regularly for review and versioning

---

## Contributing

Contributions are welcome.
To contribute:

1.  Fork the repository
2.  Create a new branch (for example: `feature/ethercat-integration`)
3.  Make changes and commit
4.  Submit a Pull Request with a detailed explanation of changes

Please follow embedded firmware coding and documentation best practices.

---

## License and Credits

Licensed under the [MIT License](LICENSE).
See `LICENSE` for more details.

Developed by Siddharth Roy.

Acknowledgements:

* STMicroelectronics (STM32 MCU and sensors)
* Infineon / SinoWealth (Motor drivers)
* Texas Instruments (PHY, op-amps, regulators)
* EtherCAT Technology Group and open-source contributors

---

## Support

For issues, open a GitHub Issue in this repository.
You may also contact: `siddharth-roy27@github.com`

---

## Roadmap

* Initial firmware skeleton and FreeRTOS integration
* EtherCAT PDO implementation
* SD logging and DFU firmware update via USB
* Secure boot with firmware signature verification
* Adaptive control and PID tuning
* Vision-based sensing integration (future enhancement)

---

## References

* STM32F407VG Datasheet and Reference Manual — STMicroelectronics
* BTS7960 Motor Driver Datasheet — Infinegon Technologies
* EtherCAT Slave Controller (LAN9252) — Microchip
* ICM-42688-P IMU Datasheet — TDK InvenSense
* FreeRTOS Kernel Documentation
* ST HAL API Reference
* Relevant research papers and automation standards
