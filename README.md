# ARDUINO-BASED-DIGITAL-IC-TESTER
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// LCD Address: 0x27, Size: 16x2
LiquidCrystal_I2C lcd(0x27, 16, 2);

// -------- Pin Definitions --------

// Inputs to IC
const int A_in = 2;
const int B_in = 3;

// Outputs from IC
const int AND_out = 4;   // IC 7408
const int OR_out  = 5;   // IC 7432
const int NOT_out = 6;   // IC 7404

// Push buttons
const int btn_AND = 7;
const int btn_OR  = 8;
const int btn_NOT = 9;

void setup() {

  // Configure pins
  pinMode(A_in, OUTPUT);
  pinMode(B_in, OUTPUT);

  pinMode(AND_out, INPUT);
  pinMode(OR_out, INPUT);
  pinMode(NOT_out, INPUT);

  pinMode(btn_AND, INPUT_PULLUP);
  pinMode(btn_OR, INPUT_PULLUP);
  pinMode(btn_NOT, INPUT_PULLUP);

  // LCD setup
  lcd.init();
  lcd.backlight();

  lcd.setCursor(0,0);
  lcd.print("DIGITAL IC");
  lcd.setCursor(0,1);
  lcd.print("TESTER READY");

  delay(2000);
  lcd.clear();
}

void loop() {

  lcd.setCursor(0,0);
  lcd.print("Select IC");

  if(digitalRead(btn_AND) == LOW) {
    testAND();
  }

  else if(digitalRead(btn_OR) == LOW) {
    testOR();
  }

  else if(digitalRead(btn_NOT) == LOW) {
    testNOT();
  }
}

//==================================================
// 7408 AND IC TEST
//==================================================
void testAND() {

  lcd.clear();
  lcd.print("Testing 7408");
  delay(1000);

  bool correct = true;

  int A[4] = {0,0,1,1};
  int B[4] = {0,1,0,1};

  int expected[4] = {0,0,0,1};

  for(int i=0;i<4;i++) {

    digitalWrite(A_in, A[i]);
    digitalWrite(B_in, B[i]);

    delay(20);

    int out = digitalRead(AND_out);

    if(out != expected[i]) {
      correct = false;
    }
  }

  lcd.clear();

  if(correct) {
    lcd.print("7408 PASS");
  }
  else {
    lcd.print("7408 FAIL");
  }

  delay(3000);
  lcd.clear();
}

//==================================================
// 7432 OR IC TEST
//==================================================
void testOR() {

  lcd.clear();
  lcd.print("Testing 7432");
  delay(1000);

  bool correct = true;

  int A[4] = {0,0,1,1};
  int B[4] = {0,1,0,1};

  int expected[4] = {0,1,1,1};

  for(int i=0;i<4;i++) {

    digitalWrite(A_in, A[i]);
    digitalWrite(B_in, B[i]);

    delay(20);

    int out = digitalRead(OR_out);

    if(out != expected[i]) {
      correct = false;
    }
  }

  lcd.clear();

  if(correct) {
    lcd.print("7432 PASS");
  }
  else {
    lcd.print("7432 FAIL");
  }

  delay(3000);
  lcd.clear();
}

//==================================================
// 7404 NOT IC TEST
//==================================================
void testNOT() {

  lcd.clear();
  lcd.print("Testing 7404");
  delay(1000);

  bool correct = true;

  int A[2] = {0,1};
  int expected[2] = {1,0};

  for(int i=0;i<2;i++) {

    digitalWrite(A_in, A[i]);

    delay(20);

    int out = digitalRead(NOT_out);

    if(out != expected[i]) {
      correct = false;
    }
  }

  lcd.clear();

  if(correct) {
    lcd.print("7404 PASS");
  }
  else {
    lcd.print("7404 FAIL");
  }

  delay(3000);
  lcd.clear();
}
