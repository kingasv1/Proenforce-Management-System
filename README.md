# Proenforce-Management-System
full automation of public transport and pro-enforcement of traffic rules
#include <SoftwareSerial.h>
#include<EEPROM.h>

SoftwareSerial mySerial(9, 10); // RX, TX
char a;
String e = "21007B8E4D99";
String c = "21007BCAAA3A";
String d = "21007BA7B74A";
String b = "21007B60467C";
String temp="KL03A4321";
String f, h;

int swich = 0;
int motx = 6;
int moty = 5;
int buzzer = 11;
int led = 13;
int vgnd=8;
int gas=7;
int mercury=4;
int z, w, q, r, x, xbee;




void setup()
{
  Serial.begin(9600);
  mySerial.begin(9600);
  //pinMode(swich, INPUT);
  pinMode(motx, OUTPUT);
  pinMode(moty, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(vgnd,OUTPUT);
  digitalWrite(vgnd,LOW);
  digitalWrite(moty, 0);
  analogWrite(motx, 0);
  digitalWrite(buzzer, 0);
  z = 1;
  w = 2;
  q = 20;
  r = 2;
  x = 0;
  xbee = 0;
}

//==============================================================





void loop()
{


  f = "";
  int i = 0;
  while (mySerial.available())
  {
    a = mySerial.read();
    f.concat(a);
    if (i == 11)
    {
      if (f == b)   //-----------------------------Good Licence
      {
        q = 0;
        r = r + 1;
        if (r % 2 == 0)
        {
          q = 2;
        }


      }
      else if (f == e)   //-------------------------BAD Licence
      {
        if (xbee = 10)
        {
          x = 255;
        }
        else
        {
          x = 0;
        }

        q = 1;
        analogWrite(motx, x);
        for (int m = 1; m <= 3; m++)
        {
          digitalWrite(buzzer, 1);
          delay(500);
          digitalWrite(buzzer, 0);
          delay(500);
        }
      }
    }
    i++;
  }


  if (q == 0)
  {
    analogWrite(motx, 255);
    if (analogRead(swich) >= 50) //--------------------------Testing for switch acitvation
    {
      digitalWrite(led, 1);
      delay(2);
      w = 2;
      z = z + 1;
    }
    //--------------------------------------------------------------

    if (z % 2 == 1) //---------------------------Speed mode - 1st case
    {
      if ((f == c) || (f == d))
      {
        w = w + 1;
      }

      if (w % 2 == 1)
      {
        digitalWrite(buzzer, 1);
        analogWrite(motx, 255);
      }
      if (w % 2 == 0)
      {
        digitalWrite(buzzer, 0);
        //----------------------------------------akhil bro do
      }
    }

    //--------------------------------------------------------

    if (z % 2 == 0) //------------------------Slow mode - 2nd case
    {
      if ((f == c) || (f == d))
      {
        w = w + 1;
      }

      if (w % 2 == 1)
      {
        analogWrite(motx, 150);
      }
      if (w % 2 == 0)
      {
        analogWrite(motx, 255);
        //---------------------------------------akhil bro do
      }
    }
  }
  if (q == 2)
  {
    analogWrite(motx, 0);
    delay(1000);
  }
  delay(200);
}
