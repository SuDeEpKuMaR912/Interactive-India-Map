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
  "Which state is famous for Dal Lake?",
  "Amber Fort, a major tourist attraction, is in which state?",
  "Statue of Unity, the worlds tallest statue, is located in which state?",
  "Ajanta and Ellora caves are located in which state?",
  "Which state is famous for the Jagannath Temple in Puri?",
  "In which state is the Pangong Lake located?",
  "In which state is the dance form Bharatanatyam most popular?",
  "Which is the largest state in India by area?",
  "Which state's capital is Dehradun?",
  "Which state has the famous Mughal Gardens?",
  "Hampi, a UNESCO World Heritage Site, is located in which state?",
  "Which state is home to Gir National Park, famous for Asiatic lions?",
  "Kedarnath Temple is located in which Indian state?",
  "Lingaraj Temple, a masterpiece of Kalinga architecture, is located in which state?",
  "Marina Beach, one of the longest beaches, is located in which state?",
  "Mahabalipuram temples are located in which state?",
  "Which state is known for the Sun Temple at Konark?",
  "Which state is famous for the Valley of Flowers National Park?",
  "Nalanda University, one of the oldest universities, is in which state?",
  "Which state has the famous Sanchi Stupa?",
  "Which state celebrates the famous Pongal festival?",
  "Kashi Vishwanath Temple is located in which state?",
  "Bengaluru, also known as silicon valley of India, is in which state?",
  "Which state is famous for the Gateway of India?",
  "Sabarmati Ashram, associated with Mahatma Gandhi, is located in which state?",
  "Which state's traditional dress includes a pheran?"
};

int correctAnswers[] = {6,7,8,10,0,7,8,2,3,1,5,7,4,0,11,8,4,3,5,5,3,4,10,9,5,6,11,2,8,0};  

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
  handleSerialInput(); // Check if RESET command is received
  testModeFunction();  // Run the game
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

// =================== HANDLE SERIAL INPUT =================== //
void handleSerialInput() {
  if (Serial.available()) {
    String incoming = Serial.readStringUntil('\n');
    incoming.trim(); // Remove any \r \n whitespace

    if (incoming == "RESET") {
      resetQuiz();
    }
  }
}

// =================== RESET QUIZ FUNCTION =================== //
void resetQuiz() {
  Serial.println("\n🔄 Quiz Reset Received!");
  currentQuestion = 0;
  score = 0;
  delay(500); // Small pause for clarity
  displayMode();
}
