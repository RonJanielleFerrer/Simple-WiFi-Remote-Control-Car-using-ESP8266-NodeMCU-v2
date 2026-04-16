# Sumo Line-Following Remote-Controlled Robot (3-in-1 System)

## Introduction
This project presents the design and implementation of a multi-functional mobile robot that integrates line-following, sumo, and remote-controlled (RC) operation modes into a single platform. The system applies fundamental and advanced concepts in communication systems, including signal transmission, wireless communication, sensor-based data acquisition, and real-time control. By combining hardware and embedded system design, the project demonstrates practical applications of communication principles in autonomous and semi-autonomous robotic systems.

---

## Description
The Sumo Line-Following Remote-Controlled Robot is a 3-in-1 mobility system capable of operating in three distinct modes: line-following, sumo combat, and manual remote control. The robot utilizes an Arduino Uno as the main controller, interfaced with motor drivers, sensors, and a wireless communication module.

In line-following mode, the robot uses a line tracking sensor to detect and follow a predefined path. In sumo mode, an ultrasonic sensor is used to detect opponents and execute pushing strategies. In RC mode, a 2.4 GHz transmitter and receiver system enables real-time wireless control by the user.

The system integrates multiple subsystems including sensing, processing, actuation, and communication, making it a comprehensive demonstration of embedded and communication system design.

---

## Components / Parts Used
- Arduino Uno (Main Microcontroller)
- SparkFun Motor Driver – TB6612FNG
- 3-Channel Line Tracking Sensor Module
- Ultrasonic Sensor (HC-SR04)
- 2 × DC Gear Motors (12V SGM37-3530, 1000 RPM)
- 6-Channel 2.4 GHz Transmitter and Receiver (RC System)
- Robot Chassis and Wheels
- Power Supply (Battery Pack)
- Connecting Wires and Mounting Hardware

---

## Communication System Overview
Communication plays a critical role in the operation of the robot, particularly in its remote-controlled functionality. The system employs a 2.4 GHz radio frequency (RF) transmitter and receiver pair to enable wireless data transmission between the user and the robot.

The transmitter encodes control inputs such as direction and speed into RF signals, which are transmitted through free space and received by the onboard receiver module. The receiver decodes these signals and sends corresponding control commands to the Arduino microcontroller. This process demonstrates key communication system principles such as modulation, signal transmission, reception, and decoding.

Additionally, internal communication occurs between sensors and the microcontroller. The line tracking sensor and ultrasonic sensor continuously transmit electrical signals representing environmental data. These signals are processed in real time, allowing the system to respond dynamically. This reflects concepts in data communication, signal processing, and system integration.

---

## Importance of the Project
This project highlights the integration of multiple communication and control techniques into a single, versatile robotic platform. By combining three operational modes, the system demonstrates adaptability and efficient use of hardware resources.

From an engineering perspective, the project reinforces important concepts such as wireless communication, sensor interfacing, real-time signal processing, and system design. It also provides practical experience in implementing communication links between transmitter and receiver systems, as well as interpreting sensor data for autonomous decision-making.

The 3-in-1 functionality enhances the system’s value by demonstrating how a single platform can be reconfigured for different applications, including autonomous navigation, competitive robotics, and manual control systems. This reflects real-world engineering practices where flexibility and integration are essential.

---

## Conclusion
The Sumo Line-Following Remote-Controlled Robot successfully demonstrates the application of communication system principles in a practical and integrated platform. Through the implementation of wireless control, sensor-based feedback, and multiple operational modes, the project showcases how theoretical concepts such as modulation, signal transmission, and data processing can be applied in real-world systems.

The project also emphasizes the importance of system integration, reliability, and adaptability in modern communication and control applications. Overall, it serves as a comprehensive demonstration of advanced communication system design and its role in robotics and automation.
