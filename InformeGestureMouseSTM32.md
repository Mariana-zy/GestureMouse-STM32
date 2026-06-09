# GestureMouse STM32

## Informe

**GestureMouse STM32** es un sistema embebido portátil tipo guante/controlador gestual, diseñado para detectar movimientos de la mano y de un dedo mediante sensores inerciales IMU. El proyecto utiliza un microcontrolador **STM32F030C8T6** como unidad principal de control, encargado de leer las IMU por comunicación SPI, procesar la información de movimiento y servir como base para generar acciones tipo mouse, como movimiento de cursor, clic, scroll o arrastre.

El sistema está pensado para funcionar como un dispositivo portátil alimentado por batería LiPo, con entrada USB-C para carga, regulación a 3.3 V, comunicación con sensores de movimiento e integración de un **ESP32-C6** para comunicación inalámbrica mediante Bluetooth Low Energy.

---

## Tabla de contenido

- [Contextualización](#contextualización)
- [Objetivos](#objetivos)
- [Evolución del proyecto](#evolución-del-proyecto)
- [Prueba de concepto](#prueba-de-concepto)
- [Arquitectura general](#arquitectura-general)
- [Diagramas del sistema](#diagramas-del-sistema)
- [Diseño electrónico](#diseño-electrónico)
- [Árbol de potencia](#árbol-de-potencia)
- [Diseño de PCB](#diseño-de-pcb)
- [Firmware propuesto](#firmware-propuesto)
- [Validación esperada](#validación-esperada)
- [Alcance y aplicaciones](#alcance-y-aplicaciones)
- [Limitaciones](#limitaciones)
- [Trabajo futuro](#trabajo-futuro)
- [Referencias](#referencias)

---

## Contextualización

Las interfaces tradicionales como el mouse y el teclado requieren contacto físico directo. Esto puede limitar algunas formas de interaccion, especialmente en aplicaciones de accesibilidad, control remoto, presentaciones, realidad virtual o sistemas roboticos.

El proyecto propone un guante controlador gestual que interpreta movimientos naturales de la mano y del dedo para convertirlos en acciones tipo mouse. La solucion busca ser portatil, alimentada por bateria y con comunicacion inalambrica BLE HID.

---

## Objetivos

### Objetivo general

Diseñar un sistema embebido portátil capaz de detectar movimientos de mano y dedo mediante sensores IMU, con el fin de servir como base para un controlador gestual tipo mouse.

### Objetivos de esta etapa

- Diseñar una PCB principal para el sistema GestureMouse STM32.
- Implementar entrada de alimentación por USB-C.
- Integrar carga de bateria LiPo.
- Regular el sistema a 3.3 V.
- Programar el STM32 mediante SWD.
- Leer sensores IMU mediante SPI.
- Separar físicamente la IMU del dedo en una PCB secundaria.
- Dejar comunicación UART disponible para integración con ESP32-C6.
- Documentar el sistema mediante diagramas, esquemáticos, PCB e informe técnico.

---

## Evolución del proyecto

El proyecto evolucionó desde una idea inicial basada en un guante gestual inalámbrico hasta el diseño de una PCB personalizada. Este se divide en cinco fases principales:

### Fase 1: Nota de aplicación

En esta fase se planteó la idea inicial del proyecto, se definió el contexto de uso y se analizaron referencias relacionadas con guantes gestuales, sensores de movimiento y comunicación inalámbrica.

- [Nota de aplicación](<docs/Fase1_NotaAplicación/NotaAplicación.pdf>)
- [Enlaces de referencia](<docs/Fase1_NotaAplicación/Enlaces.md>)

Inicialmente se consideró usar un **ESP32-C6** como microcontrolador principal debido a que integra Bluetooth Low Energy. Sin embargo, durante el desarrollo se decidió usar el **STM32F030C8T6** como controlador principal, ya que permite organizar mejor la lectura de sensores, la programación por SWD, el manejo de periféricos y la validación de la electrónica base.

El ESP32-C6 se mantiene dentro de la arquitectura del proyecto, pero como módulo dedicado a la comunicación Bluetooth. De esta forma, el STM32 se encarga de leer y procesar los sensores, mientras que el ESP32-C6 puede recibir comandos por UART y transmitirlos al computador como eventos tipo mouse.

### Principio de funcionamiento de la IMU

En este punto es importante aclarar que es una IMU o unidad de medición inercial, esta integra sensores MEMS (Sistemas MicroElectroMecánicos) capaces de medir movimiento en varios ejes. Inlcuye tanto acelerómetro como giroscopio. 

El acelerómetro mide aceleración lineal en los ejes X, Y y Z. Cuando el sensor está quieto, también permite estimar inclinación respecto a la gravedad. El giroscopio mide velocidad angular, lo que permite detectar rotaciones y movimientos rápidos.

Al combinar ambas mediciones, el sistema puede identificar movimientos de la mano y del dedo, que posteriormente pueden interpretarse como gestos asociados a acciones tipo mouse.

---

### Fase 2: Prueba de concepto

En esta fase se validó la idea del proyecto usando una plataforma de desarrollo existente. Se utilizó el kit **STEVAL-STLKT01V1 SensorTile** de STMicroelectronics para probar lectura de sensores y comunicación Bluetooth mediante una aplicación móvil.

Durante la prueba se utilizó firmware de referencia basado en **FP-SNS-ALLMEMS1**, junto con la aplicación móvil **ST BLE Sensor**. Con esto se logró visualizar información real de sensores como acelerómetro, giroscopio y temperatura.

La prueba de concepto fue importante porque permitió entender el funcionamiento general de una plataforma con IMU, microcontrolador y comunicación inalámbrica. También permitió identificar la necesidad de diseñar una alimentación propia y estable para una versión personalizada del sistema.

- [Documento de prueba de concepto](<docs/Fase2_PruebaConcepto/PruebaConcepto.pdf>)
- [Video de la prueba de concepto](<docs/Fase2_PruebaConcepto/Video.mp4>)
- [Enlaces usados en la prueba de concepto](<docs/Fase2_PruebaConcepto/Enlaces.md>)

### Hardware usado

- SensorTile STEVAL-STLKT01V1.
- Cradle o base del SensorTile.
- STM32 Nucleo-L476RG usada como programador ST-LINK externo.
- Cable SWD.
- Cable USB.
- Aplicacion movil ST BLE Sensor.

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

- Programacion del SensorTile por SWD.
- Compilacion y carga del firmware ALLMEMS1.
- Visualizacion de datos reales de acelerometro, giroscopio y temperatura.
- Transmision BLE hacia el celular.
- Confirmacion de que la lectura de sensores y comunicacion inalambrica eran viables para el proyecto.

### Limitacion detectada

Durante las pruebas se observó dependencia de la Nucleo o del cable SWD para alimentacion o referencia de voltaje. Esto permitio identificar la necesidad de una alimentacion independiente y estable en el diseno final.

---

### Fase 3: Diagramas del sistema

Esta fase contiene los diagramas principales del proyecto. Se incluyen los diagramas de bloques de hardware y firmware, además del árbol de potencia usado para analizar el flujo de energía del sistema.

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
UART debug / ESP32-C6
        |
        v
Computador
```

### Diagrama de bloques de hardware

El diagrama de bloques muestra el flujo de energia, sensado, procesamiento y comunicacion del sistema.

Flujo general del hardware:

```text
USB-C / Batería
       ↓
Regulación 3.3 V
       ↓
STM32F030
   ↙     ↓     ↘
IMU   UART   LEDs/Botones
       ↓
    ESP32-C6
       ↓
      BLE
       ↓
  Computador
```

### Diagrama de bloques de firmware 

El firmware inicial se plantea como una validación progresiva del hardware. La primera meta no es implementar directamente el mouse completo, sino comprobar que el sistema puede alimentar correctamente los bloques y leer las IMU mediante SPI.

Flujo general del firmware:

```text
Inicio
  ↓
Configuración de reloj
  ↓
Inicialización GPIO
  ↓
Inicialización SPI
  ↓
Inicialización UART
  ↓
Lectura función de la IMU
  ↓
Validación de comunicación SPI
  ↓
Lectura de acelerómetro y giroscopio
  ↓
Filtrado básico
  ↓
Interpretación de gestos
  ↓
Envío de comandos por UART
```

El diagrama detallado de firmware se encuentra junto con el diagrama detallado de hardware en:

- [Diagramas de bloques de hardware y firmware](<docs/Fase3_Diagramas/DiagramasBloques.pdf>)

### Arbol de potencia

El sistema recibe alimentación desde un puerto USB-C, usado únicamente para energía y carga de batería. No se utilizan las líneas de datos USB.

La batería LiPo se carga mediante un circuito integrado de carga, y posteriormente el voltaje de la batería se regula a 3.3 V para alimentar el STM32, las IMU y los demás circuitos digitales.

Flujo general del árbol de potencia:

```text
USB-C 5 V
   │
   ├── Cargador LiPo
   │       │
   │       ├── LEDs de carga
   │       │   ├── CHRG
   │       │   └── STDBY
   │       │
   │       └── Batería LiPo 3.7 V
   │
   └── Entrada de alimentación

Batería LiPo 3.7 V
   │
   └── Regulador 3.3 V
           │
           ├── STM32F030C8T6
           │       ├── LED de estado
           │       ├── LED de error
           │       ├── Botón de usuario
           │       └── Reset
           │
           ├── IMU mano
           │
           ├── IMU dedo
           │
           └── ESP32-C6
                   └── Bluetooth Low Energy
```

### Consumo estimado

```text
Sistema base       aprox. 50 mA
ESP32-C6 promedio  aprox. 80 mA
Total promedio     aprox. 130 mA

Sistema base       aprox. 50 mA
ESP32-C6 pico      aprox. 200 mA
Total pico         aprox. 250 mA
```

### Potencia total

Con ESP32-C6 promedio

```text
P = 3.3 V * 0.13 A
P = 0.429 W
```

Con ESP32-C6 en pico:

```text
P = 3.3 V * 0.25 A
P = 0.825 W
```

### Autonomia ideal

La autonomia ideal se calcula como:

```text
Autonomia = Capacidad bateria / Corriente total
```

Para una bateria de 1000 mAh:

```text
Con ESP32-C6 promedio:
Autonomia = 1000 mAh / 130 mA = 7.7 h

Con ESP32-C6 en escenario conservador:
Autonomia = 1000 mAh / 250 mA = 4 h
```

El árbol de potencia resume esta distribución:

- [Árbol de potencia](<docs/Fase3_Diagramas/ÁrbolPotencia.pdf>)

---

### Fase 4: Diseño electrónico y PCB

En esta fase se documenta el diseño electrónico del sistema. Incluye los criterios de diseño, los esquemáticos y las vistas de PCB. Esta es la etapa central del desarrollo de hardware del proyecto.

### Selección del regulador

Inicialmente se consideró el uso del regulador **AMS1117-3.3**, pero se descartó porque su caída de tensión es alta para un sistema alimentado con batería LiPo.

Una batería LiPo de una celda tiene aproximadamente:

```text
4.2 V completamente cargada
3.7 V nominal
```

El AMS1117 puede necesitar alrededor de 4.4 V para garantizar una salida estable de 3.3 V, por lo que no aprovecha correctamente el rango útil de la batería.

Por esta razón se seleccionó el **CAT6219**, un LDO de bajo dropout. Este regulador permite mantener una salida cercana a 3.3 V durante una mayor parte de la descarga de la batería.

### Calculo termico del nuevo LDO CAT6219

La potencia disipada por el LDO se calcula como:

```text
P_LDO = (VIN - VOUT) * ILOAD
```

Con bateria completamente cargada:

```text
VIN = 4.2 V
VOUT = 3.3 V
VIN - VOUT = 0.9 V
```

Para una corriente de 250 mA:

```text
P_LDO = 0.9 V * 0.25 A
P_LDO = 0.225 W
```

Temperatura de union estimada:

```text
TJ = TA + thetaJA * P
TA = 25 C
thetaJA = 235 C/W
TJ = 25 + 235 * 0.225
TJ = 77.9 C aprox.
```

Este valor esta por debajo del limite recomendado de 125 C. Sin embargo, si la corriente sube hacia 450 mA o 500 mA, el LDO puede acercarse a una temperatura no recomendable para operacion continua.

La justificación completa de esta y otras decisiones se encuentra en:

- [Criterios de diseño electrónico](<docs/Fase4_PCB's/CriteriosDiseño.pdf>)

### Esquemático por bloques

El esquemático se organizó por bloques para facilitar la revisión, depuración y fabricación del sistema.

Los bloques principales son:

- **Input / Power In:** entrada de alimentación por USB-C.
- **Battery Charger:** carga de batería LiPo.
- **Regulation 3.3 V:** generación de la alimentación principal del sistema.
- **MCU Power:** alimentación y desacople del STM32.
- **MCU STM32F030C8T6:** unidad principal de control.
- **Debug / Program:** programación por SWD y comunicación UART.
- **IMU Hand:** sensor IMU ubicado en la PCB principal.
- **External Finger IMU:** conexión hacia la PCB secundaria del dedo.

Los esquemáticos completos se encuentran en:

- [Esquemáticos del proyecto](<docs/Fase4_PCB's/Esquemáticos.pdf>)

### Diseño de PCB

El proyecto contempla una PCB principal y una PCB secundaria para la IMU del dedo.

### PCB principal

La PCB principal integra:

- Entrada USB-C.
- Cargador de batería LiPo.
- Regulador de 3.3 V.
- STM32F030C8T6.
- IMU de la mano.
- Headers de programación.
- Header UART.
- Header hacia la IMU externa del dedo.
- LEDs de estado.
- Botones de usuario y reset.

### PCB secundaria

La PCB secundaria contiene la IMU del dedo, sus capacitores de desacople y el conector hacia la PCB principal. Esta separación permite ubicar físicamente el sensor sobre el dedo, manteniendo el procesamiento en la PCB principal.

Las vistas de las PCB's se encuentran en:

- [Diseño de PCB](<docs/Fase4_PCB's/PCB's.pdf>)

---

### Fase 5: Presentación del proyecto

Esta fase contiene las diapositivas utilizadas para presentar el proyecto, incluyendo contexto, objetivos, prueba de concepto, arquitectura, diseño electrónico, PCB y conclusiones.

- [Diapositivas de presentación](<docs/Fase5_Presentación/Diapositivas.pdf>)

---

## Validacion esperada

La validacion inicial debe realizarse en el siguiente orden:

1. Verificar continuidad y ausencia de cortos.
2. Alimentar por USB-C y revisar `USB_5V`.
3. Verificar carga de bateria.
4. Medir salida de 3.3 V del regulador.
5. Verificar alimentacion del STM32.
6. Programar el STM32 por SWD.
7. Inicializar SPI.
8. Revisar datos de la IMU de mano.
9. Revisar datos de la IMU de dedo.
10. Enviar datos por UART.
11. Probar movimiento de sensores.
12. Implementar deteccion basica de gestos.

---

## Alcance y aplicaciones

### Alcance actual

Hasta esta etapa del proyecto se logró:

- Definir la arquitectura general del sistema.
- Realizar una prueba de concepto con SensorTile.
- Diseñar los diagramas de hardware y firmware.
- Elaborar el árbol de potencia.
- Diseñar el esquemático por bloques.
- Seleccionar y justificar los componentes principales.
- Diseñar la PCB principal.
- Diseñar la PCB secundaria para la IMU del dedo.
- Preparar la documentación técnica del proyecto.

### Aplicaciones posibles

El sistema puede utilizarse como base para:

- Mouse gestual.
- Control de presentaciones.
- Interfaces de accesibilidad.
- Control remoto de dispositivos.
- Interfaces para realidad virtual o aumentada.
- Control de sistemas robóticos.
- Interacción humano-máquina sin contacto directo.

### Limitaciones actuales

- El sistema aún debe fabricarse y ensamblarse físicamente.
- La comunicación BLE HID debe integrarse con el ESP32-C6.
- El reconocimiento de gestos aún requiere desarrollo de firmware.
- La IMU en encapsulado LGA puede ser difícil de soldar manualmente.
- La autonomía real de la batería debe medirse experimentalmente.
- Se debe validar el comportamiento eléctrico con batería real.

### Trabajo futuro

- Fabricar PCB principal y PCB de dedo.
- Ensamblar componentes.
- Validar alimentacion y consumo real.
- Programar firmware base en STM32.
- Leer IMU por SPI.
- Implementar filtros digitales.
- Implementar reconocimiento de gestos.
- Integrar ESP32-C6 para BLE HID.
- Lograr que el computador reconozca el sistema como mouse inalambrico.
- Mejorar ergonomia e integracion fisica en el guante.

---

## Reflexion final

La mayor dificultad tecnica fue integrar correctamente todos los bloques de hardware para que funcionaran como un sistema portatil. No era solo conectar el STM32 con las IMU, sino asegurar que la alimentacion desde bateria fuera estable, que el cargador LiPo estuviera bien conectado, que el regulador entregara 3.3 V adecuados y que las senales SPI, SWD y UART quedaran organizadas para facilitar programacion, validacion y futuras mejoras.

La parte mejor resuelta fue el bloque de alimentacion: entrada USB-C, carga LiPo y regulacion a 3.3 V. El cambio de AMS1117 a un LDO de bajo dropout permitio aprovechar mejor el rango de voltaje de la bateria.

El aprendizaje principal fue entender que disenar una PCB no es solo conectar componentes. Tambien requiere leer datasheets, calcular consumos, justificar decisiones, organizar el esquematico por bloques y pensar en fabricacion, montaje y pruebas.

---

## Resumen de archivos en `docs`

```text
docs/
├── Fase1_NotaAplicación/
│   ├── NotaAplicación.pdf
│   └── Enlaces.md
│
├── Fase2_PruebaConcepto/
│   ├── PruebaConcepto.pdf
│   ├── Video.mp4
│   └── Enlaces.md
│
├── Fase3_Diagramas/
│   ├── DiagramasBloques.pdf
│   └── ÁrbolPotencia.pdf
│
├── Fase4_PCB's/
│   ├── CriteriosDiseño.pdf
│   ├── Esquemáticos.pdf
│   └── PCB's.pdf
│
└── Fase5_Presentación/
    └── Diapositivas.pdf
```

---

## Referencias

- STMicroelectronics. (s. f.). *STEVAL-STLKT01V1: SensorTile connectable sensor node: Sensor and connectivity development kit*. STMicroelectronics. https://www.st.com/en/evaluation-tools/steval-stlkt01v1.html
- STMicroelectronics. (s. f.). *FP-SNS-ALLMEMS1: STM32Cube function pack for IoT node with Bluetooth Low Energy connectivity, digital microphone, environmental, and motion sensors*. STMicroelectronics. https://www.st.com/en/embedded-software/fp-sns-allmems1.html
- STMicroelectronics. (s. f.). *STBLESensor: BLE sensor application for Android and iOS*. STMicroelectronics. https://www.st.com/en/embedded-software/stblesensor.html
- STMicroelectronics. (s. f.). *STM32F030C8: Mainstream Arm Cortex-M0 MCU*. STMicroelectronics. https://www.st.com/en/microcontrollers-microprocessors/stm32f030c8.html
- STMicroelectronics. (s. f.). *LSM6DS3TR-C: iNEMO inertial module, 3D accelerometer and 3D gyroscope*. STMicroelectronics. https://www.st.com/en/mems-and-sensors/lsm6ds3tr-c.html
- onsemi. (s. f.). *CAT6219: 500 mA CMOS LDO regulator datasheet*. onsemi. https://www.onsemi.com/pdf/datasheet/cat6219-d.pdf
- NanJing Top Power ASIC Corp. (s. f.). *TP4056: 1A standalone linear Li-ion battery charger datasheet*. https://www.tp4056.com/datasheet/
- STMicroelectronics. (s. f.). *STM32CubeMX*. STMicroelectronics. https://www.st.com/en/development-tools/stm32cubemx.html
- STMicroelectronics. (s. f.). *STM32CubeIDE*. STMicroelectronics. https://www.st.com/en/development-tools/stm32cubeide.html
- STMicroelectronics. (s. f.). *STM32CubeProgrammer software description*. STMicroelectronics. https://www.st.com/resource/en/user_manual/dm00403500-stm32cubeprogrammer-software-description-stmicroelectronics.pdf
- Git. (s. f.). *Git documentation*. https://git-scm.com/docs/git.html
- Mariana-zy. (2026). *fp-sns-allmems1: Prototipo con SensorTile y firmware ALLMEMS1* [Repositorio de GitHub]. GitHub. https://github.com/Mariana-zy/fp-sns-allmems1

