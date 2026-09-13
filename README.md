# A-Smart-Parking-Barrier
Automaton in Gate Open And Close

For a Smart Parking Barrier, I have made a project in Tinkercad, which I have attached the link to here. It detects an approaching vehicle using an ultrasonic sensor and sends the signal to an Arduino Uno. The Arduino then sends a signal to the servo motor to open the gate. I have also added a display, programmable strip light, and buzzer.

Link For Tinkercad Project : https://www.tinkercad.com/things/8MVDVlKI2Wy-smart-parking-barrier?sharecode=2yz7DyUGI7sQicLfNNkuFBY-ueiuygSm82ee3J_jiHk

Link For Demo video : https://drive.google.com/file/d/1OX9NQDNaZ217pFm9kKdUbC3gISmVN-0o/view?usp=drive_link

#############################################################################################################################

Code:

// Made By Harimithran K
#include <LiquidCrystal_I2C.h>
#include <Servo.h>
#include <Adafruit_NeoPixel.h>


#define trig 2
#define echo 4
#define led 8
#define PIN 3	 
#define NUMPIXELS 10

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);
LiquidCrystal_I2C lcd(0x20,16,2);
Servo gate;

void setup()
{
  Serial.begin(9600);
  
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);
  pinMode(led,OUTPUT);
  gate.attach(9);
  
  lcd.init();
  lcd.clear();         
  lcd.backlight();   
  lcd.setCursor(0, 0);
  lcd.print("AUTOMATIC GATE");
  pixels.begin();
  
}
// Made By Harimithran K
void loop()
{
  
  digitalWrite(trig,LOW);
  delayMicroseconds(2);
  digitalWrite(trig,HIGH);
  delayMicroseconds(10);
  digitalWrite(trig,LOW);

  long t =pulseIn(echo,HIGH);
  long cm = t /29 / 2;

  Serial.print(cm);
  Serial.println("cm");

  if (cm <=10){
    digitalWrite(led, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Gate OPEN       ");
    for (int i=0; i < NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(0, 255, 0));
    pixels.show();
    delay(100);
    }  
    
  }
  else{
    digitalWrite(led,LOW);
    lcd.setCursor(0, 1);
    lcd.print("Gate Closed       ");
    for (int i=0; i < NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(255, 0, 0));
    pixels.show();
    
    }  
    
  }
    gate.write(180);

  if (cm<= 10){

    for( int j=180;j>=90;j--){
      gate.write(j);
      delay(50);
    }

    delay(2500);
    
    for( int i=90;i<=180;i++){
    gate.write(i);
    delay(50);
  }
  }
  else{
    gate.write(180);
  }
  
}

#############################################################################################################################

I also made an automatic gate-opening project using a QR code with an ESP32-CAM. I made this project for my sister for her college project when I was in 10th grade. I will provide the project video link below.

Link Of Gate Open Using QR Code : https://drive.google.com/file/d/16KC2om9YHV5EzEXrAgKa_djseTrB4iHV/view?usp=sharing


#############################################################################################################################
