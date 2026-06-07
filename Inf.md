# GestureMouse STM32

## Informe

GestureMouse STM32 es un sistema embebido portatil tipo guante/controlador gestual, disenado para detectar movimientos de la mano y de un dedo mediante sensores IMU. El sistema utiliza un microcontrolador STM32F030 para leer las IMU por SPI y procesar la informacion de movimiento como base para generar acciones tipo mouse, como movimiento de cursor, clic, scroll o arrastre.

La primera etapa del proyecto se enfoca en validar la base electronica: entrada de alimentacion por USB-C, carga de bateria LiPo, regulacion a 3.3 V, programacion por SWD y comunicacion con sensores IMU.

---

## Tabla de contenido

* [Contextualizacion](#contextualizacion)
* [Objetivos](#objetivos)
* [Evolucion del proyecto](#evolucion-del-proyecto)
* [Prueba de concepto](#prueba-de-concepto)
* [Arquitectura general](#arquitectura-general)
* [Diagramas del sistema](#diagramas-del-sistema)
* [Diseno electronico](#diseno-electronico)
* [Arbol de potencia](#arbol-de-potencia)
* [Diseno de PCB](#diseno-de-pcb)
* [Firmware propuesto](#firmware-propuesto)
* [Validacion esperada](#validacion-esperada)
* [Alcance y aplicaciones](#alcance-y-aplicaciones)
* [Limitaciones](#limitaciones)
* [Trabajo futuro](#trabajo-futuro)
* [Referencias](#referencias)

---

## Contextualizacion

Las interfaces tradicionales como el mouse y el teclado requieren contacto fisico directo. Esto puede limitar algunas formas de interaccion, especialmente en aplicaciones de accesibilidad, control remoto, presentaciones, realidad virtual o sistemas roboticos.

El proyecto propone un guante controlador gestual que interpreta movimientos naturales de la mano y del dedo para convertirlos en acciones tipo mouse. La solucion busca ser portatil, alimentada por bateria y con comunicacion inalambrica BLE HID.

---

## Objetivos

### Objetivo general

Disenar un sistema embebido portatil capaz de detectar movimientos de mano y dedo mediante IMU para controlar acciones tipo mouse.

### Objetivos de esta etapa

* Disenar una PCB principal para el sistema GestureMouse STM32.
* Implementar entrada de alimentacion por USB-C.
* Integrar carga de bateria LiPo.
* Regular el sistema a 3.3 V.
* Programar el STM32 mediante SWD.
* Leer sensores IMU mediante SPI.
* Separar fisicamente la IMU del dedo en una PCB secundaria.
* Implementar comunicacion UART y BLE con ESP32-C6.

---

## Evolucion del proyecto

El proyecto evoluciono en tres etapas principales:

1. **Nota de aplicacion inicial**

   * Se planteo un guante gestual inalambrico.
   * Se considero inicialmente usar ESP32-C6 como microcontrolador principal.
   * Se propuso comunicacion BLE HID.
   * Se penso en multiples IMU y comunicacion I2C.

2. **Prueba de concepto con SensorTile**

   * Se uso el kit STEVAL-STLKT01V1.
   * Se programo con una Nucleo-L476RG usada como ST-LINK externo.
   * Se cargo el firmware FP-SNS-ALLMEMS1.
   * Se visualizaron datos reales en la aplicacion ST BLE Sensor.

3. **Diseno de PCB personalizada**

   * Se selecciono STM32F030C8T6 como microcontrolador principal.
   * Se cambio a comunicacion SPI para las IMU.
   * Se simplifico el sistema a una IMU de mano y una IMU externa de dedo.
   * Se dejo el ESP32-C6 como modulo para BLE HID.

---

## Prueba de concepto

Antes del diseno de la PCB personalizada se desarrollo una prueba de concepto usando el kit **STEVAL-STLKT01V1 SensorTile** de STMicroelectronics.

### Hardware usado

* SensorTile STEVAL-STLKT01V1.
* Cradle o base del SensorTile.
* STM32 Nucleo-L476RG usada como programador ST-LINK externo.
* Cable SWD.
* Cable USB.
* Aplicacion movil ST BLE Sensor.

### Firmware usado

Se utilizo el paquete oficial:

```text
FP-SNS-ALLMEMS1
Projects
STM32L476JG-SensorTile
Applications
ALLMEMS1
STM32CubeIDE
```

Este firmware permite leer sensores del SensorTile y transmitir informacion por Bluetooth Low Energy hacia la aplicacion movil ST BLE Sensor.

### Validaciones logradas

* Programacion del SensorTile por SWD.
* Compilacion y carga del firmware ALLMEMS1.
* Visualizacion de datos reales de acelerometro, giroscopio y temperatura.
* Transmision BLE hacia el celular.
* Confirmacion de que la lectura de sensores y la comunicacion inalambrica eran viables para el proyecto.

### Limitacion detectada

Durante las pruebas se observo dependencia de la Nucleo o del cable SWD para alimentacion o referencia de voltaje. Esto permitio identificar la necesidad de una alimentacion independiente y estable en el diseno final.

---

## Arquitectura general

La arquitectura actual del sistema se resume asi:

```text
Movimiento de mano/dedo
        |
        v
IMU LSM6DS3TR-C
        |
        v
SPI
        |
        v
STM32F030C8T6
        |
        v
Procesamiento local de gestos
        |
        v
UART debug / ESP32-C6 futuro
        |
        v
Computador
```

El objetivo inicial no es implementar completamente el mouse inalambrico, sino validar el hardware necesario para leer las IMU correctamente y procesar datos de movimiento.
