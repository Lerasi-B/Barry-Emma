
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27,16,2);

int L1 = 2;
int L2 = 3;
int L3 = 4;
int L4 = 5;

int Red = 8;
int Yellow = 9;
int Green = 10;
int Blue = 11;

int buzzer = 12;

void setup()
{
  pinMode(L1, INPUT);
  pinMode(L2, INPUT);
  pinMode(L3, INPUT);
  pinMode(L4, INPUT);

  pinMode(Red, OUTPUT);
  pinMode(Yellow, OUTPUT);
  pinMode(Green, OUTPUT);
  pinMode(Blue, OUTPUT);

  pinMode(buzzer, OUTPUT);

  lcd.init();
  lcd.backlight();

  Serial.begin(9600);
}

void loop()
{
  bool p1 = digitalRead(L1);
  bool p2 = digitalRead(L2);
  bool p3 = digitalRead(L3);
  bool p4 = digitalRead(L4);

  digitalWrite(Red, p1);
  digitalWrite(Yellow, p2);
  digitalWrite(Green, p3);
  digitalWrite(Blue, p4);

  lcd.clear();

  if(p4)
  {
    lcd.setCursor(0,0);
    lcd.print("Tank FULL");
    lcd.setCursor(0,1);
    lcd.print("Level:100%");
    digitalWrite(buzzer,HIGH);
  }
  else
  {
    digitalWrite(buzzer,LOW);

    if(p3)
    {
      lcd.setCursor(0,0);
      lcd.print("Level:75%");
    }
    else if(p2)
    {
      lcd.setCursor(0,0);
      lcd.print("Level:50%");
    }
    else if(p1)
    {
      lcd.setCursor(0,0);
      lcd.print("Level:25%");
    }
    else
    {
      lcd.setCursor(0,0);
      lcd.print("Tank Empty");
      lcd.setCursor(0,1);
      lcd.print("Level:0%");
    }
  }

  Serial.print("25%=");
  Serial.print(p1);
  Serial.print(" 50%=");
  Serial.print(p2);
  Serial.print(" 75%=");
  Serial.print(p3);
  Serial.print(" 100%=");
  Serial.println(p4);

  delay(500);
}
