# Wireless Notice Board using Arduino and Bluetooth Module

## Project Overview
This project demonstrates a simple wireless notice board system using an Arduino, a Bluetooth module (HC-05/HC-06), and a 16x2 LCD display. The notice board displays messages received wirelessly via Bluetooth from a smartphone or any Bluetooth-enabled device. This project is particularly useful for displaying dynamic content remotely, such as announcements or important updates.

## Components Required
- Arduino Uno or Nano
- Bluetooth Module (HC-05 or HC-06)
- 16x2 LCD Display
- Jumper wires
- Breadboard
- Power supply

## Circuit Diagram
1. **Bluetooth Module (HC-05/HC-06) to Arduino:**
   - **VCC** to **5V**
   - **GND** to **GND**
   - **TXD** to **RX** (D2)
   - **RXD** to **TX** (D3)

2. **16x2 LCD to Arduino:**
   - **RS** to **Digital Pin 4**
   - **EN** to **Digital Pin 5**
   - **D4** to **Digital Pin 6**
   - **D5** to **Digital Pin 7**
   - **D6** to **Digital Pin 8**
   - **D7** to **Digital Pin 9**
   - **VCC** to **5V**
   - **GND** to **GND**

## Code Explanation
The code is designed to read messages sent via Bluetooth and display them on a 16x2 LCD screen. The message will scroll across the screen if it's longer than 16 characters.

### Key Parts of the Code:
- **LiquidCrystal.h**: Manages communication between the Arduino and the LCD display.
- **SoftwareSerial.h**: Allows the Arduino to communicate with the Bluetooth module via software serial communication on digital pins 2 (RX) and 3 (TX).
- **mySerial.readString()**: Reads the incoming Bluetooth message as a string.
- **lcd.setCursor()**: Sets the starting point for displaying the message on the LCD.
- **val.trim()**: Trims any unnecessary whitespace from the received message.

### Code Structure:
- **Setup Function**: Initializes the LCD and serial communication. It also displays a welcome message.
- **Loop Function**: Continuously checks for incoming messages via Bluetooth and updates the display. The message scrolls across the LCD if it changes.

### Arduino Code:
```cpp
#include <LiquidCrystal.h>
#include <SoftwareSerial.h>

LiquidCrystal lcd (4, 5, 6, 7, 8, 9);
SoftwareSerial mySerial (2, 3);   //(RX, TX);

String val = "No Data";
String oldval;
String newval = "No Data";
int i = 0;

void setup() 
{
  // put your setup code here, to run once:
  lcd.begin(16,2);
  mySerial.begin(9600);
  Serial.begin(9600);
  lcd.setCursor(0, 0);
  lcd.print("Wireless Notice");
  lcd.setCursor(0, 1);
  lcd.print("     Board     ");
  delay(3000);
  lcd.clear();
  lcd.print("Welcome!");
}

void loop() 
{
  val = mySerial.readString();
  val.trim();
  Serial.println(val);
  if(val != oldval)
  {
    newval = val;
  }
  lcd.clear();
  lcd.setCursor(i, 0);
  lcd.print(newval);
  i++;
  if(i >= 15)
  {
    i = 0;
  }
  val = oldval;
}


