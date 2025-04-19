// Pins for Motor A
const int motorAIn1 = 6;
const int motorAIn2 = 7;
const int motorAEnable = 2; // PWM pin for Motor A

// Pins for Motor B
const int motorBIn3 = 8;
const int motorBIn4 = 3;
const int motorBEnable = 10; // PWM pin for Motor B

// Pins for Ultrasonic Sensor
const int trigPin = 9;
const int echoPin = 11;
const int ledPin = 12; // LED to indicate obstacle proximity

// Pins for Input and Indicators (LEDs, Buzzer)
int Input = A0;             // MQ2 sensor input
int Buzzer = A1;
int GreenLED = A2;
int RedLED = A3;
int Pump = A4;              // Pump control pin

// Threshold values
const int thresholdDistance = 5;  // Adjust this value for testing
const int MAX = 600;             // MQ2 gas sensor threshold

void setup() {
  Serial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);

  pinMode(Input, INPUT);
  pinMode(RedLED, OUTPUT);
  pinMode(GreenLED, OUTPUT);
  pinMode(Buzzer, OUTPUT);
  pinMode(Pump, OUTPUT);          // Pump control

  pinMode(motorAIn1, OUTPUT);
  pinMode(motorAIn2, OUTPUT);
  pinMode(motorAEnable, OUTPUT);
  pinMode(motorBIn3, OUTPUT);
  pinMode(motorBIn4, OUTPUT);
  pinMode(motorBEnable, OUTPUT);

  stopMotors();
  digitalWrite(Pump, LOW); // Ensure pump is off at start
}

void loop() {
  long distance = getDistance();

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance > 0) {
    if (distance < thresholdDistance) {
      digitalWrite(ledPin, HIGH);
      stopMotors();
    } else {
      digitalWrite(ledPin, LOW);
      moveForward(100);
    }
  } else {
    digitalWrite(ledPin, LOW);
    stopMotors();
  }

  // Read gas sensor value
  int value = analogRead(Input);
  Serial.print("Gas Sensor Value: ");
  Serial.println(value);

  if (value >= MAX) {
    digitalWrite(GreenLED, LOW);
    digitalWrite(RedLED, HIGH);
    digitalWrite(Buzzer, HIGH);
    digitalWrite(Pump, HIGH); // Turn on pump
    delay(5000);
    digitalWrite(Buzzer, LOW);
    delay(6000);
  } else {
    digitalWrite(RedLED, LOW);
    digitalWrite(Buzzer, LOW);
    digitalWrite(Pump, LOW); // Turn off pump
    digitalWrite(GreenLED, HIGH);
  }

  delay(100);
}

long getDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duration = pulseIn(echoPin, HIGH);
  long distance = (duration * 0.0344) / 2;

  if (distance > 0 && distance < 400) {
    return distance;
  } else {
    return -1;
  }
}

void moveForward(int speed) {
  digitalWrite(motorAIn1, HIGH);
  digitalWrite(motorAIn2, LOW);
  analogWrite(motorAEnable, speed);

  digitalWrite(motorBIn3, HIGH);
  digitalWrite(motorBIn4, LOW);
  analogWrite(motorBEnable, speed);
}

void stopMotors() {
  digitalWrite(motorAIn1, LOW);
  digitalWrite(motorAIn2, LOW);
  analogWrite(motorAEnable, 0);

  digitalWrite(motorBIn3, LOW);
  digitalWrite(motorBIn4, LOW);
  analogWrite(motorBEnable, 0);
}

void moveBackward(int speed) {
  digitalWrite(motorAIn1, LOW);
  digitalWrite(motorAIn2, HIGH);
  analogWrite(motorAEnable, speed);

  digitalWrite(motorBIn3, LOW);
  digitalWrite(motorBIn4, HIGH);
  analogWrite(motorBEnable, speed);
}

void turnLeft(int speed) {
  digitalWrite(motorAIn1, LOW);
  digitalWrite(motorAIn2, HIGH);
  analogWrite(motorAEnable, speed);

  digitalWrite(motorBIn3, HIGH);
  digitalWrite(motorBIn4, LOW);
  analogWrite(motorBEnable, speed);
}

void turnRight(int speed) {
  digitalWrite(motorAIn1, HIGH);
  digitalWrite(motorAIn2, LOW);
  analogWrite(motorAEnable, speed);

  digitalWrite(motorBIn3, LOW);
  digitalWrite(motorBIn4, HIGH);
  analogWrite(motorBEnable, speed);
}
