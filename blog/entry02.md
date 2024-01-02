# Entry 2
##### 1/1/24

I started to try to play with the motor and get it to work in the case of being able to slice up food. I used the [SIK guide](https://learn.sparkfun.com/tutorials/sik-experiment-guide-for-arduino---v33/experiment-12-driving-a-motor) and was able to get the basic starting code for driving a motor.
``` java
const int motorPin = 9;  // Connect the base of the transistor to pin 9.
                         // Even though it's not directly connected to the motor,
                         // we'll call it the 'motorPin'

void setup()
{
  pinMode(motorPin, OUTPUT);  // set up the pin as an OUTPUT
  Serial.begin(9600);         // initialize Serial communications
}


void loop()
{ // This example basically replicates a blink, but with the motorPin instead.
  int onTime = 3000;  // milliseconds to turn the motor on
  int offTime = 3000; // milliseconds to turn the motor off

  analogWrite(motorPin, 255); // turn the motor on (full speed)
  delay(onTime);                // delay for onTime milliseconds
  analogWrite(motorPin, 0);  // turn the motor off
  delay(offTime);               // delay for offTime milliseconds

  // Uncomment the functions below by taking out the //. Look below for the
  // code examples or documentation.

  // speedUpandDown(); 
  // serialSpeed();
}

// This function accelerates the motor to full speed,
// then decelerates back down to a stop.
void speedUpandDown()
{
  int speed;
  int delayTime = 20; // milliseconds between each speed step

  // accelerate the motor
  for(speed = 0; speed <= 255; speed++)
  {
    analogWrite(motorPin,speed);    // set the new speed
    delay(delayTime);               // delay between speed steps
  }
  // decelerate the motor
  for(speed = 255; speed >= 0; speed--)
  {
    analogWrite(motorPin,speed);    // set the new speed
    delay(delayTime);               // delay between speed steps
  }
}


// Input a speed from 0-255 over the Serial port
void serialSpeed()
{
  int speed;

  Serial.println("Type a speed (0-255) into the box above,");
  Serial.println("then click [send] or press [return]");
  Serial.println();  // Print a blank line

  // In order to type out the above message only once,
  // we'll run the rest of this function in an infinite loop:

  while(true)  // "true" is always true, so this will loop forever.
  {
    // Check to see if incoming data is available:
    while (Serial.available() > 0)
    {
      speed = Serial.parseInt();  // parseInt() reads in the first integer value from the Serial Monitor.
      speed = constrain(speed, 0, 255); // constrains the speed between 0 and 255
                                        // because analogWrite() only works in this range.

      Serial.print("Setting speed to ");  // feedback and prints out the speed that you entered.
      Serial.println(speed);

      analogWrite(motorPin, speed);  // sets the speed of the motor.
    }
  }
}
```
I ran the code on [tinkercad](https://www.tinkercad.com/) and it worked perfectly fine. However, I do find this a bit excessive for starter code. So I found the area where I needed to focus on which was the `setup()` and the `loop()` because the other methods are used to set the motor's rotation speed. I don't need to set the rotation speed from a user input or change the speed of rotation since slicing food should be a constant motion. After I removed the unnecessary code, I wrote some pseudo code to envision how I would code the slicing mechanism to the point where it would be fine slicing any amount of time. I wrote this pseudo code:
``` java
void setup()
{
  pinMode(motorPin, OUTPUT);  // set up the pin as an OUTPUT
  Serial.begin(9600);         // initialize Serial communications
}


void loop()
{
  int onTime = # of time it takes to do a full rotation;  // milliseconds to turn the motor on
  int offTime = # of time it takes to travel to the distance to do it's full rotation; // milliseconds to turn the motor off

  for(int i = 0; i < # of slices;i++){
  analogWrite(motorPin, 255); // turn the motor on (full speed)
  delay(onTime);                // delay for onTime milliseconds
  analogWrite(motorPin, 0);  // turn the motor off
  delay(offTime);               // delay for offTime milliseconds
  }
}
```
I realize that when I filled the numbers in assuming I know how much slices I want and all the delay timing. The amount of slicing kept increasing; passing the number of slices it should've stopped. I realized that I should just put that all in the `setup()` and I needed a condition to when it recieves a request from the flutter website, then it should run. So I fixed my code and it looked like this:
``` java
void setup()
{
  pinMode(motorPin, OUTPUT);  // set up the pin as an OUTPUT
  Serial.begin(9600);         // initialize Serial communications
  int onTime = 3000;  // milliseconds to turn the motor on
  int offTime = 3000; // milliseconds to turn the motor off
  boolean condition = false;
  if(condition = true){
  	for(int i = 0; i < 3;i++){
  	analogWrite(motorPin, 255); // turn the motor on (full speed)
  	delay(onTime);                // delay for onTime milliseconds
  	analogWrite(motorPin, 0);  // turn the motor off
  	delay(offTime);               // delay for offTime milliseconds
  	} 
    condition = false;
  }
}
```
I switched the starting `condition` to true and it worked perfectly fine as the motor only rotated 3 times assuming it takes 3 seconds to rotate and 3 seconds to move. Over the winter break I decided to spend 10 minutes each day to try to find ways to move the slicing mechanism to it's destination and find some inspiration from maybe a claw machine or something small that can move in a 2D plane.

Currently I'm on the second step of my engineering design process (EDP) which was to research the problem of how I solve the slicing mechanic. I would be staying on my second step of my EDP to research the problem, but this time it would be moving the mechanic to it's destination. The two skills I used were embracing failure and how to read. I had to learn how to read the physical copy of the SIK guide and I learned how to embrace failure when I learned that positioning matters and I can't place things randomly. The two skills I used were to learn how to read and logical reasoning. I had to use to learn how to read in order to cut down some code that were unnecessary and not pretend that I needed the whole starter code even though some of them were not in use. I also had to use logical reasoning to create my pseudo code in order to create the actual code.

[Previous](entry01.md) | [Next](entry03.md)

[Home](../README.md)
