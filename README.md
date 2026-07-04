# Fire-Fighting Truck — Arduino Firmware

An autonomous 2-motor rover built around an ultrasonic obstacle sensor and an MQ2 gas/smoke sensor: it drives forward until something blocks its path, and if it detects smoke/gas above a threshold it stops, sounds an alarm, and switches on a water pump.

## Features

- **Obstacle avoidance** — an HC-SR04 ultrasonic sensor measures distance continuously; the truck drives forward and stops (lighting an indicator LED) when an obstacle is closer than `thresholdDistance`.
- **Fire/smoke detection** — an MQ2 gas sensor is polled every loop; once its reading crosses `MAX`, the truck lights a red LED, sounds a buzzer, and activates a pump for a few seconds before re-checking.
- **Status indicators** — separate green/red LEDs show "clear" vs. "gas detected" state.
- **Manual driving primitives** — `moveForward`, `moveBackward`, `turnLeft`, `turnRight`, and `stopMotors` are implemented and ready to wire up to a remote control or additional logic; only `moveForward`/`stopMotors` are currently called from `loop()`.

## Hardware / Wiring

| Signal                  | Arduino Pin | Notes                                   |
|-------------------------|:-----------:|------------------------------------------|
| Motor A – IN1           | 6           | L298N (or similar) driver, Motor A       |
| Motor A – IN2           | 7           |                                            |
| Motor A – Enable (PWM)  | 5           | Must be a PWM-capable pin (see Fixes)     |
| Motor B – IN3           | 8           | Motor B                                   |
| Motor B – IN4           | 3           |                                            |
| Motor B – Enable (PWM)  | 10          | Must be a PWM-capable pin                 |
| Ultrasonic – Trig       | 9           | HC-SR04                                   |
| Ultrasonic – Echo       | 11          |                                            |
| Obstacle LED            | 12          | Lights when an obstacle is within range   |
| MQ2 gas sensor (analog) | A0          |                                            |
| Buzzer                  | A1          |                                            |
| Green LED (clear)       | A2          |                                            |
| Red LED (gas detected)  | A3          |                                            |
| Pump (via relay/MOSFET) | A4          | Do not drive a pump directly off a digital pin — use a relay or MOSFET rated for the pump's current |

On an Arduino Uno/Nano, only pins **3, 5, 6, 9, 10, 11** support `analogWrite()` PWM.

## Fixes in this revision

- **Motor A's enable pin was moved from pin 2 to pin 5.** Pin 2 is not PWM-capable on the Uno/Nano, so the original `analogWrite(motorAEnable, speed)` call could not actually vary Motor A's speed — it behaved as an on/off signal instead of true speed control. Pin 5 is a genuine PWM pin and isn't used elsewhere in the sketch.
- Sketch moved into a folder matching its filename (`fire_fighting_truck/fire_fighting_truck.ino`) so it opens correctly as an Arduino sketch.

## Getting started

1. Open `fire_fighting_truck/fire_fighting_truck.ino` in the Arduino IDE.
2. Wire the board per the pin table above.
3. Select your board (Uno/Nano/compatible) and port, then upload.
4. Open the Serial Monitor at 9600 baud to see live distance and gas sensor readings.

## Tuning

- `thresholdDistance` (default `5` cm) — how close an obstacle needs to be before the truck stops.
- `MAX` (default `600`) — MQ2 analog reading threshold that triggers the alarm/pump. MQ2 readings vary by sensor and warm-up time; calibrate against your own sensor before relying on this for a real fire scenario.

## License

MIT — see [LICENSE](LICENSE).

## Disclaimer

This is a hobby/educational project, not a certified fire-suppression device. Don't rely on it as a safety system.
