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
## Code
[Upl#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>
#include <ArduinoOTA.h>

// connections for drive Motor FR & BR
//int enA = D3;
int in1 = D5;
int in2 = D6;
// connections for drive Motor FL & BL
int in3 = D7;
int in4 = D8;
//int enB = D6;

const int buzPin = D0;      // set digital pin D7 as buzzer pin (use active buzzer)
const int ledPin = D1;      // set digital pin D8 as LED pin (use super bright LED)
const int wifiLedPin = D2;  // set digital pin D0 as indication, the LED turn on if NodeMCU connected to WiFi as STA mode

String command;    // String to store app command state.
int SPEED = 122;
int speed_Coeff = 3;

ESP8266WebServer server(80);  // Create a webserver object that listens for HTTP request on port 80

unsigned long previousMillis = 0;

String sta_ssid = "";      // set Wifi networks you want to connect to
String sta_password = "";  // set password for Wifi networks


void setup() {
  Serial.begin(115200);  // set up Serial library at 115200 bps
  Serial.println();
  Serial.println("*WiFi Robot Remote Control Mode - L298N 2A*");
  Serial.println("------------------------------------------------");

  pinMode(buzPin, OUTPUT);      // sets the buzzer pin as an Output
  pinMode(ledPin, OUTPUT);      // sets the LED pin as an Output
  pinMode(wifiLedPin, OUTPUT);  // sets the Wifi LED pin as an Output
  digitalWrite(buzPin, LOW);
  digitalWrite(ledPin, LOW);
  digitalWrite(wifiLedPin, HIGH);

  // Set all the motor control pins to outputs
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);

 
  // Turn off motors - Initial state
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);


  // set NodeMCU Wifi hostname based on chip mac address
  String chip_id = String(ESP.getChipId(), HEX);
  int i = chip_id.length() - 4;
  chip_id = chip_id.substring(i);
  chip_id = "WiFi_RC_Car-" + chip_id;
  String hostname(chip_id);

  Serial.println();
  Serial.println("Hostname: " + hostname);

  // first, set NodeMCU as STA mode to connect with a Wifi network
  WiFi.mode(WIFI_STA);
  WiFi.begin(sta_ssid.c_str(), sta_password.c_str());
  Serial.println("");
  Serial.print("Connecting to: ");
  Serial.println(sta_ssid);
  Serial.print("Password: ");
  Serial.println(sta_password);

  // try to connect with Wifi network about 10 seconds
  unsigned long currentMillis = millis();
  previousMillis = currentMillis;
  while (WiFi.status() != WL_CONNECTED && currentMillis - previousMillis <= 10000) {
    delay(500);
    Serial.print(".");
    currentMillis = millis();
  }

  // if failed to connect with Wifi network set NodeMCU as AP mode
  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("");
    Serial.println("*WiFi-STA-Mode*");
    Serial.print("IP: ");
    Serial.println(WiFi.localIP());
    digitalWrite(wifiLedPin, LOW);  // Wifi LED on when connected to Wifi as STA mode
    delay(3000);
  } else {
    WiFi.mode(WIFI_AP);
    WiFi.softAP(hostname.c_str());
    IPAddress myIP = WiFi.softAPIP();
    Serial.println("");
    Serial.println("WiFi failed connected to " + sta_ssid);
    Serial.println("");
    Serial.println("*WiFi-AP-Mode*");
    Serial.print("AP IP address: ");
    Serial.println(myIP);
    digitalWrite(wifiLedPin, HIGH);  // Wifi LED off when status as AP mode
    delay(3000);
  }


  server.on("/", HTTP_handleRoot);     // call the 'handleRoot' function when a client requests URI "/"
  server.onNotFound(HTTP_handleRoot);  // when a client requests an unknown URI (i.e. something other than "/"), call function "handleNotFound"
  server.begin();                      // actually start the server

  ArduinoOTA.begin();  // enable to receive update/uploade firmware via Wifi OTA
}

void loop() {
  ArduinoOTA.handle();    // listen for update OTA request from clients
  server.handleClient();  // listen for HTTP requests from clients

  command = server.arg("State");  // check HTPP request, if has arguments "State" then saved the value
  if (command == "F") Forward();  // check string then call a function or set a value
  else if (command == "B") Backward();
  else if (command == "R") TurnRight();
  else if (command == "L") TurnLeft();
  else if (command == "G") ForwardLeft();
  else if (command == "H") BackwardLeft();
  else if (command == "I") ForwardRight();
  else if (command == "J") BackwardRight();
  else if (command == "S") Stop();
  else if (command == "V") BeepHorn();
  else if (command == "W") TurnLightOn();
  else if (command == "w") TurnLightOff();
  else if (command == "0") SPEED = 60;
  else if (command == "1") SPEED = 70;
  else if (command == "2") SPEED = 81;                                                                     
  else if (command == "3") SPEED = 95;
  else if (command == "4") SPEED = 105;
  else if (command == "5") SPEED = 122;
  else if (command == "6") SPEED = 150;
  else if (command == "7") SPEED = 196;
  else if (command == "8") SPEED = 272;
  else if (command == "9") SPEED = 400;
  else if (command == "q") SPEED = 1023;
}

// function prototypes for HTTP handlers
void HTTP_handleRoot(void) {
  server.send(200, "text/html", "");  // Send HTTP status 200 (Ok) and send some text to the browser/client

  if (server.hasArg("State")) {
    Serial.println(server.arg("State"));
  }
}

void handleNotFound() {
  server.send(404, "text/plain", "404: Not found");  // Send HTTP status 404 (Not Found) when there's no handler for the URI in the request
}

// function to move forward
void Forward() {
  analogWrite(in1, SPEED);
  digitalWrite(in2, LOW);
  analogWrite(in3, SPEED);
  digitalWrite(in4, LOW);
}

// function to move backward
void Backward() {
  digitalWrite(in1, LOW);
  analogWrite(in2, SPEED);
  digitalWrite(in3, LOW);
  analogWrite(in4, SPEED);
}

// function to turn right
void TurnRight() {
  digitalWrite(in1, LOW);
  analogWrite(in2, SPEED);
  analogWrite(in3, SPEED);
  digitalWrite(in4, LOW);
}

// function to turn left
void TurnLeft() {
  analogWrite(in1, SPEED);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  analogWrite(in4, SPEED);
}

// function to move forward left
void ForwardLeft() {
  analogWrite(in1, SPEED);
  digitalWrite(in2, LOW);
  analogWrite(in3, SPEED / speed_Coeff);
  digitalWrite(in4, LOW);
}

// function to move backward left
void BackwardLeft() {
  digitalWrite(in1, LOW);
  analogWrite(in2, SPEED);
  digitalWrite(in3, LOW);
  analogWrite(in4, SPEED / speed_Coeff);
}

// function to move forward right
void ForwardRight() {
  analogWrite(in1, SPEED / speed_Coeff);
  digitalWrite(in2, LOW);
  analogWrite(in3, SPEED);
  digitalWrite(in4, LOW);
}

// function to move backward right
void BackwardRight() {
  digitalWrite(in1, LOW);
  analogWrite(in2, SPEED / speed_Coeff);
  digitalWrite(in3, LOW);
  analogWrite(in4, SPEED);
}

// function to stop motors
void Stop() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}

// function to beep a buzzer
void BeepHorn() {
  digitalWrite(buzPin, HIGH);
  delay(150);
  digitalWrite(buzPin, LOW);
  delay(80);
}

// function to turn on LED
void TurnLightOn() {
  digitalWrite(ledPin, HIGH);
}

// function to turn off LED
void TurnLightOff() {
  digitalWrite(ledPin, LOW);
}
oading WiFi_Controlled_ESP8266_Based_RC_Car__2_.ino…]()


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
