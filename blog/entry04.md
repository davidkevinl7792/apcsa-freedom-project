# Entry 4
##### 5/1/24

After a long time of improvising and making the movement mechanic for the slicer to get to it's destination, I was finally able to do it with the help of this [video](https://www.youtube.com/watch?v=YU17L650k3s) to understand how to control the rotation of a dc motor. My entire code came out to be like this on [tinkercad](https://www.tinkercad.com/things/4s5I2YapnTg-dazzling-luulia-inari?sharecode=kAdRuS3yJFK39LdAcJQ4zbFGMXdLbcXQBJVlo_xg68g):
```java
#include <Servo.h>
const int motorPin = 9;
boolean condition = true;
const int a = 12;
const int b = 13;
const int c = 11;
const int d = 10;
int pos = 0;
int i;
Servo servo_9;
Servo servo_6;
void setup()
{
  servo_9.attach(9);
  servo_6.attach(8);
  pinMode(a, OUTPUT);
  pinMode(b, OUTPUT);
  pinMode(c, OUTPUT);
  pinMode(d, OUTPUT);
  Serial.begin(9600);  
}


void loop()
{
}
void order(double length, int slice){
  for (pos = 90; pos >= 0; pos -= 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      servo_6.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  double per = length/slice;
  for(int i = 0; i<slice; i++){
    right(per);
    cut();
  }
  clears(length);
}
void clears(double length){ // clears all chopped food
  left(length);
  for (pos = 0; pos <= 90; pos += 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  right(length);
  for (pos = 90; pos >= 0; pos -= 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      servo_6.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  left(length);
}
void left(double length){
  int i = length;
  double rem;
  int del = 1000; //assuming 1 second = 1 inch of distance
  while(i>0){
    if(i<1){
      del = length*1000;
    }
    else if(i < 2 && i != 1){
     rem = abs(i-length);
      del = del + rem*100;
      i--;
   }
  	digitalWrite(a,LOW);
  	digitalWrite(b,HIGH);
  	digitalWrite(c,LOW);
 	  digitalWrite(d,HIGH);
    delay(del);
    i--;
  }
  digitalWrite(a,LOW);
  digitalWrite(b,LOW);
  digitalWrite(c,LOW);
  digitalWrite(d,LOW);
}    
void right(double length){
  int i = 0;
  double rem;
  int del = 1000; //assuming 1 second = 1 inch of distance
  while(i<length){
    if(length<1){
      del = length*1000;
    }
    else if(i-length > -2 && i-length != -1){
     rem = abs(i-length);
      del = del + rem*100;
      i++;
   }
  	digitalWrite(a,HIGH);
  	digitalWrite(b,LOW);
  	digitalWrite(c,HIGH);
 	  digitalWrite(d,LOW);
    delay(del);
    i++;
  }
  digitalWrite(a,LOW);
  digitalWrite(b,LOW);
  digitalWrite(c,LOW);
  digitalWrite(d,LOW);
}
void cut(){
  //holder down
  for (pos = 0; pos <= 90; pos += 1) {
      // tell servo to go to position in variable 'pos'
      servo_6.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  //cutter
    for (pos = 0; pos <= 90; pos += 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
    for (pos = 90; pos >= 0; pos -= 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  //holder up
  for (pos = 90; pos >= 0; pos -= 1) {
      // tell servo to go to position in variable 'pos'
      servo_6.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
   }
}
```
The methods I used to move the slicer is `left()` and `right()`. They both are pretty identical, however their condtions are just reversed and they turn differently. I had a huge challenge converting the distance to the amount of times the motor should spin. Since I didn't have the motor yet, I just assumed that if it spun for one second, it would move the slicer one inch. So I started off with these lines of code:
```java
void right(double length){
  int i = 0;
  double rem;
  int del = 1000; //assuming 1 second = 1 inch of distance
  while(i<length){
  	digitalWrite(a,HIGH); //motor 1
  	digitalWrite(b,LOW); //motor 1
  	digitalWrite(c,HIGH); //motor 2
 	  digitalWrite(d,LOW); //motor 2
    delay(del);
    i++;
  }
  digitalWrite(a,LOW); //motor 1
  digitalWrite(b,LOW); //motor 1
  digitalWrite(c,LOW); //motor 2
  digitalWrite(d,LOW); //motor 2
}
```
While the slicer is inching it's way there, the motors are always turning right due to the `digitalWrite()` and `delay()`. It prevents them from stopping so it's always a smooth transition and once they get to their destination, it's the end of the `while()` loop and it stops. Each motor has 2 out pins and when they have different status, they move in a direction and when they have the same status, they stop. However, not all length of distance are whole numbers, some could be decimals. If you were to split something with a length of 12.5 inches and the motor has to move 6.25 inches, it would cause unevenness. To counteract that I added a condition to check for decimals with the following code:
```java
if(i-length > -2 && i-length != -1){
     rem = abs(i-length);
      del = del + rem*100;
      i++;
   }
```
I checked it 2 units behind so just in case I was like 1.59 inches away, I could add on extra time to the `delay ()` and end break the loop as soon as it hit the other `i++`. I also had to incorporate `i-length != -1` so that if it's perfectly a whole number, it wouldn't be mixed in as if it was a decimal. Then I also had to take in consideration what happens if the length was less than an inch. So I added another condition in front of it:
```java
if(length<1){
      del = length*1000;
    }
```
This overwrites the current `delay()` to something smaller than it. I made some adjustments to the slicer. Instead of slicing it all at once, I made the slicer slice once at a time with the following code:
```
void cut(){
  //holder down
  for (pos = 0; pos <= 90; pos += 1) {
      // tell servo to go to position in variable 'pos'
      servo_6.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  //cutter
    for (pos = 0; pos <= 90; pos += 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
    for (pos = 90; pos >= 0; pos -= 1) {
      // tell servo to go to position in variable 'pos'
      servo_9.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
    }
  //holder up
  for (pos = 90; pos >= 0; pos -= 1) {
      // tell servo to go to position in variable 'pos'
      servo_6.write(pos);
      // wait 15 ms for servo to reach the position
      delay(15); // Wait for 15 millisecond(s)
   }
}
```
I switched from using dc motors to servo motors for the cutter because I didn't want a blade spinning a full 360 and as a person, you just bring it up and chop at a 90 degree angle. Instead of using one servo motor for the cutting, I used two so one can hold the food while the other chops. For this method I had the holder go down first, then the slicer does it's cut, then the holder goes back up. Some special method I add to organize this are `order()` and `clears()`. `Order()` takes in the length and amount of slice(s) and does the math to find the length for `right()`. After `right()` finishes, it would ask for a `cut()` and it repeats the process until it's done with it's order and ask everything to be cleared. That's when `clears()` comes in and it basically goes all the way back to the beginning to push it all aside to prepare for the next order.

Currently I'm on the fifth step of my engineering design process (EDP) where I figured out how to move the slicer and a bit of improvising to create this prototype. I would move on to the sixth step of my EDP where I would start to buy these components and test out how they go. However, I still need to do some research on more components to connect the arduino to the website. The two skills I used were problem decomposition and logical reasoning. I used problem decompositioning to break down which parts I need like in order to move, I would need left and right. I also used logical reasoning to think of some possible flaws when it comes to rotating the motors.

[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)
