# 🔐 Multilayer Hardware Monitoring System

## Introduction
The system monitors physical access attempts through a hardware keypad connected to an Arduino.  
Events are captured in real time by a Raspberry Pi, stored with timestamps, and analyzed through Nagios alerts and an Excel dashboard revealing not only how many access attempts occurred, but exactly when and whether the system was compromised.

---

## Project Goal
Deploy a multi-layer monitoring system on a Raspberry Pi integrating real hardware (similar to devices we interact with daily, such as ATMs) with a logging, alerting, and visualization stack.

The goal was to build a working end-to-end system, not a simulation.

This project was fully documented including setup steps, issues encountered, troubleshooting decisions, and the reasoning behind key technical choices.

---

## Why does this matter?
This project integrates physical hardware with a real monitoring stack, the same way production systems work.  
It demonstrates hands-on experience with hardware control, log analysis in a Linux environment, and real-time alerting through Nagios.

---

## Table of Contents
1. System Description  
2. Workflow and Architecture  
3. Layer 1 – Arduino Hardware  
4. Layer 2 – Raspberry Pi Integration  
5. Layer 3 – Monitoring with Nagios  
6. Layer 4 – Excel Visualization  

---

## System Description
The system detects authentication attempts through a keypad connected to an Arduino Uno.  
When an incorrect password is entered, the Arduino sends an event over serial communication to a Raspberry Pi.

The Raspberry Pi logs the event with a timestamp into a CSV file.  
Nagios continuously monitors this file and raises alerts when failures or lockouts occur.  
Finally, Excel is used to visualize access patterns and anomalies.

---

## Workflow and Architecture

## Hardware Components
![Hardware](images/imagen1.jpg)
| Component | Role |
|-----------|------|
| Arduino Uno | Main microcontroller and authentication logic |
| Protoboard | Hardware connection base |
| Jumper wires | Physical connections |
| 4x4 Matrix Keypad | Password input |
| LCD 16x2 | Displays system state |
| 10K Potentiometer | Controls LCD contrast |
| USB Cable | Serial communication to Raspberry Pi |
| Laptop | Arduino development |
| Raspberry Pi | Python logger + Nagios stack |


## Raspberry Pi Integration
This layer acts as the bridge between the hardware and monitoring systems.  
The Python script runs continuously on the Raspberry Pi, listening to the USB serial port where the Arduino broadcasts events.

Its only job is to receive each message, add a timestamp, and save it to a CSV file.  
Python does not interpret or evaluate events — that responsibility belongs to Nagios.



## Raspberry Pi Integration
This layer acts as the bridge between the hardware and monitoring systems.  
The Python script runs continuously on the Raspberry Pi, listening to the USB serial port where the Arduino broadcasts events.

Its only job is to receive each message, add a timestamp, and save it to a CSV file.  
Python does not interpret or evaluate events — that responsibility belongs to Nagios.


---

## Monitoring with Nagios
Nagios monitors the CSV log in real time.  
A custom plugin evaluates recent log entries and raises alerts based on failed attempts and lockout events.

This provides real-time visibility into authentication activity.

---

## Layer Responsibility
The Raspberry Pi integration layer acts as the lightweight event ingestion and persistence component.  
It continuously listens to the Arduino serial output, captures authentication events, and stores them with timestamps.

This layer does not evaluate system health or generate alerts.  
Its purpose is to provide a reliable and structured event stream consumed by monitoring and analytics tools.

---

## Excel Visualization
The Excel dashboard provides a lightweight analytics layer on top of the monitoring pipeline.

By importing the CSV log generated on the Raspberry Pi, authentication events can be aggregated and visualized to reveal:
- Authentication outcome distribution
- Access attempt frequency
- Failure correlation and lockout patterns
- Time-based anomalies

---

## Features
- CSV-based event ingestion  
- Authentication outcome distribution analysis  
- Access attempt frequency tracking  
- Visual correlation of failures and lockout events  

---

## Conclusion
This project demonstrates how physical hardware can be integrated with a real monitoring stack to create an end-to-end observability pipeline.

It highlights practical experience in:
- Embedded systems
- Linux event logging
- Monitoring and alerting
- Data visualization
- System architecture thinking
