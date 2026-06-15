layout: post
title: "Engineering a Custom Pip-Boy Telemetry Dashboard"
date: 2026-06-15 10:00:00 -0400
categories: blogging car tech
---

# Designing the Dashboard Interface

For this stage of the build, the goal was to replace the factory cluster with a custom digital dashboard display to monitor live telemetry, specifically oil pressure and engine codes, during track events.

## Hardware Selection
The core of the system is driven by an ESP32-S3 microcontroller communicating over the CAN bus, outputting to a Waveshare LCD screen.

To keep the packaging tight behind the steering column, I designed a custom mount in Fusion 360. Power is managed via a compact single-cell battery shield. 

> **Download the CAD:** You can grab the `.step` file for the single-cell battery shield in the `/cad` directory of this repository [here](../cad/single-cell-battery-shield.step).

## Firmware Implementation

The firmware was written in C++ using PlatformIO. The animation rendering for the alert states required optimizing the loop to ensure no dropped frames when the oil pressure drops below the safe threshold.

Here is the snippet handling the critical alert triggers:

// Check oil pressure against safe operating limits
void checkEngineState(float currentOilPressure) {
    if (currentOilPressure < WARNING_THRESHOLD) {
        triggerVisualAlert(COLOR_RED, "LOW OIL PRESSURE");
        logTelemetryData();
    } else {
        renderNormalDisplay();
    }
}

The full PlatformIO project directory, including the display driver libraries, can be found in the `firmware/` folder.
