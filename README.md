# STM32 CAN Loopback

Embedded CAN communication project developed on an **STM32F446RE Nucleo** board using **STM32CubeIDE**.

The project focuses on implementing and understanding **CAN communication at peripheral level**, from bit timing and message filtering to interrupt-driven reception and circular buffering.

## Features

- CAN 2.0 communication using the STM32 **bxCAN** peripheral
- Internal **Loopback mode** — no external CAN transceiver required
- CAN bit timing configured for **500 kbit/s**
- Standard 11-bit CAN identifiers
- Hardware acceptance filtering
- Polling-based CAN reception
- Interrupt-driven CAN RX using **FIFO0**
- CAN RX callback handling
- Circular queue for buffering multiple received CAN frames
- Incremental payload generation for communication testing
- Debugging and validation using STM32CubeIDE
- Git version control and GitHub repository management

## Hardware

- **NUCLEO-F446RE**
- STM32F446RE microcontroller
- USB connection to PC

No external CAN transceiver or additional hardware is required because the project uses the STM32 CAN peripheral's internal Loopback mode.

## CAN Configuration

| Parameter | Configuration |
|---|---|
| CAN peripheral | CAN1 |
| Mode | Internal Loopback |
| CAN bitrate | 500 kbit/s |
| APB1 clock | 42 MHz |
| Prescaler | 6 |
| Time Segment 1 | 11 TQ |
| Time Segment 2 | 2 TQ |
| Synchronization Jump Width | 1 TQ |
| RX FIFO | FIFO0 |
| Identifier | Standard 11-bit |

### Bitrate calculation

With an APB1 clock of 42 MHz:

```text
Bitrate = 42 MHz / (6 × (1 + 11 + 2))
        = 500 kbit/s
```

## Reception Architecture

The project implements two reception approaches:

### Polling

The application periodically checks whether a CAN message is available in FIFO0.

### Interrupt-driven reception

CAN reception is handled using the FIFO0 message-pending interrupt:

```text
CAN frame
   ↓
CAN RX FIFO0
   ↓
CAN interrupt
   ↓
HAL_CAN_RxFifo0MsgPendingCallback()
   ↓
Circular RX queue
   ↓
Main application
```

The circular queue allows multiple received frames to be buffered before they are processed by the main loop.

## CAN Filtering

Hardware acceptance filtering is used to control which CAN identifiers are accepted by the CAN peripheral.

The project initially uses an acceptance filter configured in **ID-mask mode**, then tests reception with specific standard CAN identifiers.

## Testing

The project was validated using STM32CubeIDE debugging tools.

Tests include:

- CAN frame transmission in Loopback mode
- Correct reception through FIFO0
- CAN identifier filtering
- Polling reception
- Interrupt-driven reception
- Reception callback execution
- Multiple-frame buffering using a circular queue
- Payload integrity verification using incremented data patterns

Example payload:

```text
11 22 33 44 55 66 77 88
12 23 34 45 56 67 78 89
13 24 35 46 57 68 79 8A
```

## Project Structure

```text
STM32_CAN_Loopback/
├── Core/
│   ├── Inc/
│   └── Src/
├── Drivers/
├── STM32_CAN_Loopback.ioc
├── STM32F446RETX_FLASH.ld
└── README.md
```

## Tools

- STM32CubeIDE 1.19.0
- STM32CubeMX configuration
- STM32 HAL
- Embedded C
- Git / GitHub

## Skills Demonstrated

**Embedded C · STM32 · CAN / bxCAN · Interrupts · FIFO · Circular Buffer · Hardware Filtering · Polling · Debugging · Git**

## Future Improvements

Possible extensions of the project include:

- CAN error handling and error-state monitoring
- RX queue overflow management
- Application-level CAN protocol
- Command/response message handling
- Additional CAN robustness tests
- Physical CAN communication using an external transceiver