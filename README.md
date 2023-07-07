# Quicktoll
A motion sensing quick toll system based on Arduino Uno with almost negligible wait time
Whenever the vehicle enters the toll gate, the RFID reader will check whether the RFID tag is valid or not, if the card is valid it will allow the vehicle to move and deduct the toll tax from the user’s  account automatically. If the user doesn’t have sufficient balance, a message will be sent to his mobile saying “insufficient balance” or “card invalid”.

![image](https://github.com/manashpd/Quicktoll/assets/138905143/bf9feb3d-5a28-46dd-9eaf-b29009e1e0e1)
The ETC systems, unlike a manual system do not involve a collector. The vehicle when arrives at the booth it does not need to stop at the booth like it had to do in manual system. It directly passes away without giving cash to anyone. Thus, we can call it a cashless toll collection.
The ETC system involves various components which work together and form the whole system. As the vehicle arrives at the booth a sensor which has been incorporated at the booth, senses the tag or a card embedded in the vehicle. This tag is known as a RFID card. This RFID card has a unique identity for every user and thus has the information regarding the user. The system reads the card and authenticates the person to pass through the toll after deducting a fixed amount from the user's bank account. The payment in the ETC system is made through wireless mode. There is an antenna at the tollgate which establishes a wireless connection with the on-board device when the vehicle's RFID card is sensed and thus, automatically deducts the amount from the account. There is no need for the vehicles to stop and wait for the process to happen. It occurs in few seconds and thus, it is a very effective and a fast system.




Arduino Code used for the project :


#include <Servo.h>

const int trigPin = 2;       // Trig pin for ultrasonic sensor
const int echoPin = 3;       // Echo pin for ultrasonic sensor
const int servoPin = 9;      // Servo pin

Servo gateServo;             // Servo object

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  gateServo.attach(servoPin);

  Serial.begin(9600);

  closeGate();  // Initially, close the gate
}

void loop() {
  unsigned long duration = measureDistance();
  float distance = calculateDistance(duration);

  if (distance <= 10) {
    openGate();               // Open the gate
    delay(2000);              // Allow time for vehicle passage (adjust as needed)
    closeGate();              // Close the gate
  }

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(100);  // Delay between each loop iteration
}

unsigned long measureDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  return pulseIn(echoPin, HIGH);
}

float calculateDistance(unsigned long duration) {
  float speedOfSound = 0.034;  // cm/microsecond (approximate value)

  float distance = (speedOfSound * duration) / 2;  // Divide by 2 for one-way distance

  return distance;
}

void openGate() {
  gateServo.write(90);  // Set the servo position to open the gate
  Serial.println("Gate opened");
}

void closeGate() {
  gateServo.write(0);  // Set the servo position to close the gate
  Serial.println("Gate closed");
}






                                      





