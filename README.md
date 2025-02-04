#include <Arduino.h>

// --- Pin Configuration ---
const int flameSensorPins[] = {25, 26, 27, 14, 12}; // GPIO pins for flame sensors
const int numFlameSensors = sizeof(flameSensorPins) / sizeof(flameSensorPins[0]);
const int flameThreshold = 2000; // Threshold for detecting a flame (adjust based on your sensor)

// Variables to track time
unsigned long startTime;
const unsigned long duration = 60000; // 1 minute = 60000 milliseconds

// Function to read values from all sensors
void readFlameSensors(int values[]) {
    for (int i = 0; i < numFlameSensors; i++) {
        values[i] = analogRead(flameSensorPins[i]);
    }
}

// Function to check if any sensor detects a flame and print the sensor number(s)
bool detectFlame(int values[]) {
    bool flameDetected = false;

    Serial.print("Flame detected by sensor(s): ");
    for (int i = 0; i < numFlameSensors; i++) {
        if (values[i] > flameThreshold) {
            Serial.print(i + 1); // Sensor numbers are 1-based
            Serial.print(" ");
            flameDetected = true;
        }
    }

    if (flameDetected) {
        Serial.println(); // Move to the next line
    } else {
        Serial.println("None");
    }

    return flameDetected;
}

const int motorIA = 32; // Connect to IA on L9110
const int motorIB = 33; // Connect to IB on L9110

void setup() {
    Serial.begin(9600);
    pinMode(motorIA, OUTPUT);
    pinMode(motorIB, OUTPUT); 
    // Initialize flame sensor pins as inputs
    for (int i = 0; i < numFlameSensors; i++) {
        pinMode(flameSensorPins[i], INPUT);
    }

    Serial.println("5-channel flame sensor module initialized.");

    // Record the start time
    startTime = millis();
}

void stop_fan() {
    digitalWrite(motorIA, LOW);
    digitalWrite(motorIB, LOW);
}

void run_fan() {
    // Set motor to rotate clockwise
    analogWrite(motorIA, 225); // Full speed
    digitalWrite(motorIB, LOW);
    serial.printIn("fan start");
    delay(4000);

    // Stop the motor
    digitalWrite(motorIA, LOW);
    digitalWrite(motorIB, LOW);
}        

void loop() {
    int sensorValues[numFlameSensors];
    
    // Read sensor values
    readFlameSensors(sensorValues);
    
    // Check if flame is detected
    if (detectFlame(sensorValues)) {
        Serial.println("Flame detected!");
        run_fan();
        stop_fan();  // Stop the fan after flame is detected
        return;  // Exit the loop after detecting the flame
    } else {
        Serial.println("No flame detected.");
        stop_fan();  // Ensure fan is stopped when no flame is detected
    }

    delay(1000); // Small delay to avoid flooding the Serial Monitor
}
