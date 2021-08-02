#include <LiquidCrystal.h>;

//define custom characters for display
byte degree[8] = {
	0b00110,
	0b01001,
	0b01001,
	0b00110,
	0b00000,
	0b00000,
	0b00000,
	0b00000
};

byte thermometer[8] = {
    0b00100,
    0b01010,
    0b01010,
    0b01110,
    0b01110,
    0b11111,
    0b11111,
    0b01110,
};

byte arrow[] = {
  B00100,
  B01110,
  B11111,
  B01110,
  B01110,
  B11111,
  B01110,
  B00100
};



//define upper and lower bounds for hysteresis and its value
 bool hu = false;
 bool hl = true;
const int hystereze = 2;

//define display
LiquidCrystal lcd(12,11,5,4,3,2);

//define inputs
const int temp = A0;
const int plus =9;
const int minus = 10;

//define temperature related variables
int tempF;
int tempC;
int tempVal;
int tempSetF;
int tempSet = 24;


//define outputs
const int ledUp = 13;
const int ledDown = 7;


//timer variables
const int increment = 1;
const int delayMillis = 250;
int lastMillis;
int currMillis;

//button debounce variables

int buttonState;             // the current reading from the input pin
int lastButtonState = LOW; 

int buttonState2;            // the current reading from the input pin
int lastButtonState2 = LOW;

// the previous reading from the input pin
unsigned long lastDebounceTime = 0; 
unsigned long lastDebounceTime2 = 0;

// the last time the output pin was toggled
unsigned long debounceDelay = 10; 




void setup()
{
  
  lcd.createChar(1, thermometer);
  lcd.createChar(0, degree);
  lcd.createChar(3, arrow);
  
  lcd.begin(16, 2);
  
 pinMode(ledDown, OUTPUT);
 pinMode(ledUp, OUTPUT);
 pinMode(plus, INPUT);
 pinMode(minus, INPUT);
 pinMode(temp, INPUT);
 Serial.begin(9600);  
}





void loop()
{
  
//write all values onto display
 lcd.home();
 lcd.write((uint8_t)1);
 lcd.setCursor(1, 0);
 lcd.print(" :");
 lcd.setCursor(3,0);
 lcd.print(tempC);
 lcd.print(" ");
 lcd.write((uint8_t)0);
 lcd.print("C / ");
 lcd.print(tempF);
 lcd.print(" ");
 lcd.write((uint8_t)0);
 lcd.print("F");
 lcd.setCursor(0, 1);
 lcd.write((uint8_t)1);
 lcd.write((uint8_t)3);
 lcd.print(":");
 lcd.print(tempSet);
 lcd.print(" ");
 lcd.write((uint8_t)0);
 lcd.print("C / ");
 lcd.print(tempSetF);
 lcd.print(" ");
 lcd.write((uint8_t)0);
 lcd.print("F");
  
//initiate counting of elapsed time
currMillis = millis();

//read sensor value
tempVal = analogRead(temp);

//transfer sensor values into desired ones
tempC = map(tempVal, 20, 357, -40, 125);
tempF = (tempC * 9 / 5) + 32;
tempSetF = (tempSet * 9 / 5) + 32;
  
//read button state
int reading = digitalRead(plus);
int reading2 = digitalRead(minus);
  

  //if button state changes
  if (reading != lastButtonState) {
    
    // reset the debouncing timer
    lastDebounceTime = millis();
  
  }
  

   if ((millis() - lastDebounceTime) > debounceDelay) {
     
    if (reading != buttonState) {
      buttonState = reading;
      
      
      if (buttonState == HIGH) {
        
     tempSet++;
     Serial.println(tempSet);
        
      }
    }
     
   }
  
  
     if (reading2 != lastButtonState2) {
    // reset the debouncing timer
    lastDebounceTime2 = millis();
  
  }

  if ((millis() - lastDebounceTime2) > debounceDelay) {
    // whatever the reading is at, it's been there for longer
    // than the debounce delay, so take it as the actual current state:

    // if the button state has changed:
    if (reading2 != buttonState2) {
      buttonState2 = reading2;
      

      // only toggle the LED if the new button state is HIGH
      if (buttonState2 == HIGH) {
      
          tempSet--;
    Serial.println(tempSet);
      
      }
    }
    }
  
 //if output is off and temperature exceeds desired value, turn on
   if(hl == true && tempC > tempSet)
  {
    
  digitalWrite(ledUp, 1);
  digitalWrite(ledDown,!digitalRead(ledUp));
    
  hl = false;
    
  }
  //if value exceeds upper bound of hysteresis, stay on and let program know
  //this happened
  else if((hl == false || digitalRead(ledUp) == HIGH )&& tempC > (tempSet + hystereze))
  {
   
  digitalWrite(ledUp, 1);
  digitalWrite(ledDown,!digitalRead(ledUp));
  
  hu = true;
  
  
  }
  //if value exceeded upper bound of hysteresis (previous if statement),
  // turn off at set value
  else if(hu == true && tempC < tempSet)
  {
  digitalWrite(ledUp, 0);
  digitalWrite(ledDown,!digitalRead(ledUp));
    
   hu = false;
  }
  //if value didn't exceed hysteresis (stayed close to set value), 
  //turn off at lower bound of hysteresis, so not to cause flickering
  else if(hu == false && tempC < (tempSet - hystereze))
  {
   digitalWrite(ledUp, 0);
   digitalWrite(ledDown,!digitalRead(ledUp));
    
    hl = true;
  }
 
  
 //write values onto Serial monitor with delay
if(currMillis - lastMillis >= delayMillis ){
  
  
Serial.println("CURRENT TEMP: ");
Serial.print(tempC);
Serial.println(" °C");
  
Serial.print(tempF);
Serial.println(" °F");
  
Serial.println("SET TEMP: ");
Serial.print(tempSet);
Serial.println(" °C");

lastMillis = currMillis;
 
 }
 //reset last state of button
 lastButtonState = reading;
 lastButtonState2 = reading2;
 }
