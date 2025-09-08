
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
// Importa a biblioteca SoftwareSerial para comunicação serial em pinos alternativos
#include <SoftwareSerial.h>

// Define os pinos de comunicação com o módulo Bluetooth (RX, TX)
SoftwareSerial bluetooth(10, 11); // RX ← Bluetooth TX | TX → Bluetooth RX

// Define os pinos conectados aos LEDs
const int led1 = 2;
const int led2 = 3;

void setup() {
  // Configura os pinos dos LEDs como saída
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);

  // Inicia a comunicação serial com o módulo Bluetooth a 9600 baud
  bluetooth.begin(9600); 
}

void loop() {
  // Verifica se há dados recebidos via Bluetooth
  if (bluetooth.available()) {
    // Lê o caractere enviado pelo aplicativo Bluetooth
    char comando = bluetooth.read();   

    // Compara o comando recebido e executa a ação correspondente
    if (comando == '1') {
      digitalWrite(led1, HIGH); // Liga LED 1 (pino 2)
    } else if (comando == '2') {
      digitalWrite(led1, LOW);  // Desliga LED 1 (pino 2)
    } else if (comando == '3') {
      digitalWrite(led2, HIGH); // Liga LED 2 (pino 3)
    } else if (comando == '4') {
      digitalWrite(led2, LOW);  // Desliga LED 2 (pino 3)
    }
  }
}
📱 Comandos Bluetooth
Use um aplicativo de terminal Bluetooth no seu celular e envie os seguintes comandos:

Comando	Ação
1	Liga LED 1
2	Desliga LED 1
3	Liga LED 2
4	Desliga LED 2

🛠️ Considerações
Use um divisor de tensão no pino RX do módulo Bluetooth para evitar sobrecarga (o Arduino usa 5V, mas o Bluetooth trabalha com 3.3V).

Certifique-se de que o módulo Bluetooth está pareado com o celular.

A taxa de transmissão padrão do HC-05 é 9600 baud.

O uso de SoftwareSerial evita conflito com a porta serial padrão (pinos 0 e 1), permitindo comunicação via USB ao mesmo tempo.
