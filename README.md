

# LED Blinking with Watchdog Supervision on STM32

## Overview

This project demonstrates an LED blinking application with watchdog supervision on an STM32 microcontroller. The project ensures that the LED blinks with a 500ms periodicity and incorporates a watchdog timer that resets if the LED management function does not operate correctly.

## Components

### 1. GPIO
- **GPIO_Init**: Initializes GPIO configuration for the pin controlling the LED.
- **GPIO_Write**: Writes a specific value (0 or 1) to the LED pin.

### 2. LEDMgr
- **LED_Init**: Initializes the internal variables of the LED manager.
- **LED_Manage**: Manages the LED blinking actions using `GPIO_Write`. Called every 10ms to maintain a 500ms blink cycle.

### 3. WDGDrv
- **WDGDrv_Init**: Configures the watchdog timer with a 50ms timeout, disables window mode, enables early interrupt, and activates the watchdog.
- **WDGDrv_IsrNotification**: Refreshes the watchdog timer if `WDGM_MainFunction` is not stuck and the state is OK.

### 4. WDGM
- **WDGM_Init**: Initializes internal variables.
- **WDGM_MainFunction**: Checks the call frequency of `LED_Manage` within a 100ms period. Called every 20ms.
- **WDGM_ProvideSupervisionStatus**: Provides the supervision status of the LED manager.
- **WDGM_AlivenessIndication**: Indicates that `LED_Manage` is being called at the correct timing.

## Simulation Scenarios

### A. Positive Scenario
- **Objective**: Verify the periodicity of LED blinking, calls to `LED_Manage`, `WDGM_MainFunction`, and watchdog refreshment.
- **Method**: Toggle test pins and observe on an oscilloscope.

### B. Negative Scenario 1
- **Objective**: Ensure watchdog reset occurs after 50ms when `WDGM_MainFunction` is not called.
- **Method**: Comment out the call to `WDGM_MainFunction` and observe the reset.

### C. Negative Scenario 2
- **Objective**: Ensure watchdog reset occurs after 100ms when `WDGM_AlivenessIndication` is not called.
- **Method**: Comment out `WDGM_AlivenessIndication` in `LED_Manage` and observe the reset.

### D. Negative Scenario 3
- **Objective**: Ensure watchdog reset occurs after 100ms when `LED_Manage` is called every 5ms.
- **Method**: Modify the call frequency of `LED_Manage` and observe the reset.

## Usage

1. Clone the repository:
    ```bash
    git clone https://github.com/yourusername/led-watchdog-stm32.git
    cd led-watchdog-stm32
    ```

2. Open the project in your preferred STM32 development environment (e.g., STM32CubeIDE).

3. Build and flash the code onto your STM32 microcontroller.

4. Run the simulations to verify the functionality and supervision capabilities.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Authors

- **Ahmed Mohamed Ali** 

## Acknowledgments

- Inspired by standard embedded systems design practices.
- Special thanks to the STM32 community for their support and resources.



