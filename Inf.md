
- [Evolucion del proyecto](#evolucion-del-proyecto)
- [Prueba de concepto](#prueba-de-concepto)
- [Arquitectura general](#arquitectura-general)
- [Principio de funcionamiento de la IMU](#principio-de-funcionamiento-de-la-imu)
- [Decision STM32 vs ESP32-C6](#decision-stm32-vs-esp32-c6)
- [Diagramas del sistema](#diagramas-del-sistema)
- [Diseno electronico](#diseno-electronico)
- [Arbol de potencia](#arbol-de-potencia)
- [Alcance y aplicaciones](#alcance-y-aplicaciones)
- [Limitaciones](#limitaciones)
- [Trabajo futuro](#trabajo-futuro)
- [Capturas sugeridas de datasheets](#capturas-sugeridas-de-datasheets)
- [Referencias](#referencias)

---
   - Se simplifico el sistema a una IMU de mano y una IMU externa de dedo.
   - Se dejo el ESP32-C6 como posible modulo futuro para BLE HID.

Esta evolucion fue importante porque el proyecto paso de una idea inicial centrada en comunicacion inalambrica a una etapa mas realista de validacion de hardware. En la version actual, el STM32 se encarga del control local y lectura de sensores, mientras que el ESP32-C6 queda reservado para una etapa posterior de comunicacion BLE HID.

---

## Prueba de concepto

---

## Principio de funcionamiento de la IMU

Una IMU, o unidad de medicion inercial, integra sensores MEMS capaces de medir movimiento en varios ejes. En este proyecto se usa la **LSM6DS3TR-C**, que combina:

- Acelerometro de 3 ejes.
- Giroscopio de 3 ejes.

El acelerometro mide aceleracion lineal sobre los ejes `X`, `Y` y `Z`. Cuando el dispositivo esta quieto, tambien permite observar la orientacion relativa respecto a la gravedad. Por eso puede usarse para detectar inclinacion de la mano o del dedo.

El giroscopio mide velocidad angular sobre los ejes `X`, `Y` y `Z`. Esto permite detectar rotaciones, cambios rapidos de orientacion y movimientos dinamicos. En un guante gestual, esta informacion es util para diferenciar movimientos suaves, giros, inclinaciones y gestos rapidos.

En el sistema GestureMouse STM32, la IMU entrega datos digitales al microcontrolador mediante SPI:

```text
Movimiento fisico
      |
      v
Acelerometro + giroscopio
      |
      v
Datos digitales por SPI
      |
      v
STM32F030
      |
      v
Filtrado e interpretacion de gestos
```

La IMU de la mano se usa para detectar movimiento global, inclinacion y orientacion principal. La IMU del dedo se usa para detectar acciones especificas asociadas a gestos, como clic, seleccion o confirmacion.

El firmware debe leer periodicamente los registros de aceleracion y giro, aplicar algun filtrado basico para reducir ruido y luego comparar los valores contra umbrales o estados para identificar gestos.

---

## Decision STM32 vs ESP32-C6

En la nota de aplicacion inicial se considero usar un ESP32-C6 como microcontrolador principal, principalmente porque integra Bluetooth Low Energy y podria funcionar directamente como dispositivo HID. Sin embargo, para la version actual se decidio usar un **STM32F030C8T6** como microcontrolador principal.

La decision se baso en el alcance real de esta etapa del proyecto. Antes de implementar un mouse BLE completo, era necesario validar la base electronica: alimentacion, regulacion, programacion, comunicacion SPI y lectura de sensores. Para ese objetivo, el STM32 es una opcion adecuada porque permite trabajar de forma ordenada con CubeMX, SWD, SPI, UART y una arquitectura de firmware embebido mas controlada.

Comparacion de enfoque:

```text
STM32F030:
- Control principal del hardware.
- Lectura de IMU por SPI.
- Programacion y depuracion por SWD.
- Bajo consumo frente a un modulo inalambrico activo.
- Adecuado para validar PCB y sensores.

ESP32-C6:
- Integra Bluetooth Low Energy.
- Util para BLE HID en una etapa futura.
- Mayor consumo, especialmente con radio activa.
- Mas conveniente como modulo de comunicacion que como centro del prototipo actual.
```

Por esta razon, la arquitectura final separa responsabilidades:

```text
IMU -> STM32F030 -> UART -> ESP32-C6 futuro -> BLE HID -> Computador
```

Esta separacion permite que la PCB actual se enfoque en la adquisicion y procesamiento local de movimiento, dejando la comunicacion Bluetooth como una mejora posterior sin comprometer la validacion inicial del hardware.

---

## Diagramas del sistema

### Diagrama de bloques de hardware

---
