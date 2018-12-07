# PIANO
//We always have to include the library
#include "LedControl.h"

/*
  Now we need a LedControl to work with.
 ***** These pin numbers will probably not work with your hardware *****
  pin 12 is connected to the DataIn
  pin 11 is connected to the CLK
  pin 10 is connected to LOAD
  We have only a single MAX72XX.
*/
LedControl lc = LedControl(12, 11, 10, 1);
int row; 
int PinC = 2;   // define the different digital pins according to the push of each musical note
int PinD = 3;
int PinE = 4;
int PinF = 5;
int PinG = 6;
int PinA = 7;
int PinB = 8;
int PinCc = 9;

int buzz = 13;  //define digital pin of the buzzer

int C, D, E, F, G, A, B, Cc;  //Save HIGH or LOW states of each push-buttons

//define which leds light up, depending on the musical note

byte NOTE_C[8] = {B00000000, B00011000, B00100100, B00100000, B00100000, B00100100, B00011000, B00000000};
byte NOTE_D[8] = {B00000000, B00111000, B00100100, B00100100, B00100100, B00100100, B00111000, B00000000};
byte NOTE_E[8] = {B00000000, B00111100, B00100000, B00111000, B00100000, B00100000, B00111100, B00000000};
byte NOTE_F[8] = {B00000000, B00111100, B00100000, B00111000, B00100000, B00100000, B00100000, B00000000};
byte NOTE_G[8] = {B00000000, B00011000, B00100100, B00100000, B00101100, B00100100, B00011000, B00000000};
byte NOTE_A[8] = {B00000000, B00011000, B00100100, B00100100, B00111100, B00100100, B00100100, B00000000};
byte NOTE_B[8] = {B00000000, B00111000, B00100100, B00111000, B00100100, B00100100, B00111000, B00000000};
byte NOTE_Cc[8] = {B00000001, B00011001, B00100100, B00100000, B00100000, B00100100, B00011000, B00000000};
byte NOTHING[8] = {B00000000, B00000000, B00000000, B00000000, B00000000, B00000000, B00000000, B00000000};


void setup() {
  /*
    The MAX72XX is in power-saving mode on startup,
    we have to do a wakeup call
  */
  lc.shutdown(0, false);
  /* Set the brightness to a medium values */
  lc.setIntensity(0, 8);
  /* and clear the display */
  lc.clearDisplay(0);

  pinMode(PinC, INPUT);    //set the pins as an INPUT
  pinMode(PinD, INPUT);
  pinMode(PinE, INPUT);
  pinMode(PinF, INPUT);
  pinMode(PinG, INPUT);
  pinMode(PinA, INPUT);
  pinMode(PinB, INPUT);
  pinMode(PinCc, INPUT);
}


void drawSymbol(byte* symbol) {//draw the musical notes
  for (int i = 0; i < 8; i++) {
    lc.setRow(0, i, symbol[i]);
  }
}

void loop()
{
  C = digitalRead(PinC);  //state HIGH or LOW depending on the variable 
  D = digitalRead(PinD);
  E = digitalRead(PinE);
  F = digitalRead(PinF);
  G = digitalRead(PinG);
  A = digitalRead(PinA);
  B = digitalRead(PinB);
  Cc = digitalRead(PinCc);

  if (C == LOW) {       //When I press my button, it acquires status 1 and complies with the condition, causing us to enter the TONE function
    tone(buzz, 261);  //With the TONE function We will sound the buzzer at a certain frequency. BUZZ, Frequency
    drawSymbol(NOTE_C);
  }

  else if (D == LOW) {
    tone(buzz, 293);
    drawSymbol(NOTE_D);
  }
  else if (E == LOW) {
    tone(buzz, 329);
    drawSymbol(NOTE_E);
  }
  else if (F == LOW) {
    tone(buzz, 349);
    drawSymbol(NOTE_F);
  }
  else if (G == LOW) {
    tone(buzz, 392);
    drawSymbol(NOTE_G);
  }
  else if (A == LOW) {
    tone(buzz, 440);
    drawSymbol(NOTE_A);
  }
  else if (B == LOW) {
    tone(buzz, 493);
    drawSymbol(NOTE_B);
  }
  else if (Cc == LOW) {
    tone(buzz, 523);
    drawSymbol(NOTE_Cc);
  }

  else {            //If there is nothing pressed, the buzzer wont produce any sound
    noTone(buzz);

    drawSymbol(NOTHING); //If no button is being pressed, there will be nothing written down in the ledboard. 
  }

  delay(100);
}
