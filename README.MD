# PRACTICA 1  :  BLINK
Alumnes: **Bernat Rubiol i Àfrica Abad**

## Objectiu
El objeticvo de la practica es producir el parpadeo periodico de un led. 
Se utilizara la salida serie  para depurar  el programa 

El microcontrolador que utilizaremos es el ya comentado en la introducción  **ESP32**

## TASQUES I PREGUNTES 

1. Generar el programa  y subir el codigo  al github de cada uno
```cpp
   #include <Arduino.h>
   int led = 13;

   // the setup routine runs once when you press reset:
   void setup() {                
   // initialize the digital pin as an output.
   pinMode(led, OUTPUT);     
   }

   // the loop routine runs over and over again forever:
   void loop() {
   digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
   delay(1000);               // wait for a second
   digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
   delay(2000);               // wait for a second
   }   
```

2. Modificar el programa para que incluya el envio de datos (ON y OFF) al puerto serie.
    Añadir la iunicialización del puerto serie y el envio cada vez que cambia el estado del led
   - Iniciar pin de led como salida 
   - Iniciar el terminal serie                      
   - bucle infinito  
       * encender led  
       * sacar por puerto serie mensaje *ON*          
       * espera de 1000 milisegundos  
       * apagar led  
       * sacar por puesto serie mensaje *OFF*        
       * espera de 1000 milisegundos  
   ```cpp
   #include <Arduino.h>

   int led = 13; 

   void setup() {                
      pinMode(led, OUTPUT);   
      Serial.begin(115200);
   }

   void loop() {
      Serial.println("ON");
      digitalWrite(led, HIGH);
      delay(1000);
      Serial.println("OFF");      
      digitalWrite(led, LOW);
      delay(1000);
   }
   ```

3. Modificar el programa para que actue directamente sobre los registros de los puertos de entrada y salida
   ```cpp
   #include <Arduino.h>

   int led = 13;

   void setup() {                
      pinMode(led, OUTPUT);   
      Serial.begin(115200);
   }

   void loop() {
      Serial.println("ON");
      digitalWrite(led, HIGH);
      Serial.println("OFF");      
      digitalWrite(led, LOW);
   }
   ```
4. Eliminar los delay modificar el pin de salida a uno cualquiera de los que estan disponibles i medir con el osciloscopio cual es la màxima frecuencia de apagado encendido que permite el microcontrolador. Medir la frecuencia en estos cuatro casos: 
   - 4.1 Con el envio por el puerto série del mensaje i utilizando las funciones de Arduino
   ```cpp
   #include <Arduino.h>

   int led = 13; 

   void setup() {                
      pinMode(led, OUTPUT);   
      Serial.begin(115200);
   }

   void loop() {
      Serial.println("ON");
      digitalWrite(led, HIGH);
      Serial.println("OFF");      
      digitalWrite(led, LOW);
   }
   ```
En aquest cas es de **34.2us o 29,24KHZ**
   - 4.2 Con el envio por el puerto série y accedirendo directamente a los registros:
   ```cpp
   #include <Arduino.h>

   int led = 13;
   uint32_t *gpio_out = (uint32_t *)GPIO_OUT_REG;

   void setup() {                
      pinMode(led, OUTPUT);   
      Serial.begin(115200);
   }

   void loop() {
      Serial.println("ON");
      *gpio_out |= (1 << led);
      Serial.println("OFF");      
      *gpio_out ^= (1 << led);
   }
   ```
En Aquest cas es de **34.2us o 29.24KHZ**
   - 4.3 Sin el envio por el puerto série del mensaje i utilizando las funciones de Arduino
```cpp
#include <Arduino.h>
int led = 13; 

void setup() {                
   pinMode(led, OUTPUT);   
}

void loop() {
   digitalWrite(led, HIGH);
   digitalWrite(led, LOW);
}
   ```
En Aquest cas es de **568ns o 1.76MHZ**
   - 4.4 Sin el envio por el puerto série y accedirendo directamente a los registros
```cpp
#include <Arduino.h>

int led = 13; 
uint32_t *gpio_out = (uint32_t *)GPIO_OUT_REG;

void setup() {                
   pinMode(led, OUTPUT);   
}

void loop() {
   *gpio_out |= (1 << led);
   *gpio_out ^= (1 << led);
}
```
En aquest cas es de **212ns o 4.717MHZ**

5. Generar un informe fichero  informe.MD ( markdown ) donde se muestre el codigo, un diagrama de flujo y un diagrama de tiempos 

### Diagrama de flux

```mermaid 
   graph TD
   A[Inici] --> B[Inicialitzar Pin de Sortida]
   B --> C[Encendre LED]
   C --> D[Esperar 1 segon]
   D --> E[Apagar LED]
   E --> F[Esperar 1 segon]
   F --> C
```

### Diagrama de temps

```mermaid
   sequenceDiagram
   participant A as Programa
   A->>A: Inicio
   A->>A: Inicializar Pin de Salida
   loop Bucle Principal
      A->>A: Encender LED
      A->>A: Esperar 1 segundo
      A->>A: Apagar LED
      A->>A: Esperar 1 segon
      end
```


6. Responder a la siguiente pregunta en el programa que se ha realizado cual es el tiempo libre que tiene el procesador ?

# Coclusions

En aquesta primera pràctica hem vist com 

# EJERCICIOS voluntarios  DE MEJORA DE NOTA

Elejir entre cualquiera de los siguentes:

* leer el valor de un convertidor  A/D  de entrada ; sacarlo por el puerto serie  y sacar el mismo valor  por otro pin  D/A

  - https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/


  ```cpp
#include <Arduino.h>

int led = 13; 
uint32_t *gpio_out = (uint32_t *)GPIO_OUT_REG;

void setup() {                
   pinMode(led, OUTPUT);   
}

void loop() {
   *gpio_out |= (1 << led);
   *gpio_out ^= (1 << led);
}
```

* Leer el valor del sensor de temperatura interno y sacarlo por el puerto serie 

  - https://gist.github.com/xxlukas42/7e7e18604f61529b8398f7fcc5785251

El resultado se ha de subir al github de cada uno y realizar un informe .MD 