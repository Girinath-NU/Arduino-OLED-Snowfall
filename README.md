# Christmas Tree with Snowfall on OLED Display 🎄❄️

This project uses an SSD1306 OLED display to create a simple animation of a Christmas tree with falling snowflakes. The project is implemented using an Arduino-compatible microcontroller and the Adafruit SSD1306 and GFX libraries.

## Features

- A festive Christmas tree displayed on an OLED screen.
- Snowflakes falling from the top of the screen in a loop.
- Real-time animation with adjustable snowfall speed.

## Hardware Requirements

1. SSD1306 OLED Display (I2C interface)
2. Arduino-compatible microcontroller (e.g., Uno, Nano, ESP32, etc.)
3. Jumper wires and breadboard for connections

## Connections

| OLED Pin | Arduino Pin (Uno/Nano) | ESP32 Pin   |
|----------|-------------------------|-------------|
| VCC      | 5V                     | 3.3V        |
| GND      | GND                    | GND         |
| SCL      | A5                     | GPIO 22     |
| SDA      | A4                     | GPIO 21     |

## Software Requirements

1. **Arduino IDE**: Download and install from the [official website](https://www.arduino.cc/en/software).
2. **Adafruit SSD1306 Library**: Install via the Arduino Library Manager.
3. **Adafruit GFX Library**: Install via the Arduino Library Manager.

## Code

```cpp
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Number of snowflakes
const int numFlakes = 20;
int flakeX[numFlakes], flakeY[numFlakes];

void setup() {
  display.begin(0x3C, 0x3C);
  display.clearDisplay();
  display.display();

  // Initialize random positions for snowflakes
  for (int i = 0; i < numFlakes; i++) {
    flakeX[i] = random(0, SCREEN_WIDTH);
    flakeY[i] = random(0, SCREEN_HEIGHT);
  }
}

void loop() {
  display.clearDisplay();

  // Draw Christmas tree (simple triangle shape)
  display.fillTriangle(64, 10, 50, 40, 78, 40, SSD1306_WHITE);
  display.fillTriangle(64, 25, 55, 50, 73, 50, SSD1306_WHITE);

  // Draw snowflakes
  for (int i = 0; i < numFlakes; i++) {
    display.drawPixel(flakeX[i], flakeY[i], SSD1306_WHITE);
    flakeY[i] += 1; // Move flake down

    // Reset snowflake if it moves off the screen
    if (flakeY[i] > SCREEN_HEIGHT) {
      flakeY[i] = 0;
      flakeX[i] = random(0, SCREEN_WIDTH);
    }
  }

  display.display();
  delay(100); // Adjust speed of snowfall
}
