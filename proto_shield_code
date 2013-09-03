//Simple sketch to try out each component on the proto shield board
//To run a sub-routine that exercises particular component
//press the number then enter
//pressing any other key then enter will stop that sub routine
//and return to main loop


#include <SoftwareSerial.h>

//Define variables
int incomingByte = 0;	//place to put all incoming serial
int pclk = 3;		//pot clock pin 3
int pdata = 4;		//pot data pin 4
int prst = 2;		//pot reset pin 2
int cclr = 7;		//counter clear pin 7
int cclk = 8;		//counter clock pin 8
int anald = 10;		//DAC LD pin 10
int anaclk = 11;	//DAC clock pin 11
int anasdi = 12;	//DAC data pin 12
int anaclr = 13;	//DAC clear pin 13

//The following arrays are data sets for the pot and DAC
int potarray[17]={0,0,1,0,0,1,0,0,0,0,0,0,0,0,0,0,0};
int anaarray[13]={0,0,0,0,0,0,0,0,0,0,0,0,0};

//More variables
int i = 0;
int analogpin = 0;	//Analog input pin for temp sensor
int val = 0;

//setup 
void setup()
{
  Serial.begin(9600); // open the serial port at 9600 hps:

//Set all digital pins used to be outputs
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(11, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(13, OUTPUT);

  analogReference(INTERNAL);	//Set analog reference to be internal 1.1v

//Preset some digital values
  digitalWrite(prst, LOW);
  digitalWrite(anaclr, LOW);
  digitalWrite(cclr, HIGH);
  
}
void loop()

//check for anything on serial and compare to values
//this calls different sub routines for control
{
if (Serial.available() > 0){
    incomingByte = Serial.read();

 if (incomingByte == 49){		//to get a 49 press 1 then enter
    pot();
}

 else if (incomingByte == 50){		//to get a 50 press 2 then enter
    count();
}

 else if (incomingByte == 51){		//to get a 51 press 3 then enter
    anaout();
}  

 else if (incomingByte == 52){		//to get a 52 press 4 then enter
    temp();
}
}
}

//Pot control sub routine
void pot(void){
    Serial.println("pot run");	//Serial print outputs are used for debugging
    digitalWrite(prst, HIGH);	//Pot reset high - pin is active low
    delay(1);
    while(Serial.available() <= 0){	//do this until another serial input is captured
      int n = 8;
        for(int v = 0; v < 9; v++){	//this loop changes the 1 from lsb to msb one bit at a time
          Serial.println("setting pot value");
          potarray[n] = 1;
          potarray[n+1] = 0;
          for(int a = 0; a < 18; a++){	//this loops writes all the bits out to set the pot
            Serial.println("Sending pot value");
            digitalWrite(pdata, potarray[a]);
            delay(1);
            digitalWrite(pclk, HIGH);
            delay(1);
            digitalWrite(pclk, LOW);
            delay(1);
          }
          digitalWrite(prst, LOW);	//latch the pot value
          Serial.println("holding pot value");
          delay(2000);			//hold for 2 seconds
          n = n-1;         		//change the array value location pointer
        }
}
digitalWrite(prst, LOW);		//serial input captured end this routine
Serial.println("pot change done");
return;
}

//counter control sub routine
void count(void){
    Serial.println("counting");
    digitalWrite(cclr, LOW);		//set clear pin low
    delay(1);
    while(Serial.available() <= 0){	//do this until another serial input is captured
        //Serial.println("counting up");	//serial here commented out so delay isnt too long
        digitalWrite(cclk, HIGH);	//set clock pin high
        delay(1);
        digitalWrite(cclk, LOW);	//set clock pin low count advances on falling edge
        //Serial.println("holding count");
        delay(1);
}
digitalWrite(cclr, HIGH);		//serial input captured clear counter and end routine
Serial.println("counting done");
return;
}

//DAC control sub routine
void anaout(void){
    Serial.println("analog out");
    //set the outputs ready for write value
    digitalWrite(anaclk, HIGH);
    delay(1);
    digitalWrite(anaclr, HIGH);
    delay(1);
    digitalWrite(anald, HIGH);
    delay(1);
    while(Serial.available() <= 0){	//do this until another serial input is captured
      int n = 11;
        for(int v = 0; v < 12; v++){	//this loop changes the bit from lsb to msb one at a time
          Serial.println("sending value");
          anaarray[n] = 1;
          anaarray[n+1] = 0;
          for(int a = 0; a < 13; a++){	//this loop sends the digital data
            Serial.println("sending value ");
            digitalWrite(anasdi, anaarray[a]);
            delay(1);
            digitalWrite(anaclk, LOW);	//clock data in bit by bit
            delay(1);
            digitalWrite(anaclk, HIGH);
            delay(1);
          }
          digitalWrite(anald, LOW);	//all bits in activate output
          Serial.println("analog hold");
          delay(500);			//hold for half a second
          digitalWrite(anald, HIGH);
          delay(1);
          n = n-1;			//next bit pointer
        }
        digitalWrite(anaclr, LOW);	//clear output
        delay(1);
        digitalWrite(anaclr, HIGH);
        delay(1);
      }
digitalWrite(anald, LOW);		//serial input captured end routine
delay(1);
Serial.println("analog out done");
return;
}

//analog temperature sampler
//in here the conversion mathematics can be done
void temp(void){
    Serial.println("temp get");
    while(Serial.available() <= 0){	//do this until another serial input is captured
        val = analogRead(analogpin);	//read the pin put it in val
        Serial.println(val);		//output val to serial
}
Serial.println("temp grab done");	//serial input captured end routine
return;
}
