# Fire-Fighting Truck — Arduino

Autonomous 2-motor rover that patrols forward, stops on obstacle detection, and automatically triggers a firefighting response (pump + alarm) when it detects smoke or gas.

<p>
  <img alt="Arduino" src="https://img.shields.io/badge/Platform-Arduino-00979D?logo=arduino&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-yellow.svg">
</p>

## Features

- **Obstacle avoidance** — an HC-SR04 ultrasonic sensor continuously measures distance ahead; the rover drives forward until an obstacle comes within the threshold distance, then stops and lights an indicator LED.
- **Gas/smoke detection & auto-response** — an MQ2 sensor monitors for smoke/flammable gas. Crossing the threshold triggers the pump, a red LED, and a buzzer alarm; a green LED indicates a clear/safe reading.
- **2-motor differential drive** — dual H-bridge-driven DC motors with full forward/backward/turn control (`moveForward`, `moveBackward`, `turnLeft`, `turnRight`, `stopMotors`).

## Hardware

| Component | Purpose | Pin |
|---|---|---|
| Motor A (IN1/IN2/EN) | Left drive motor | 6 / 7 / 2 (PWM) |
| Motor B (IN3/IN4/EN) | Right drive motor | 8 / 3 / 10 (PWM) |
| HC-SR04 (Trig/Echo) | Obstacle distance | 9 / 11 |
| Obstacle LED | Proximity indicator | 12 |
| MQ2 gas sensor | Smoke/gas detection | A0 |
| Buzzer | Audible alarm | A1 |
| Green LED | Safe/clear indicator | A2 |
| Red LED | Alarm indicator | A3 |
| Pump (relay) | Firefighting response | A4 |

## How It Works

1. On each loop, the rover measures distance via the ultrasonic sensor. If clear (above the threshold), it drives forward; if an obstacle is within range, it stops and lights the obstacle LED.
2. In parallel, it reads the MQ2 gas sensor. If the reading crosses the configured threshold, it turns on the red LED, sounds the buzzer, and activates the pump for a fixed response window before re-checking.
3. When gas levels drop back below the threshold, the pump and alarm turn off and the green "clear" LED lights.

## Setup

1. Open `fire_fighting_truck.ino` in the Arduino IDE.
2. Wire the components per the pin table above.
3. Adjust `thresholdDistance` and `MAX` (gas threshold) in the sketch to match your sensors and desired sensitivity.
4. Upload to an Arduino Uno/Nano (or compatible board).

## License

MIT — see [LICENSE](LICENSE).
