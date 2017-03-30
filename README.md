# Smart-Obstacle-Avoiding-Robot
Its a bot with ultrasonic sensor used to avoid obstacle.
int mot1=2,mot2=3,mot3=4,mot4=5;
int echoPin1=13,trigPin1=12;
#include <SoftwareSerial.h>// import the serial library
SoftwareSerial intelli_bot(10, 11); // RX, TX
int BluetoothData; // the data transmitted over Bluetooth
void setup() {
intelli_bot.begin(9600);
Serial.begin(9600);
pinM ode(mot1,OUTPUT);
pinM ode(mot2,OUTPUT);
pinM ode(mot3,OUTPUT);
pinM ode(mot4,OUTPUT);
pinM ode(echoPin1,INPUT);
pinM ode(trigPin1,OUTPUT);
}
void loop() {
long duration1, distance1;
if (intelli_bot.available()){
BluetoothData=intelli_bot.read();
// by default manual mode
if(BluetoothData=='s'){ // reverse
digitalWrite(mot1,1);
digitalWrite(mot2,0);
digitalWrite(mot3,1);
digitalWrite(mot4,0);
}
else if (BluetoothData=='w'){ // forward
digitalWrite(mot1,0);
digitalWrite(mot2,1);
digitalWrite(mot3,0);
digitalWrite(mot4,1);
}
else if (BluetoothData=='a'){ // left
digitalWrite(mot1,1);
digitalWrite(mot2,0);
digitalWrite(mot3,0);
digitalWrite(mot4,1);
}
else if (BluetoothData=='d'){ // right
digitalWrite(mot1,0);
digitalWrite(mot2,1);
digitalWrite(mot3,1);
digitalWrite(mot4,0);
}
else if(BluetoothData=='q')
{ //begin intelligent node
for(;;){
if (intelli_bot.available()){
BluetoothData=intelli_bot.read();
if(BluetoothData=='p'){break;}
}
digitalWrite(trigPin1,LOW);
delayM icroseconds(2);
digitalWrite(trigPin1,HIGH);
delayM icroseconds(10);
digitalWrite(trigPin1,LOW);
duration1 = pulseIn(echoPin1,HIGH);
distance1 = (duration1/2) / 29.1;
Serial.println(distance1);
delay(300);
if(distance1<=20)
{ //if obstacle distance is <=20cm move right for 1.2 sec
digitalWrite(mot1,0);
digitalWrite(mot2,1);
digitalWrite(mot3,1);
digitalWrite(mot4,0);
delay(1200);
}
digitalWrite(mot1,1); // default move forward
digitalWrite(mot2,0);
digitalWrite(mot3,1);
digitalWrite(mot4,0);
}
}
//end of intelligent mode
else { // do nothing
digitalWrite(mot1,0);
digitalWrite(mot2,0);
digitalWrite(mot3,0);
digitalWrite(mot4,0);
}
}
delay(10);// prepare for next data ...
}
