# Projeto IoT para Captura de Dados de Fórmula E

## Descrição do Projeto

Este projeto tem como objetivo desenvolver um sistema IoT para captar e monitorar informações sobre a Fórmula E, permitindo que pessoas de todas as idades e níveis de conhecimento sobre automobilismo se envolvam e se emocionem com a competição. O projeto inclui sensores de temperatura e distância, além de um potenciômetro para simular o consumo de bateria.

## Componentes Utilizados

- Arduino Uno R3
- Sensor de Temperatura DHT22
- Sensor de Distância Ultrassônico HC-SR04
- Potenciômetro (simulando consumo de bateria)
- LED
- Resistor de 220Ω

## Conexões

### Sensor de Temperatura DHT22

- **VCC:** Conectado ao 5V do Arduino
- **GND:** Conectado ao GND do Arduino
- **Data:** Conectado ao pino digital 2 do Arduino

### Sensor de Distância Ultrassônico HC-SR04

- **VCC:** Conectado ao 5V do Arduino
- **GND:** Conectado ao GND do Arduino
- **Trig:** Conectado ao pino digital 9 do Arduino
- **Echo:** Conectado ao pino digital 10 do Arduino

### Potenciômetro

- **Pino do Meio:** Conectado ao pino analógico A1 do Arduino
- **Outros Pinos:** Conectados ao 5V e GND do Arduino

### LED

- **Ânodo (perna longa):** Conectado ao pino digital 13 do Arduino através de um resistor de 220Ω
- **Cátodo:** Conectado ao GND do Arduino

## Código

```cpp
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT22
#define LEDPIN 13
#define POTPIN A1
#define TRIGPIN 9
#define ECHOPIN 10

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  pinMode(LEDPIN, OUTPUT);
  pinMode(TRIGPIN, OUTPUT);
  pinMode(ECHOPIN, INPUT);
  dht.begin();
}

void loop() {
  float t = dht.readTemperature();

  int batteryValue = analogRead(POTPIN);
  float batteryPercentage = (batteryValue / 1023.0) * 100;

  long duration, distance;
  digitalWrite(TRIGPIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIGPIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIGPIN, LOW);
  duration = pulseIn(ECHOPIN, HIGH);
  distance = (duration / 2) / 29.1;

  if (t > 25) {
    digitalWrite(LEDPIN, HIGH);
  } else {
    digitalWrite(LEDPIN, LOW);
  }

  Serial.print("Temperatura: ");
  Serial.print(t);
  Serial.print(" *C\t");
  Serial.print("Distância: ");
  Serial.print(distance);
  Serial.print(" cm\tBateria: ");
  Serial.print(batteryPercentage);
  Serial.println(" %");

  delay(2000);
}
