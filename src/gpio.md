# General-purpose I/Os (GPIO)

Die Pins sind in Ports unterteilt. Dabei hat jeder Port 16 Pins. Die Ports sind nach den Buchstaben A, B, C, ... benannt. Die Pins werden von 0 bis 15 benannt. Jeder Pin kann als *Input* oder *Output* konfiguriert werden. Dabei kann man *pull-up* und *pull-down* Widerstände einstellen.  Man kann außerden die *Geschwindigkeit* ändern. Die Pins sind besonders schnell. Sie können alle 2 *clock cycles* [^1]  gesetzt werden.   

Jerder Port hat 10 32-bit Register. Davon sind 4 zur Konfiguration (GPIOx_MODER, GPIOx_OTYPER, GPIOx_OSPEEDR und GPIOx_PUPDR), zwei Datenregister (GPIOx_IDR und GPIOx_ODR), ein set / reset Register (GPIOx_BSRR), ein Register um die Pins zu Sperren (GPIOx_LCKR) und zwei weitere Register für alternative funktionen (GPIOx_AFRH and GPIOx_AFRL).

Die Register der Ports A bis F sind folgender Tabelle zu entnehmen.

| Port | Adressenbereich           | Größe |
| ---- | ------------------------- | ----- |
| A    | 0x4800_0000 - 0x4800_03FF | 1 K   |
| B    | 0x4800_0400 - 0x4800_07FF | 1 K   |
| C    | 0x4800_0800 - 0x4800_0BFF | 1 K   |
| D    | 0x4800_0C00 - 0x4800_0FFF | 1 K   |
| E    | 0x4800_1000 - 0x4800_13FF | 1 K   |
| F    | 0x4800_1400 - 0x4800_17FF | 1 K   |



### GPIO port mode register (GPIOx_MODER)

Dieser Register ist am Offset 0x00 zu finden. Der Rigister wird benutzt um den IO Modus festzulegen. Jeder Mode wird mit 2 Bits spezifiziert. So können mit den 32 Bit alle 16 Pins angesprochen werden. Die folgende Tabellen Spezifizieren die Konfigurationsmöglichkeiten.

| Bit  | 0-1  | 2-3  | 4-5  | 6-7  | 8-9  | 10-11 | 12-13 | 14-15 | 16-17 | 18-19 | 20-21 | 22-23 | 24-25 | 26-27 | 28-29 | 30-31 |
| ---- | ---- | ---- | ---- | ---- | ---- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Port | 0    | 1    | 2    | 3    | 4    | 5     | 6     | 7     | 8     | 9     | 10    | 11    | 12    | 13    | 14    | 15    |

| Wert | Mode                 |
| ---- | -------------------- |
| 0b00 | Eingabe              |
| 0b01 | Ausgabe              |
| 0b10 | Alternativer FN Mode |
| 0b11 | Analog               |

Anmerkung: Auf dem  STM32F303xB/xC und STM32F358x werden 0b10 und 0b11 nicht unterstützt.

### GPIO port output type register (GPIOx_OTYPER) 

Dieser Register ist am Offset 0x04 zu finden. Bit 16 bis 31 sind Reserviert und sollten nicht genutzt werden. In Bit 0 bis 15 kann man zwischen den Ausgabe Typen

- 0: Output push-pull (Standard)
- 1: Output open-drain[^2]

entscheiden.

### GPIO port output speed register (GPIOx_OSPEEDR)

Dieser Register ist am Offset 0x08 zu finden. Dieser Register wird benutzt um die Ausgabegeschwindigkeit festzulegen.  Jeder Mode wird mit 2 Bits spezifiziert. So können mit den 32 Bit alle 16 Pins angesprochen werden. Die folgende Tabellen Spezifizieren die Konfigurationsmöglichkeiten.

| Bit  | 0-1  | 2-3  | 4-5  | 6-7  | 8-9  | 10-11 | 12-13 | 14-15 | 16-17 | 18-19 | 20-21 | 22-23 | 24-25 | 26-27 | 28-29 | 30-31 |
| ---- | ---- | ---- | ---- | ---- | ---- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Port | 0    | 1    | 2    | 3    | 4    | 5     | 6     | 7     | 8     | 9     | 10    | 11    | 12    | 13    | 14    | 15    |

| Wert | Mode         |
| ---- | ------------ |
| 0b00 | Low speed    |
| 0b01 | Medium Speed |
| 0b10 | Low speed    |
| 0b11 | High speed   |

### GPIO port pull-up/pull-down register (GPIOx_PUPDR)

Dieser Register ist am Offset 0x0C zu finden. Dieser Register wird benutzt um die Vorwiderstände zu schalten.  Jeder Mode wird mit 2 Bits spezifiziert. So können mit den 32 Bit alle 16 Pins angesprochen werden. Die folgende Tabellen Spezifizieren die Konfigurationsmöglichkeiten.

| Bit  | 0-1  | 2-3  | 4-5  | 6-7  | 8-9  | 10-11 | 12-13 | 14-15 | 16-17 | 18-19 | 20-21 | 22-23 | 24-25 | 26-27 | 28-29 | 30-31 |
| ---- | ---- | ---- | ---- | ---- | ---- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Port | 0    | 1    | 2    | 3    | 4    | 5     | 6     | 7     | 8     | 9     | 10    | 11    | 12    | 13    | 14    | 15    |

| Wert | Mode               |
| ---- | ------------------ |
| 0b00 | kein Vorwiderstand |
| 0b01 | Pull-up            |
| 0b10 | Pull-Down          |
| 0b11 | Reserviert         |

### GPIO port input data register (GPIOx_IDR)

Dieser Register ist am Offset 0x10 zu finden. Bit 16 bis 31 sind Reserviert und sollten nicht genutzt werden. Aus Bit 0 bis 15 kann man die jeweiligen Eingabewerte des Ports lesen. Es besteht keine Schreibberechtigung.

### GPIO port output data register (GPIOx_ODR)

Dieser Register ist am Offset 0x14 zu finden. Bit 16 bis 31 sind Reserviert und sollten nicht genutzt werden. In Bit 0 bis 15 kann man die jeweiligen Ausgaben des Ports schreiben.

### GPIO port bit set/reset register (GPIOx_BSRR) 

Dieser Register ist am Offset 0x18 zu finden. Bit 0 bis 15 werden genutzt, um den entsprechenden Pin zu setzten. Bit 16 bis 31 werden genutzt, um um den entsprechenden Pin auf 0 zurück zu setzten. Es gibt keine Leseberechtigung in dem Register. Bei einem Leseversuch wird 0 zurück gegeben.

### GPIO port configuration lock register (GPIOx_LCKR)

Das Sperren von Registern unterstützte ich anfangs auch nicht.  Eine Beschreibung wird später hinzugefügt.

###  GPIOx_AFRL und GPIOx_AFRH

Diese Register unterstützte ich Erstmal nicht. Eine Beschreibung wird später hinzugefügt.

### GPIO port bit reset register (GPIOx_BRR)

Dieser Register ist am Offset 0x28 zu finden. Bit 16 bis 31 sind Reserviert und sollten nicht genutzt werden. Bit 0 bis 15 werden genutzt, um die Ausgabe auf 0 zusetzten. Dazu muss in dem Bit 1 geschrieben werden. Bei 0 wird keine Aktion ausgeführt. 



------



[^1]: Clock cycles dienen zur  Synchronisation der CPU. 
[^2]: ([Open-Collector-Ausgang](https://de.wikipedia.org/wiki/Open-Collector-Ausgang))

