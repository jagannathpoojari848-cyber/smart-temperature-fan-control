# Temperature Based Fan Speed Control (ESP32 + DHT + L298N)

Automatic fan speed control system that reads ambient temperature and drives a DC fan through an L298N motor driver. The fan stays off in normal conditions and speeds up proportionally as temperature rises, reaching full speed at a defined high-temperature threshold.

## Features
- Real-time temperature sensing with DHT11/DHT22
- PWM-based variable fan speed control (not just ON/OFF)
- Adjustable temperature thresholds and PWM range
- Serial monitor logging of temperature and fan speed %

## Hardware Used
- ESP32 Dev Board
- DHT11 / DHT22 temperature sensor
- L298N motor driver module
- DC fan
- External power supply for the motor

See [`docs/wiring.md`](docs/wiring.md) for the full connection diagram.

## How It Works
1. The DHT sensor is read every 2 seconds.
2. If temperature is below `TEMP_LOW` (25°C by default), the fan is off.
3. If temperature is at/above `TEMP_HIGH` (35°C by default), the fan runs at full PWM (255).
4. Between these two thresholds, fan speed increases linearly with temperature.
5. Fan speed is set on the L298N's `ENA` pin using ESP32's LEDC PWM peripheral; `IN1`/`IN2` are fixed so the fan spins in a single direction.

## Getting Started

### Requirements
- [Arduino IDE](https://www.arduino.cc/en/software) with ESP32 board support installed
- Library: **DHT sensor library** by Adafruit (install via Library Manager)
- Library: **Adafruit Unified Sensor** (dependency of the above)

### Setup
1. Clone this repository.
2. Open `TemperatureFanControl.ino` in the Arduino IDE.
3. Select your ESP32 board and correct COM port.
4. In the sketch, set `DHTTYPE` to `DHT11` or `DHT22` depending on your sensor.
5. Adjust `TEMP_LOW`, `TEMP_HIGH`, `PWM_MIN`, and `PWM_MAX` if needed.
6. Upload and open the Serial Monitor at 115200 baud to see live readings.

## Configuration Reference
| Constant     | Default | Meaning                                  |
|--------------|---------|-------------------------------------------|
| `TEMP_LOW`   | 25.0°C  | Below this, fan is off                    |
| `TEMP_HIGH`  | 35.0°C  | At/above this, fan runs at full speed     |
| `PWM_MIN`    | 120     | Minimum PWM that reliably spins the fan   |
| `PWM_MAX`    | 255     | Full-speed PWM                            |

## Possible Improvements
- Add an OLED/LCD display to show live temperature and fan status
- Add humidity-based logic (DHT provides humidity too)
- Push readings to a cloud dashboard (e.g., ThingSpeak) for logging
- Add hysteresis to avoid rapid speed changes near threshold temperatures

## License
MIT License — feel free to use and modify.
