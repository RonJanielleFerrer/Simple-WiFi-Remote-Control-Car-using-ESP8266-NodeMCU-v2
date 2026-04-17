# Simple WiFi Remote Control Car using ESP8266 NodeMCU v2

---

## Introduction 
This project presents the design and development of a WiFi-based remote-controlled car using the ESP8266 NodeMCU v2 microcontroller. The system applies key concepts from communication engineering, including wireless transmission, signal processing, and real-time control. By utilizing embedded networking and microcontroller-based design, the project demonstrates how modern communication technologies can be implemented in mobile robotic systems for remote operation and control.

---

## Objective
- To design a WiFi-controlled mobile robot using ESP8266.
- To implement wireless communication between user and robot.
- To apply communication system principles in a real-world system.
- To demonstrate remote control using network-based communication.

---

## Materials and Components Used
- ESP8266 NodeMCU v2 (CP2102)
- L298N Dual H-Bridge Motor Driver Module
- 2 × MiniQ Wheel and Bracket Set
- 2 × Micro Metal DC Gear Motor (6V 500RPM)
- 2 × Lithium-ion 18650 2600 mAh Rechargeable Batteries
- Connecting wires and chassis

---

## Description
The Simple WiFi Remote Control Car is a mobile robotic platform that can be wirelessly controlled using a smartphone or computer over a WiFi network. The system is built around the ESP8266 NodeMCU v2, which serves as both the microcontroller and wireless communication module.

The NodeMCU receives control commands from a user interface, processes these signals, and sends appropriate outputs to the L298N motor driver module. The motor driver then controls the direction and speed of the DC motors, allowing the car to move forward, backward, left, and right. The entire system is powered by rechargeable lithium-ion batteries, making it portable and efficient.

---

## Images
  
### Top View
<img width="1800" height="2400" alt="RC Car Top" src="https://github.com/user-attachments/assets/3a08199b-e287-44a1-9c7e-a3fbc003d413" />

### Right Side View
<img width="2382" height="1787" alt="RC Car Right" src="https://github.com/user-attachments/assets/0ba4c142-0671-4e97-9702-19d0ecf13afc" />

### Left Side View
<img width="2400" height="1800" alt="RC Car Left" src="https://github.com/user-attachments/assets/1e82766b-471a-4a13-a841-1b5ffaf49937" />

### Isometric View
<img width="2400" height="1800" alt="RC Car Isometric" src="https://github.com/user-attachments/assets/5cba56d9-3a68-43e8-9c5f-bde8455c2cd3" />

---

## Documentation

### RC Car Test 1
https://github.com/user-attachments/assets/08931302-4303-4865-92dd-b561983b87ca

### RC Car Test 1
https://github.com/user-attachments/assets/e81d43bc-9e58-4e93-8c35-bc8fe050ed6a

---

## Communication System Overview
Communication is the core component of this project, as it enables real-time wireless control of the vehicle. The ESP8266 NodeMCU uses WiFi technology to establish a connection between the user and the robot. The microcontroller acts as a server or access point, receiving commands transmitted from a client device such as a smartphone or laptop.

User inputs are encoded into digital signals and transmitted over a wireless medium using radio frequency (RF) signals in the 2.4 GHz band. These signals follow communication protocols such as TCP/IP, allowing reliable data exchange. Upon reception, the NodeMCU decodes the signals and converts them into motor control commands.

This process demonstrates key communication system principles such as modulation, transmission media, signal encoding and decoding, and data communication. Additionally, it highlights the role of wireless networks and embedded systems in modern telecommunication applications.

---

## Results and Discussion
The system successfully enabled wireless control of the robot using a web-based interface. Commands sent from a smartphone or computer were received and processed in real time by the NodeMCU, resulting in immediate motor response. The communication link remained stable within the WiFi coverage area, demonstrating reliable signal transmission and control.

---

## Importance of the Project
This project is important as it demonstrates the practical application of wireless communication systems in controlling real-world devices. It integrates concepts such as data communication, networking, signal processing, and embedded system design into a single functional platform.

From an engineering perspective, the project provides hands-on experience in implementing communication links using WiFi technology, which is widely used in modern telecommunication networks. It also shows how communication systems can be used for remote operation, automation, and control in robotics.

Although simple in design, the system reflects real-world applications such as remote vehicles, smart devices, and IoT-based control systems. It serves as a foundational model for more advanced communication-based robotic systems.

---

## Reflection / Learning Summary
This project enhanced understanding of wireless communication, embedded systems, and real-time control. It provided practical experience in implementing WiFi-based data transmission and interpreting control signals for motor operation. The activity also highlighted the importance of system integration and reliable communication in modern engineering applications.

---

## Conclusion
The Simple WiFi Remote Control Car successfully demonstrates the integration of communication system principles with embedded hardware design. By utilizing WiFi-based communication, the project enables real-time control of a mobile platform, showcasing the role of wireless transmission and networking in modern systems.

Overall, the project reinforces theoretical concepts learned in communication engineering and highlights their practical applications in robotics, automation, and smart systems.
