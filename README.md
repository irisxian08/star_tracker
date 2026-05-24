# Star Tracker Project

## PLEASE READ BEFORE STARTING PROJECT

---

## Introduction

This project is a motorized alt-azimuth star tracker/pointing system designed to rotate toward celestial coordinates using two stepper motors controlled by an ESP32 microcontroller. The long-term goal is to create a low-cost, modular system capable of automatically pointing a laser (and potentially a camera or telescope) toward astronomical targets using coordinate conversion algorithms and motorized movement.

The project combines mechanical design, electronics, soldering, embedded programming, and astronomical coordinate systems into a single integrated system. While the current version is still a prototype and several hardware issues remain unresolved, the project documents the full design process, including successful subsystems, failed attempts, debugging notes, and lessons learned.

This repository includes CAD models, wiring diagrams, motor-control code, calibration/error-detection ideas, troubleshooting notes, and engineering/design reflections. The goal of the repository is not just to show a finished build, but to document the actual engineering process behind building a system like this from scratch.

---

## Repository Navigation

### Main Files

- [Parts List](parts_list.xlsx)
- [Sources and References](sources.docx)
- [Engineering and Debugging Notes](notes.docx)

### Folders

- [CAD Files](cad_files/)
- [Photos and Build Documentation](photos/)

---

## Components List

See the full components list here:

[Parts List](parts_list.xlsx)

The components document includes:
- part names
- Amazon/product links
- approximate prices
- notes about compatibility
- suggested substitutions and alternatives

The cells colored green in the sources document correspond to especially important resources/components and should be prioritized.

The link to the relevant CAD files can be found here:

[CAD Files](cad_files/)

An electrical diagram can be found in the photos folder.

Please note that the laser used in this project is fairly old and is no longer widely available online. Before purchasing a replacement laser, make sure its dimensions are similar to the dimensions assumed by the laser holder CAD model. Otherwise, the holder may need to be modified slightly in Tinkercad or another CAD software before printing.

---

## Photos and Build Documentation

This folder contains development and assembly images for the star tracker project. The images are intended to document the prototyping and construction process rather than serve as formal assembly instructions.

Included images may show:
- the completed mechanical structure
- close-up views of the rotating turntable assembly
- breadboard testing setups
- the soldered circuit/perfboard assembly
- ESP32 power and testing states
- conceptual diagrams for the altitude rotation system
- internal wiring layouts
- partially assembled prototypes

Many of the images focus on intermediate stages of development and debugging rather than polished final results. The goal is to preserve the engineering process and document how different subsystems evolved throughout the project.

---

## Suggested Workflow

### 1. Learn the Coordinate System First

Before building hardware, it is important to understand the coordinate systems involved and how celestial coordinates translate into physical movement. Much of this project depends on rotational geometry and angular conversion.

Topics worth learning beforehand:
- altitude and azimuth coordinates
- right ascension and declination
- stepper motor stepping/microstepping
- gear ratios and rotational conversion
- basic spherical astronomy concepts

Without understanding the coordinate system, it becomes difficult to debug whether errors are mechanical, electrical, or mathematical.

---

### 2. Test Electronics Individually

**Do not permanently assemble or solder components before individually testing them.**

Recommended order (test using a breadboard):

1. Verify ESP32 connection to the computer
2. Upload a simple LED blink program
3. Test one DRV8825 driver with one motor
4. Verify stepping and direction behavior
5. Confirm stable power delivery from the battery and buck converter
6. Only after successful testing should soldering begin

A major lesson from this project was that debugging multiple unknowns simultaneously becomes extremely difficult once systems are integrated together.

---

### 3. Build the Mechanical Structure Separately

The mechanical structure should initially be tested independently from the electronics.

Important things to test:
- structural rigidity
- friction between moving surfaces
- gear engagement
- axle alignment
- wobble during rotation
- whether the motors can physically support the load

Small alignment problems can create surprisingly large pointing inaccuracies.

---

### 4. Integrate Motors With the Structure

Once the electronics and structure both work independently:

- mount motors onto the structure
- verify full rotational range
- measure actual conversion ratios
- compare expected vs observed movement

The current prototype uses experimentally measured conversion ratios between motor rotation and sky-angle movement. These values are approximate and should be recalibrated for any rebuild.

---

### 5. Implement Calibration and Error Checking

The current prototype uses a simple calibration/error-detection system based on physical angle dials attached to the altitude and azimuth axes. These dials allow comparison between the angle the software expects the mount to rotate and the angle the mount physically rotates in reality.

This is important because the system currently operates with an open-loop logic system. The ESP32 microcontroller assumes the motors successfully completed every commanded step, but there is no built-in way to verify this physically occurred.

Potential causes of error include:
- missed motor steps
- backlash
- structural flexing
- insufficient torque to drive mechanical components
- systematic calibration errors

The dial system provides a low-cost and mechanically simple way to detect these problems during testing.

---

### 6. Keep Extensive Notes

One of the most useful parts of the project ended up being the running engineering/debugging notes document.

The notes include:
- failed experiments
- wiring mistakes
- CAD revisions
- calibration attempts
- power-delivery problems
- design tradeoffs
- lessons learned

Reading these notes before attempting a rebuild is strongly recommended because they contain many practical warnings that are difficult to capture in formal documentation alone.

You can find the notes document here:

[Engineering and Debugging Notes](notes.docx)

---

## Electronics and Soldering Notes

This project involved significantly more hardware debugging than originally expected.

Important recommendations:
- use breadboards to test everything first before soldering
- avoid permanently soldering untested systems
- use common grounding carefully
- double-check DRV8825 orientation before powering
- verify buck converter voltage before connecting the ESP32
- expect electrical debugging to take much longer than anticipated
- be VERY conservative with work plans, since debugging after completion can take twice as much time (or more) than the original work itself

A large portion of the project’s development time was spent diagnosing electrical rather than programming problems.

---

## 3D Printing Notes

The parts for this project were printed primarily using Bambu Lab printers, specifically the X1 Carbon and A1 series printers.

If using different printers, tolerances and dimensions may come out slightly differently. Some parts may require small adjustments, sanding, resizing, or redesign depending on printer calibration and print quality.

Mechanical fit should therefore always be tested before fully assembling the structure.

---

## Current Issues

The project is currently incomplete due to unresolved hardware reliability problems.

### 1. Faulty circuit board

The perfboard/circuit assembly may contain unstable or incomplete connections, potentially causing inconsistent power delivery.

### 2. Possible faulty buck converter

The buck converter may not be supplying stable voltage/current under load. This may have contributed to ESP32 instability and inconsistent behavior.

### 3. ESP32 upload/connection problems

The ESP32 occasionally failed to enter proper upload mode and sometimes produced inconsistent serial behavior.

### 4. Motor driver/power uncertainty

There is still uncertainty regarding:
- driver configuration
- stable current delivery
- whether the motors are receiving sufficient power under load

These uncertainties were likely related to the issues described in section 1.

### 5. Mechanical precision limitations

The current gear-driven (azimuth) and tension-driven (altitude) systems are mechanically stable, but there are possible limitations regarding:
- step inconsistencies from gear manufacturing
- string lengthening over time due to continuous tension
- uneven weight distribution affecting smooth azimuthal rotation due to asymmetrical component placement on the turntable plate

### 6. Limited full-system testing

Because several subsystems failed simultaneously, the fully integrated system could not be reliably tested end-to-end.

Even though the final prototype is unfinished, much of the underlying infrastructure, design logic, and testing framework has been developed and documented.

---

## Potential Scale-Up Ideas

### 1. Stellarium Integration

A future version could connect directly to Stellarium so that selecting an object in the planetarium software automatically sends coordinates to the mount.

### 2. Closed-Loop Position Feedback

Future versions could incorporate:
- rotary encoders
- IMUs
- sensor fusion systems

This would allow the system to verify its actual orientation rather than assuming all commanded motor steps succeeded.

### 3. Telescope or Camera Mounting

The current laser-pointer payload could eventually be replaced with:
- a lightweight telescope
- an astrophotography camera
- a tracking camera system

This would require significantly improved structural rigidity and tracking precision but would make the project significantly more applicable to real-world scenarios.

### 4. Automated Object Tracking

The current system mainly focuses on pointing. A future version could continuously track celestial objects over time by dynamically updating motor positions.

---

## Sources + Helpful Links

See the full sources/reference document here:

[Sources and References](sources.docx)

This file contains tutorials, datasheets, astronomy references, ESP32/DRV8825 resources, CAD references, and troubleshooting material that were useful during development.

The cells colored green in the sources document correspond to especially important resources/components and should be prioritized.

Additional debugging notes and lessons learned can be found in the running notes document, which includes warnings, failed experiments, calibration attempts, and practical advice gathered throughout the build process.
