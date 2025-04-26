#include <Wire.h>
#include <Adafruit_MPR121.h>

// =================== HARDWARE SETTINGS =================== //
#define SDA_PIN 21
#define SCL_PIN 22

// LEDs
const int exploreLedPin = 4;   
const int testLedPin = 5;      

// =================== OBJECT INITIALIZATION =================== //
Adafruit_MPR121 cap1 = Adafruit_MPR121(); // 0x5A

// =================== VARIABLES =================== //
int currentQuestion = 0;
int score = 0;

String stateNames[] = {
  "Jammu & Kashmir","Ladakh","Maharashtra","Odisha","Uttrakhand","Tamil Nadu",
  "Uttar Pradesh","Rajasthan","Gujarat","Madhya Pradesh","Bihar","Karnataka"
};

String quizQuestions[] = {
  "Which state is famous for Taj Mahal?",
  "Which state is famous for its pink city, Jaipur?",
  "Which state is famous for Rann of Kutch?",
  "Which state is famous for Litti Chokha?",
  "Which state is famous for the Jagannath Temple in Puri?",
  "Which is the largest state in India by area?",
  "Which state is home to Gir National Park, famous for Asiatic lions?",
  "Which state is known for the Sun Temple at Konark?",
  "Which state is famous for the Valley of Flowers National Park?",
  "Which state has the famous Sanchi Stupa?",
  "Which state celebrates the famous Pongal festival?",
  "Bengaluru, also known as silicon valley of India, is in which state?",
  "Which state is famous for the Gateway of India?"
};

int correctAnswers[] = {6,7,8,10,3,7,8,3,4,9,5,11,2};  

// =================== SETUP =================== //
void setup() {
  Serial.begin(115200);

  Wire.begin(SDA_PIN, SCL_PIN, 400000);  // 400kHz I2C frequency

  Serial.println("India Map Ready!");

  pinMode(exploreLedPin, OUTPUT);
  pinMode(testLedPin, OUTPUT);

  if (!cap1.begin(0x5A)) Serial.println("MPR121 #1 not found");

  delay(1000);

  displayMode();
}

// =================== LOOP =================== //
void loop() {
  testModeFunction(); // Always stay in Test Mode
  delay(50);
}

// =================== DISPLAY MODE =================== //
void displayMode() {
  digitalWrite(testLedPin, HIGH);
  digitalWrite(exploreLedPin, LOW);

  Serial.println();
  Serial.println("Mode: TEST");
  Serial.print("Q: ");
  Serial.println(currentQuestion + 1);
  Serial.println(quizQuestions[currentQuestion]);
}

// =================== TEST MODE FUNCTION =================== //
void testModeFunction() {
  int touchedIndex = getTouchedElectrode();

  if (touchedIndex != -1) {
    Serial.println();

    if (touchedIndex == correctAnswers[currentQuestion]) {
      Serial.println("Correct!");
      score++;
    } else {
      Serial.println("Wrong!");
    }

    Serial.print("Score: ");
    Serial.println(score);

    delay(1500);

    currentQuestion++;
    if (currentQuestion >= (sizeof(quizQuestions) / sizeof(quizQuestions[0]))) {
      Serial.println();
      Serial.println("Quiz Over!");
      Serial.print("Final Score: ");
      Serial.println(score);

      delay(3000);
      currentQuestion = 0;
      score = 0;
    }

    displayMode();
  }
}

// =================== TOUCH DETECTION =================== //
int getTouchedElectrode() {
  uint16_t touched = cap1.touched();

  for (uint8_t i = 0; i < 12; i++) {
    if (touched & (1 << i)) return i;
  }

  return -1;
}
