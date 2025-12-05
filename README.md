# Hackpack
Hackpad I designed to hopefully run doom on
PCB
Here's a picture of my schematic! I wired a xiao rp204 module to a TFT display, 2 LEDs, a and 4 switches, 

Schematic	PCB

[ x ] I ran DRC in KiCad and have made sure there are 0 errors!
<img width="761" height="625" alt="Screenshot 2025-12-04 202734" src="https://github.com/user-attachments/assets/03f9dc96-f870-4407-81ef-4e6a6fcf04de" />


CAD Model:
Everything fits together using 4 M3 Heatset inserts and 4 M3x16mm screws. Here's how it looks:
<img width="644" height="486" alt="Screenshot 2025-12-04 202718" src="https://github.com/user-attachments/assets/d00c3ba7-c258-4f84-8f20-d819dd3fb843" />

It was made in fusion360 pretty nifty stuff.

Firmware overview
The firmware for this is written in micropython. it is supposed to run doom
#include <Arduino.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// Pin map (adjust to match your PCB)
#define PIN_SW1 26 // D0
#define PIN_SW2 27 // D1
#define PIN_SW3 28 // D2
#define PIN_SW4 29 // D3
#define OLED_SDA 6 // D4
#define OLED_SCL 7 // D5

// Display config
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

void setupPins() {
  pinMode(PIN_SW1, INPUT_PULLUP);
  pinMode(PIN_SW2, INPUT_PULLUP);
  pinMode(PIN_SW3, INPUT_PULLUP);
  pinMode(PIN_SW4, INPUT_PULLUP);
}

bool pressed(uint8_t pin) { return digitalRead(pin) == LOW; }

void drawFrame(uint8_t playerX, uint8_t playerY) {
  display.clearDisplay();
  // Simple “Doom-ish” demo: player as a box and walls
  display.drawRect(0, 0, SCREEN_WIDTH, SCREEN_HEIGHT, SSD1306_WHITE);
  display.fillRect(playerX, playerY, 6, 6, SSD1306_WHITE);
  // Fake walls
  display.drawLine(20, 10, 100, 10, SSD1306_WHITE);
  display.drawLine(25, 20, 95, 25, SSD1306_WHITE);
  display.drawLine(30, 35, 90, 45, SSD1306_WHITE);
  display.display();
}

void setup() {
  // If your board needs explicit Wire pins:
  Wire.setSDA(OLED_SDA);
  Wire.setSCL(OLED_SCL);
  Wire.begin();
  delay(50);

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    // Try 0x3D if your OLED uses that address
    for(;;) {}
  }

  setupPins();
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(2, 2);
  display.print("Doom By Aum");
  display.display();
  delay(800);
}

void loop() {
  static uint8_t x = SCREEN_WIDTH/2, y = SCREEN_HEIGHT/2;
  const uint8_t speed = 2;

  if (pressed(PIN_SW1)) x = (x > speed) ? x - speed : 0;      // Left
  if (pressed(PIN_SW2)) x = (x + speed < SCREEN_WIDTH-6) ? x + speed : SCREEN_WIDTH-6; // Right
  if (pressed(PIN_SW3)) y = (y > speed) ? y - speed : 0;      // Up
  if (pressed(PIN_SW4)) y = (y + speed < SCREEN_HEIGHT-6) ? y + speed : SCREEN_HEIGHT-6; // Down

  drawFrame(x, y);
  delay(16); // ~60 FPS target
}


BOM
All prices must be in USD.

Provided by dari // alex:

1x xiao rp-2040
1x 0.91 inch OLED displays display WITHOUT FEMALE HEADERS.
1x 3D printed case
Purchasing from HQ:

1x xiao at $4.30/per. I will be losing $1from my grant as a result.
I will be sourcing the following parts with my grant:

1x PCB from JLCPCB
$2 for 5x + $1.50 shipping
4x Blank DSA keycaps (LCSC) $0.44 (min order 5)
4x MX-Style switches
1x0.91 inch OLED displays
4x M3x16mm screws
4x M3x5mx4mm heatset inserts

I own a 3d printed and will print my own case
