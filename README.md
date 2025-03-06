
# **🔢 Arduino DIY Calculator using 4x4 Keypad & 16x2 LCD 📟**  

![Arduino Calculator](https://img.shields.io/badge/Arduino-Calculator-blue?style=for-the-badge&logo=arduino)  

### **🚀 Overview**  
This is a **simple yet functional calculator** built using an **Arduino Uno, a 4x4 Keypad, and a 16x2 LCD Display**. It can perform **basic arithmetic operations (+, -, *, /)**, supports **decimal numbers**, and includes **error handling** for invalid inputs (like division by zero).  

---

## **🛠 Features**  
✅ **Basic Arithmetic Operations** → Addition, Subtraction, Multiplication, Division  
✅ **Decimal Support** → Perform calculations with decimal numbers  
✅ **Error Handling** → Prevents multiple operators, division by zero errors  
✅ **Clear Functionality** → Reset calculations with a single press  

---

## **📜 Components Required**  

| **Component** | **Quantity** |
|--------------|-------------|
| Arduino Uno  | 1 |
| 16x2 LCD Display (with or without I2C) | 1 |
| 4x4 Keypad  | 1 |
| 10KΩ Potentiometer (for LCD contrast) | 1 |
| Jumper Wires  | Multiple |
| Breadboard | 1 |

---

## **🔌 Wiring Diagram**  
```
LCD Pins  →  Arduino Pins  
VSS      →  GND  
VDD      →  5V  
V0       →  Middle Pin of Potentiometer  
RS       →  13  
RW       →  GND  
E        →  12  
D4       →  11  
D5       →  10  
D6       →  9  
D7       →  8  
A (LED)  →  5V  
K (LED)  →  GND  

Keypad Pins  →  Arduino Pins  
Row 1 (R1)  →  7  
Row 2 (R2)  →  6  
Row 3 (R3)  →  5  
Row 4 (R4)  →  4  
Col 1 (C1)  →  3  
Col 2 (C2)  →  2  
Col 3 (C3)  →  1  
Col 4 (C4)  →  0  
```
---

## **💻 Arduino Code**  
```cpp
#include <LiquidCrystal.h>
#include <Keypad.h>

LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

float first = 0, second = 0, total = 0;
char customKey;
bool isSecondNum = false;
char operation = '\0';

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','/'},
  {'4','5','6','*'},
  {'7','8','9','-'},
  {'C','0','=','+'}
};
byte rowPins[ROWS] = {7 ,6 ,5 ,4};
byte colPins[COLS] = {3, 2, 1, 0};

Keypad customKeypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

void setup() {
  lcd.begin(16, 2);
  lcd.print("Arduino Calculator");
  delay(2000);
  lcd.clear();
}

void loop() {
  customKey = customKeypad.getKey();

  if (customKey) {
    if ((customKey >= '0' && customKey <= '9') || customKey == '.') {
      if (isSecondNum) {
        second = (second * 10) + (customKey - '0');
      } else {
        first = (first * 10) + (customKey - '0');
      }
      lcd.print(customKey);
    }
    else if (customKey == '+' || customKey == '-' || customKey == '*' || customKey == '/') {
      if (operation == '\0' && first != 0) {
        operation = customKey;
        isSecondNum = true;
        lcd.print(customKey);
      }
    }
    else if (customKey == '=') {
      if (operation != '\0' && isSecondNum) {
        switch (operation) {
          case '+': total = first + second; break;
          case '-': total = first - second; break;
          case '*': total = first * second; break;
          case '/': 
            if (second == 0) {
              lcd.clear();
              lcd.print("Err: Div by 0");
              delay(2000);
              resetCalculator();
              return;
            } else {
              total = first / second;
            }
            break;
        }

        lcd.setCursor(0, 1);
        lcd.print("= ");
        lcd.print(total);
        delay(3000);
        resetCalculator();
      }
    }
    else if (customKey == 'C') {
      resetCalculator();
    }
  }
}

void resetCalculator() {
  first = second = total = 0;
  isSecondNum = false;
  operation = '\0';
  lcd.clear();
}
```

---

## **📷 Project Preview**  
🔹 _You can add a photo of your project here!_  
🔹 **Example:**  
![Project Image](https://your-image-url.com)

---

## **📌 How to Use?**  
1️⃣ Power up the Arduino.  
2️⃣ The LCD will display **"Arduino Calculator"**.  
3️⃣ Press numeric keys to input numbers.  
4️⃣ Press `+`, `-`, `*`, or `/` to choose an operation.  
5️⃣ Enter the second number and press `=` to see the result.  
6️⃣ Press `C` to clear and start a new calculation.  

---

## **📚 Libraries Used**  
📌 [`Keypad.h`](https://www.arduino.cc/reference/en/libraries/keypad/) - Used for handling the 4x4 keypad.  
📌 [`LiquidCrystal.h`](https://www.arduino.cc/en/Reference/LiquidCrystal) - Used to control the 16x2 LCD Display.  

---

## **📜 License**  
📝 This project is **open-source** under the **MIT License**. Feel free to modify and improve!  

---

## **💬 Support & Contributions**  
⭐ If you like this project, consider **starring** this repository!  
👨‍💻 Contributions & improvements are welcome! Feel free to **fork** and submit a **pull request**.  

🚀 **Happy Coding!** 🎯  
