# Serial Peripheral Interface

Das SPI ist Interface für synchrone und serielle Kommunikation. 

### Funktionsweise

Es werden alle Teilnehmer an folgenden drei Leitungen angschloseen:

- SCLK (auch SCK genannt) wird vom Host zur Synchronisation ausgegeben
- MOSI ist eine Datenleitung vom Host zum Client
- MISO ist eine Datenleitung vom Client zum Host



Es gibt eine weitere Leitung vom Host zu jedem Client mit dem Namen SS (oft auch CS oder STE genannt). Diese wählt einen Client aus, in dem sie dessen SS Leitung auf null setzt. In dem Folgendem Bild sehen Sie ein möglichen Aufbau. (Ich habe die Begriffe "Master" und "Slave" gegen "Host" und "Client" ausgetauscht. )

[![SPI Aufbau](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/SPI_three_slaves.svg/330px-SPI_three_slaves.svg.png)](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/SPI_three_slaves.svg/1920px-SPI_three_slaves.svg.png)

Für die Übertragung haben sich folgende vier Modi durchgesetzt:

| Mode | CPOL | CPHA |
| ---- | ---- | ---- |
| 0    | 0    | 0    |
| 1    | 0    | 1    |
| 2    | 1    | 0    |
| 3    | 1    | 1    |

Die Modi werden durch die Parameter "Clock Polarity" (CPOL) und "Clock Phase" (CPHA) festgelegt.  Bei CPOL=0 ist der Clock Idle Low, bei CPOL=1 ist der Clock Idle High.  Das beutet, dass bei CPOL = 0 ein bit versendet wird, wenn die Clock auf 1 ist.

CPHA gibt an, bei der wievielten Flanke die Daten übernommen werden  sollen. Bei CPHA=0 werden sie bei der ersten Flanke übernommen und bei CPHA=1 bei der zweiten.  Dies wurde bei dem folgendem Bild visualisiert.

[![Quelle: Wikipedia](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/SPI_timing_diagram2.svg/330px-SPI_timing_diagram2.svg.png)](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/SPI_timing_diagram2.svg/1920px-SPI_timing_diagram2.svg.png)

 Der Host legt seine Daten immer kurz nach der fallenden Flanke von SCK an.