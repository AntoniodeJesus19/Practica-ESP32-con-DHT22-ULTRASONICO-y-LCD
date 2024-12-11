# Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD
Utilizaremos la ESP32 en un entorno de adquisición de datos, le conectaremos un sensor DHT22, un Ultrasónico y una pantalla LCD; todo esto se hará en el simulador [WOKWI](https://wokwi.com/).



## Material Necesario
- [WOKWI](https://wokwi.com/)
- Trajeta ESP32
- Sensor DTH32
- Sensor Ultrasónico
- pantalla LCD I2C


## Instrucciones

### Requisitos Previos

Necesitas abrir la la pagina [WOKWI](https://wokwi.com/).


### Preparación del entorno

1. Ya en la plataforma debes seleccionar la tarjeta que usaremos en este caso seria la  ```ESP32```.
![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22/blob/main/Captura%20de%20pantalla%202024-12-09%20223637.png?raw=true)

2. Luego bajamos un poco el cursor y en ```Starter Templates``` seleccionamos ESP32.
![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22/blob/main/Captura%20de%20pantalla%202024-12-09%20224130.png?raw=true)

3. En la terminal de programacion borramos todo y colocamos la siguiente programación:
```
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 16;   //Pin digital 3 para el Echo del sensor
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int DHT_PIN = 15;

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  Serial.begin(9600);//iniciailzamos la comunicación
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  lcd.init();
  lcd.init();
  lcd.backlight();

}

void loop()
{

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
 lcd.clear();
 lcd.setCursor(2,0);
 lcd.print("Diplomado V");
 lcd.setCursor(2,1);
 lcd.print("Mecatronica");
 delay(2000);

 lcd.clear();
 lcd.setCursor(0,0);
 lcd.print("Antonio de Jesus");
 lcd.setCursor(2,1);
 lcd.print("Ing. Mecanico");
 delay(2000);


  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms

  lcd.clear();
 lcd.setCursor(0,0);
 lcd.print("Distancia " + String(d) + " cm");
 delay(2000);

 lcd.clear();
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  lcd.setCursor(2, 0);
  lcd.print("Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");

  delay(2000);
}



```

4. Instalar las librerias **DHT sensor library for ESPx** y **LiquidCrystal I2C**.

![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD/blob/main/Captura%20de%20pantalla%202024-12-10%20002136.png?raw=true)

6. Seleccionamos nuestro sensor en la parte de ```Simulacion``` en el boton ```+``` y buscamos **DH22** y **Ultrasonico**, luego buscamos **LCDI2C** y los conectamos de la siguiente manera.
![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD/blob/main/Captura%20de%20pantalla%202024-12-10%20192631.png?raw=true)


### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los textos y los datos en la pantalla LCD.


## Resultados

Cuando haya funcionado, verás los valores y los textos en la pantalla LCD como se muestran en las siguentes imagenes cada 2s.
![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD/blob/main/Captura%20de%20pantalla%202024-12-10%20192714.png?raw=true)

![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD/blob/main/Captura%20de%20pantalla%202024-12-10%20192745.png?raw=true)

![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD/blob/main/Captura%20de%20pantalla%202024-12-10%20192835.png?raw=true)

![](https://github.com/AntoniodeJesus19/Practica-ESP32-con-DHT22-ULTRASONICO-y-LCD/blob/main/Captura%20de%20pantalla%202024-12-10%20192856.png?raw=true)

# Créditos

Desarrollado por Antonio de Jesús Mentado Huerta

- [GitHub](https://github.com/AntoniodeJesus19)
