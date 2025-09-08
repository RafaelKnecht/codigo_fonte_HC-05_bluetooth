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

// Inclui a biblioteca SoftwareSerial para permitir comunicação serial em outros pinos
#include <SoftwareSerial.h>

// Define os pinos usados para comunicação com o módulo Bluetooth
// Pino 10 será o RX (recebe dados do Bluetooth)
// Pino 11 será o TX (envia dados para o Bluetooth)
SoftwareSerial bluetooth(10, 11); // RX, TX

// Define os pinos onde os LEDs estão conectados
const int led1 = 2;
const int led2 = 3;

void setup() {
  // Define os pinos dos LEDs como saída
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);

  // Inicia a comunicação serial com o módulo Bluetooth
  bluetooth.begin(9600); // A maioria dos módulos HC-05 usa 9600 bps
}

void loop() {
  // Verifica se há dados recebidos via Bluetooth
  if (bluetooth.available()) {
    // Lê o caractere recebido
    char comando = bluetooth.read();   

    // Verifica qual comando foi recebido e executa a ação correspondente
    if (comando == '1') {
      // Liga o LED 1 (pino 2)
      digitalWrite(led1, HIGH);
    } else if (comando == '2') {
      // Desliga o LED 1 (pino 2)
      digitalWrite(led1, LOW);
    } else if (comando == '3') {
      // Liga o LED 2 (pino 3)
      digitalWrite(led2, HIGH);
    } else if (comando == '4') {
      // Desliga o LED 2 (pino 3)
      digitalWrite(led2, LOW);
    }
  }
}
