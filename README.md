
//Here is the code, by which my plants watering robot is working.
#include <AFMotor.h>                          //For adding the library of shield
#include<Servo.h>                             //For adding the library of servo motor
Servo m1;                                     //Variable of servo motor 1
Servo m2;                                     //variable of servo motor 2
int pos;                                      //Variable that will define the postion of servos
const int trigPin = A0;                       //Defines the trig pin of ultrasonic sensor 1st 
const int echoPin = A1;                       //Defines the echo pin of ultrasonic sensor 1st
const int trigPin1=A2;                        //Defines the trig pin of ultrasonic sensor 2nd 
const int echoPin1=A3;                        //Defines the echo pin of ultrasonic sensor 2nd
int mpin = A4;                                //Defines the pin of moisture sensor pin
int mout;                                     //Variable to store the value given by moisture sensor
long duration, duration1;                     //Variable that stores the duration value given by ultrasonic sensor
int distance, distance1;                      //Variable that stores the distance value calculated by the formaula


AF_DCMotor motor1(1, MOTOR12_1KHZ);           //Defines the frequency which will be given to motor 1
AF_DCMotor motor2(2, MOTOR12_1KHZ);           //Defines the frequency which will be given to motor 2

void setup() {
  Serial.begin(9600);                         //starts serial communication with the arduino and PC
  pinMode(trigPin, OUTPUT); 
  pinMode(echoPin, INPUT);
  pinMode(trigPin1, OUTPUT); 
  pinMode(echoPin1, INPUT);
  m1.attach(10);                             //Define the attached pin for servo motor 1
  m2.attach(9);                              //Define the attached pin for servo motor 1

  m1.write(0);
  m2.write(120);
  
  motor1.setSpeed(255);                      //To set the particular speed of motor 1
  motor2.setSpeed(255);                      //To set the particular speed of motor 2
}

void loop() 
{
 
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
duration = pulseIn(echoPin, HIGH);
distance= duration*0.034/2;
digitalWrite(trigPin1, LOW);
delayMicroseconds(2);
digitalWrite(trigPin1, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin1, LOW);
duration1 = pulseIn(echoPin1, HIGH);
distance1= duration1*0.034/2;


  if (distance>=15)
 {
//   Serial.println(distance);
   
   Forward();
   delay(100);
 }


 else if(distance<15)

{
//  Serial.println("0 is fine");
//  delay(10);
  
  Stop();
  delay(70);
  
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1= duration1*0.034/2;

 //5
 int Entry1=0;
  if(distance1>=6)
  {
   servoF();
   delay(100);
  // Entry1=1;
  }
}
   
   mout=analogRead(mpin);
  
//   Serial.print("Sensor value is  ");
//   Serial.println(mout);
//   delay(1000);

   
  if(distance1<6 && mout<=500)
  
{
//  Serial.println("1 is fine");
//  delay(100);
  
  servoB();
  delay(100);

  Forward();
  delay(1000);
  
}
//Serial.println(distance1);

  if(distance1<6 && mout>500)
 
 {
  
//  Serial.println("2 is fine");
//  Serial.println(mout);
//  Serial.println(distance1);
//  
//  delay(100);

  
 while(mout>500)
 {
  
//  Serial.print("Sensor value is");
//  Serial.println(mout);
//  delay(100);
  
  motor2.run(FORWARD);

  mout=analogRead(mpin);

  if(mout<=500)
  {
  motor2.run(RELEASE);
  delay(100);
  break;
  }
  
 }
 }
 }
void Stop()

{
  
//  Serial.println("stop is fine");
//  delay(100);
  
  motor1.run(RELEASE);
  
}

void Forward()

{

 
//  Serial.println("Forward is fine");
//  delay(100);
  
  motor1.run(FORWARD);
  
}
void servoF()
{
  
  Serial.println("servo forward is fine");
  delay(100);
  
for (pos=0;pos<=120;pos+=1)

{
 
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1= duration1*0.034/2;

 
 if(distance1>6)
  {
    m1.write(pos);
    m2.write(120-pos);
  }

}

}

void servoB()
{
  Serial.println("Servo back is fine");
  delay(100);
  
  for(pos=120;pos>=0;pos-=1)
  {
    {
    m1.write(pos);
    m2.write(120-pos);
    }
    delay(30);
  }
}                              
