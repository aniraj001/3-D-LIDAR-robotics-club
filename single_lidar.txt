#include <NewPing.h>
#include <Servo.h>

#define TRIGGER_PINL  A5  // Arduino pin tied to trigger pin on ping sensor.
#define ECHO_PINL     A2  // Arduino pin tied to echo pin on ping sensor.

#define MAX_DISTANCE 500 // Maximum distance we want to ping for (in centimeters). Maximum sensor distance is rated at 400-500cm.

Servo myservo1;// create servo object to control a servo
Servo myservo2;// twelve servo objects can be created on most boards

int angle1 = 0;    // variable to store the servo position
int angle2 = 0;
//for UltraSonic Sensor
NewPing sonarLeft(TRIGGER_PINL, ECHO_PINL, MAX_DISTANCE); // NewPing setup of pins and maximum distance.

unsigned int pingSpeed = 30; // How frequently are we going to send out a ping (in milliseconds). 50ms would be 20 times a second.
unsigned long pingTimer;     // Holds the next ping time


float oldLeftSensor, leftSensor, lSensor;
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  myservo1.attach(9);  // attaches the servo on pin 9 to the servo object
  myservo2.attach(10);
}

void loop() {
  // put your main code here, to run repeatedly:
  for (angle2 = 0; angle2 <= 90; angle2 += 10){
    
    myservo2.write(angle2);
    if((angle2/10)%2 == 0){
      for(angle1 = 0; angle1 <= 180; angle1 += 1){
        myservo1.write(angle1);
        ReadSensors();
          Serial.print(leftSensor);
          Serial.print(" ");
          Serial.print(angle1);
          Serial.print(",");
          Serial.print(angle2);
          Serial.println(";");
          
          delay(5);
    }}
    else{
      for(angle1 = 180; angle1 >= 0; angle1 -= 1){
        myservo1.write(angle1);
        ReadSensors();
          Serial.print(leftSensor);
          Serial.print(" ");
          Serial.print(angle1);
          Serial.print(",");
          Serial.print(angle2);
          Serial.println(";");
          
          delay(5);
    } 
      }
    }
}
  //========================================ULTRASONIC SENSORS READING DATA========================================//

void ReadSensors() {


  leftSensor = sonarLeft.convert_cm(leftSensor);

  lSensor = sonarLeft.ping_cm(); //ping in cm

  leftSensor = (lSensor + oldLeftSensor) / 2; //average distance between old & new readings to make the change smoother

}
