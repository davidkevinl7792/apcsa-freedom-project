# Entry 1
##### 11/12/23

  I was deciding between using [flutter](https://flutter.dev/) to create a website that stores recipes and sends information to a hardware to prepare ingredients or [arduino](https://www.arduino.cc/) to create the hardware itself. In the end, I decided to go with arduino to get some hands on activity and coding on the computer at the same time. I started by searching up where the software is to code for the hardware; which they had their own [ide](https://create.arduino.cc/editor) for me to use. I started by trying to create something simple which was making a led blub blink. I followed the [SIK guide](https://learn.sparkfun.com/tutorials/sik-experiment-guide-for-arduino---v33/experiment-1-blinking-an-led) and was able to get it blinking by copy and pasting this code:
```java
void setup()
{
  pinMode(13, OUTPUT);
}

void loop()
{
  digitalWrite(13, HIGH);   // Turn on the LED
  delay(1000);              // Wait for one second
  digitalWrite(13, LOW);    // Turn off the LED  
  delay(1000);              // Wait for one second
}
```
I was confused why the number `13` was included inside of `digitalWrite()`. So I change the number to a random number to see what would happen and it stopped working. I wondered what that number was for and then realized that it was the number that the wire was connected to from the breadboard to the arduino uno. I also tested if I could make the delay longer and if it could stop since it would never stop. So I changed the code into this:
```java
void setup()
{
  pinMode(13, OUTPUT);
}

void loop()
{
  int count = 0;
    if(count <= 3){
    digitalWrite(13, HIGH);   // Turn on the LED
    delay(2000);              // Wait for one second
    digitalWrite(13, LOW);    // Turn off the LED  
    delay(2000);              // Wait for one second
    count++;
  }
}
```
I was able to make the led bulb switch on for a extra second longer and it only blinked three times this time. Next, I tried to see if I switch some placements on the breadboard like change the wire's position to connect to the negative side of the led and see if it works. Turns out positioning does matter since when I ran it again, it didn't blink at all since I put the wire in the wrong place. For my future plans, I would try to create a turning motor next and see if I could attach a blade on it to slice stuff.

  Currently I'm on the first step of my engineering design process (EDP) which was to define the problem and figure out what tool I wanted to use. I would be moving on to my second step of my EDP to research the problem and research more into my tool. The two skills I used were embracing failure and how to read. I had to learn how to read the physical copy of the SIK guide and I learned how to embrace failure when I learned that positioning matters and I can't place things randomly.

[Next](entry02.md)

[Home](../README.md)
