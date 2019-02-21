
#include "PWM.h"

unsigned int newFreqA,newFreqB, frequencyA = 100;
unsigned int frequencyB= 100;
int PINA = 9;
int PINZ = 3;

// period of pulse accumulation and serial output, milliseconds
#define MainPeriod 500
long previousMillis = 0; // will store last time of the cycle end
volatile unsigned long duration = 0; // accumulates pulse width
volatile unsigned int pulsecount = 0;
volatile unsigned long previousMicros = 0;

void setup() {
  Serial.begin(19200);
  attachInterrupt(0, myinthandler, RISING);

  InitTimersSafe();
  bool success = SetPinFrequencySafe( PINA, frequencyA);
  pwmWrite( PINA, 127);
  
  success = SetPinFrequencySafe( PINZ, frequencyB);
  if (success)
  { pinMode(13, OUTPUT);
    digitalWrite(13, HIGH);
    pwmWrite( PINZ, 127);
  }

}

void loop() {
  // put your main code here, to run repeatedly:

  unsigned long currentMillis = millis();
  if (currentMillis - previousMillis >= MainPeriod)
  {
   newFreqA = frequencyA + (analogRead(A0) / 4) * 100 ;
    if (SetPinFrequencySafe( PINA, newFreqA))
    {
      pwmWrite( PINA, 127);
    }
    newFreqB = frequencyB + (analogRead(A1) / 4) * 100 ;
    if (SetPinFrequencySafe( PINZ, newFreqB))
    {
      pwmWrite( PINZ, 127);
    }
    previousMillis = currentMillis;
    // need to bufferize to avoid glitches
    unsigned long _duration = duration;
    unsigned long _pulsecount = pulsecount;
    duration = 0; // clear counters
    pulsecount = 0;
    float Freq = 1e6 / float(_duration);
    Freq *= _pulsecount; // calculate F
    // output time and frequency data to RS232
    //Serial.print(currentMillis);
    Serial.print(" "); // separator!
    Serial.println(Freq);
    //Serial.print(" ");
    //Serial.print(_pulsecount);
    //Serial.print(" ");
    //Serial.println(_duration);
  }
}

void myinthandler() // interrupt handler
{
  unsigned long currentMicros = micros();
  duration += currentMicros - previousMicros;
  previousMicros = currentMicros;
  pulsecount++;
}


