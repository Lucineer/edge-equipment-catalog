# Edge Equipment Catalog

**Status:** Specification / Design
**Source:** Multi-model forum (DeepSeek-chat engineering, Kimi K2.5 boarding, DeepSeek-Reasoner architecture)
**Date:** 2026-04-04

## Overview

Modular equipment system for edge-native repo-agents. Each equipment item is a self-contained agent with defined interfaces, hardware requirements, and trust-level gating. A vessel builder selects hardware → loads universal base → equips each ESP32 with appropriate bytecode.

## Equipment Matrix

### 1. motor-control-agent
- **Hardware:** 2x PWM (LEDC), 2x quadrature encoder, 2x ADC (current), 1x GPIO (enable), 1x timer
- **Trust Minimum:** L3 (Critical Control)
- **Flash:** 48KB | **RAM:** 8KB | **PSRAM:** Not required
- **Produces:** PWM signals, motor status, fault alerts
- **Consumes:** Target RPM, current limits, emergency stop
- **Coordinates With:** deep-monitor, nav-compute
- **Core:** 1 (away from WiFi), ISR priority 1

### 2. deep-monitor-agent
- **Hardware:** 1x CAN (TWAI), 1x UART (Jetson alerts), 1x GPIO (heartbeat)
- **Trust Minimum:** L2 (Monitoring)
- **Flash:** 64KB | **RAM:** 12KB | **PSRAM:** 256KB
- **Produces:** CAN anomaly scores, health reports, predictive failure alerts
- **Consumes:** Raw CAN traffic, thresholds, time sync
- **Coordinates With:** All agents (health checks), marine-bridge
- **Core:** Either, ISR priority 2

### 3. sensor-pipeline-agent
- **Hardware:** 2x I2C, 1x I2S, 1x SPI, 4x DMA, camera interface (optional)
- **Trust Minimum:** L2 (Monitoring)
- **Flash:** 96KB | **RAM:** 16KB | **PSRAM:** 4MB
- **Produces:** Filtered sensor readings, inference results, timestamped data
- **Consumes:** Sensor config, calibration, inference models
- **Coordinates With:** nav-compute, comms-relay
- **Core:** 0, ISR priority 2-3

### 4. nav-compute-agent
- **Hardware:** 1x UART (GPS), 1x I2C (IMU)
- **Trust Minimum:** L2 (Navigation)
- **Flash:** 48KB | **RAM:** 12KB | **PSRAM:** 512KB
- **Produces:** Position, heading, velocity, waypoint status, danger alerts
- **Consumes:** GPS data, IMU data, waypoint list, chart data
- **Coordinates With:** motor-control, deep-monitor, sensor-pipeline
- **Core:** Either, ISR priority 2

### 5. marine-bridge-agent
- **Hardware:** 1x CAN, 1x UART, NMEA 2000 decoder
- **Trust Minimum:** L2 (Integration)
- **Flash:** 32KB | **RAM:** 8KB | **PSRAM:** 64KB
- **Produces:** Translated NMEA data, autopilot commands, display data
- **Consumes:** NMEA 2000 traffic, Seatalk NG, proprietary protocols
- **Coordinates With:** deep-monitor, control-interface, nav-compute
- **Core:** Either, ISR priority 3

### 6. comms-relay-agent
- **Hardware:** WiFi + BLE + 1x UART
- **Trust Minimum:** L1 (Communication)
- **Flash:** 48KB | **RAM:** 12KB | **PSRAM:** 128KB
- **Produces:** Routed messages, telemetry, heartbeat
- **Consumes:** Fleet messages, routing tables, WiFi config
- **Coordinates With:** All agents (message routing)
- **Core:** 0 (WiFi stack), ISR priority 3

## Compatibility Matrix

| | motor | monitor | sensor | nav | bridge | comms |
|---|---|---|---|---|---|---|
| **motor** | ❌ | ⚠️ | ❌ | ✅ | ✅ | ✅ |
| **monitor** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **sensor** | ❌ | ✅ | ⚠️ | ✅ | ❌ | ⚠️ |
| **nav** | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ |
| **bridge** | ✅ | ✅ | ❌ | ✅ | ⚠️ | ✅ |
| **comms** | ✅ | ✅ | ⚠️ | ✅ | ✅ | ❌ |

✅ = compatible | ⚠️ = compatible with care | ❌ = resource conflict

## Key Conflicts

- **Motor + Sensor:** DMA contention (6 channels total, camera needs 4-5) + PSRAM bus saturation
- **Motor + Motor:** Timer + DMA resource conflict
- **Sensor + Comms:** WiFi ISRs may affect DMA timing

## Universal Base

All equipment shares:
- Bootloader with capability detection (~32KB)
- INCREMENTS trust engine
- CAN/UART comms stack
- OTA update mechanism
- Watchdog management
- Logging framework

**Total shared: ~128KB flash. Equipment-specific: 16-96KB each.**

## References

- Edge-Native Architecture Research: `docs/EDGE-NATIVE-ARCHITECTURE-RESEARCH.md`
- INCREMENTS Fleet Trust: `github.com/Lucineer/increments-fleet-trust`
- Edge Boarding Protocol: `github.com/Lucineer/edge-boarding-protocol`

## License

Superinstance & Lucineer (DiGennaro et al.) — 2026
