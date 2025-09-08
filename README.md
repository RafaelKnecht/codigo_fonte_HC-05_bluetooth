# 🔌 Controle de LEDs via Bluetooth com Arduino

Este projeto demonstra como controlar dois LEDs remotamente usando um **Arduino UNO** e um módulo **Bluetooth HC-05 ou HC-06**, utilizando a biblioteca `SoftwareSerial` para comunicação serial em pinos alternativos.

---

## 📋 Descrição

Com este sistema, é possível ligar e desligar dois LEDs conectados ao Arduino através de comandos enviados por um smartphone com um aplicativo de terminal Bluetooth. A comunicação é feita via Bluetooth, utilizando comandos simples do tipo `1`, `2`, `3`, e `4`.

---

## 🧰 Materiais Utilizados

- 1x Arduino Uno  
- 1x Módulo Bluetooth HC-05 ou HC-06  
- 2x LEDs  
- 2x Resistores de 220Ω  
- Protoboard e jumpers  
- 1x Smartphone com app Bluetooth (ex: **Serial Bluetooth Terminal**)

---

## 🧠 Esquema de Conexão

### LEDs:
- LED 1 → Pino digital **2** (com resistor de 220Ω)
- LED 2 → Pino digital **3** (com resistor de 220Ω)

### Bluetooth HC-05/HC-06:
- **VCC** → 5V do Arduino  
- **GND** → GND do Arduino  
- **TXD** → Pino **10** do Arduino  
- **RXD** → Pino **11** do Arduino (⚠️ com divisor de tensão para evitar dano)

---

## 💻 Código Arduino

```cpp
#include <SoftwareSerial.h>

SoftwareSerial bluetooth(10, 11); // RX, TX
const int led1 = 2;
const int led2 = 3;

void setup() {
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  bluetooth.begin(9600); 
}

void loop() {
  if (bluetooth.available()) {
    char comando = bluetooth.read();   

    if (comando == '1') {
      digitalWrite(led1, HIGH); // Liga LED1
    } else if (comando == '2') {
      digitalWrite(led1, LOW);  // Desliga LED1
    } else if (comando == '3') {
      digitalWrite(led2, HIGH); // Liga LED2
    } else if (comando == '4') {
      digitalWrite(led2, LOW);  // Desliga LED2
    }
  }
}
