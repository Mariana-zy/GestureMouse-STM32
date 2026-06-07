# GestureMouse STM32

## Informe

GestureMouse STM32 es un sistema embebido portátil tipo guante/controlador gestual, diseñado para detectar movimientos de la mano y de un dedo mediante sensores IMU. El sistema utiliza un microcontrolador STM32F030 para leer las IMU por SPI y procesar la información de movimiento como base para generar acciones tipo mouse, como movimiento de cursor, clic, scroll o arrastre.

La primera etapa del proyecto se enfoca en validar la base electrónica: entrada de alimentación por USB-C, carga de batería LiPo, regulación a 3.3 V, programación por SWD y comunicación con sensores IMU.

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

Diseñar un sistema embebido portátil capaz de detectar movimientos de mano y dedo mediante IMU para controlar acciones tipo mouse.

### Objetivos de esta etapa

- Diseñar una PCB principal para el sistema GestureMouse STM32.
- Implementar entrada de alimentación por USB-C.
- Integrar carga de bateria LiPo.
- Regular el sistema a 3.3 V.
- Programar el STM32 mediante SWD.
- Leer sensores IMU mediante SPI.
- Separar físicamente la IMU del dedo en una PCB secundaria.
- Implementar comunicación UART y BLE con ESP32-C6.

---

## Evolución del proyecto

El proyecto evolucionó en tres etapas principales:

1. **Nota de aplicación inicial**
   - Se planteó un guante gestual inalámbrico.
   - Se consideró inicialmente usar ESP32-C6 como microcontrolador principal.
   - Se propuso comunicación BLE HID.
   - Se pensó en múltiples IMU y comunicacion I2C.

2. **Prueba de concepto con SensorTile**
   - Se usó el kit STEVAL-STLKT01V1.
   - Se programó con una Núcleo-L476RG usada como ST-LINK externo.
   - Se cargó el firmware FP-SNS-ALLMEMS1.
   - Se visualizaron datos reales en la aplicación ST BLE Sensor.

3. **Diseno de PCB personalizada**
   - Se seleccionó STM32F030C8T6 como microcontrolador principal.
   - Se cambió a comunicación SPI para las IMU.
   - Se simplificó el sistema a una IMU de mano y una IMU externa de dedo.
   - Se dejó el ESP32-C6 como módulo para BLE HID.

---

## Prueba de concepto

Antes del diseno de la PCB personalizada se desarrollo una prueba de concepto usando el kit **STEVAL-STLKT01V1 SensorTile** de STMicroelectronics.

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

---

## Diagramas del sistema

### Diagrama de bloques de hardware

El diagrama de bloques muestra el flujo de energia, sensado, procesamiento y comunicacion del sistema.

Archivo editable:

- [Diagrama de bloques en draw.io](docs/diagrams/Diagramas_Bloques_GestureMouse_STM32_final.drawio)

PDF exportado:

- [Diagrama de bloques en PDF](docs/diagrams/Diagrama_Bloques_GestureMouse_STM32.pdf)

> Nota: para visualizarlo directamente dentro de GitHub, exportar el PDF desde draw.io como PNG y guardarlo con el nombre `docs/images/diagrama_bloques_hardware.png`.

```markdown
![Diagrama de bloques de hardware](docs/images/diagrama_bloques_hardware.png)
```

### Arbol de potencia

El arbol de potencia muestra la distribucion de energia desde USB-C y bateria LiPo hacia el regulador, microcontrolador, IMU y cargas futuras.

Archivo editable:

- [Arbol de potencia en draw.io](docs/diagrams/Arbol_Potencia_GestureMouse_STM32_final.drawio)

PDF exportado:

- [Arbol de potencia en PDF](docs/diagrams/Arbol_Potencia_GestureMouse_STM32.pdf)

> Nota: para visualizarlo directamente dentro de GitHub, exportar el PDF desde draw.io como PNG y guardarlo con el nombre `docs/images/arbol_potencia.png`.

```markdown
![Arbol de potencia](docs/images/arbol_potencia.png)
```

---

## Diseno electronico

El esquematico fue organizado por bloques para facilitar revision, depuracion y paso a PCB.

### Bloques del esquematico

1. Input / Power In.
2. Battery Charger.
3. Regulation 3.3 V.
4. MCU Power.
5. MCU STM32F030C8T6.
6. Test Points.
7. Debug / Program.
8. IMU Hand.
9. External Finger IMU.

Esquematico completo:

- [Esquematico principal en PDF](docs/schematics/Schematic_main.pdf)
- [Revision del esquematico en PDF](docs/schematics/Sheet1_review.pdf)

### Bloque 1: Input / Power In

La entrada USB-C se usa para alimentacion y carga de bateria. No se usan las lineas diferenciales de datos USB.

Conexiones principales:

```text
VBUS -> USB_5V
GND -> GND
SHIELD -> GND
CC1 -> 5.1 kOhm -> GND
CC2 -> 5.1 kOhm -> GND
DP/DN/SBU -> NC
```

![Bloque de entrada USB-C](docs/images/schematic_input_power.svg)

### Bloque 2: Battery Charger

El cargador LiPo se basa en un TP4056 o equivalente. La corriente de carga se define mediante la resistencia `RPROG`.

Calculo:

```text
I_CHG = 1200 / RPROG
RPROG = 3.3 kOhm
I_CHG = 1200 / 3.3
I_CHG = 364 mA aprox.
```

Este valor es adecuado para una bateria LiPo de 1000 mAh, ya que corresponde aproximadamente a `0.36 C`.

### Bloque 3: Regulation 3.3 V

Inicialmente se considero usar AMS1117-3.3, pero se descarto por su alto dropout. Con una bateria LiPo, el voltaje maximo es aproximadamente 4.2 V y el voltaje nominal 3.7 V. Un AMS1117 necesita alrededor de 4.4 V para garantizar 3.3 V de salida, por lo que no es adecuado para aprovechar correctamente la bateria.

Se selecciono el regulador **CAT6219 / C255621**, un LDO de bajo dropout.

![Bloque de regulacion con CAT6219](docs/images/schematic_regulation_cat6219.svg)

Conexion recomendada:

```text
VIN -> BAT+
GND -> GND
EN  -> BAT+
BYP -> NC o capacitor opcional de 10 nF
VOUT -> 3.3 V
```

![Conexion del regulador CAT6219](docs/images/regulator_cat6219_connection.svg)

### Bloque 4: MCU Power

El STM32F030 se alimenta desde la linea `3.3V_MCU`. Se usan capacitores de desacople cercanos a los pines de alimentacion.

Recomendacion de ubicacion:

```text
0.1 uF cerca de cada pin VDD/VDDA
10 uF como capacitor de reserva
Plano de GND continuo
```

### Bloque 5: MCU STM32F030C8T6

Funciones principales del STM32:

- Lectura de IMU por SPI.
- Control de chip-select independientes.
- Gestion de LEDs y botones.
- Programacion por SWD.
- Comunicacion UART para debug o ESP32-C6 futuro.

Pines principales:

```text
PA5  -> SCK
PA6  -> MISO
PA7  -> MOSI
PA9  -> TX
PA10 -> RX
PA13 -> SWDIO
PA14 -> SWCLK
PB0/PB1 -> CS de IMU
PC13/PC14 -> LEDs de estado
NRST -> Reset
BOOT0 -> Pull-down
```

### Bloques 8 y 9: IMU mano e IMU externa del dedo

La IMU de mano queda en la PCB principal. La IMU del dedo se separa en una PCB externa para mejorar la ubicacion fisica sobre el guante.

Conexiones SPI de la IMU:

```text
SCK  -> SCL
MOSI -> SDA
MISO -> SDO
CS   -> CS
3.3V -> VDD y VDDIO
GND  -> GND
INT1/INT2 -> Test points
```

![Arquitectura IMU externa](docs/images/external_imu_architecture.svg)

![Conectores para IMU externa](docs/images/external_imu_connectors.svg)

---

## Arbol de potencia

El sistema se alimenta desde una bateria LiPo 1S, cargada mediante USB-C y regulada a 3.3 V para alimentar los componentes digitales.

### Flujo de energia

```text
USB-C 5 V
   |
   v
Cargador LiPo
   |
   v
Bateria LiPo 1S
   |
   v
Regulador 3.3 V
   |
   v
STM32 + IMU + headers + ESP32-C6 futuro
```

### Consumo estimado

Escenario actual sin ESP32-C6:

```text
STM32F030      aprox. 20-30 mA
IMU mano       aprox. 1-2 mA
IMU dedo       aprox. 1-2 mA
LEDs MCU       aprox. 1-2 mA
Margen/debug   aprox. 10 mA
Total          aprox. 50 mA
```

Escenario futuro con ESP32-C6:

```text
Sistema base       aprox. 50 mA
ESP32-C6 promedio  aprox. 80 mA
Total promedio     aprox. 130 mA

Sistema base       aprox. 50 mA
ESP32-C6 pico      aprox. 200 mA
Total pico         aprox. 250 mA
```

### Potencia total

Sin ESP32-C6:

```text
P = V * I
P = 3.3 V * 0.05 A
P = 0.165 W
```

Con ESP32-C6 en escenario promedio:

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
Sin ESP32-C6:
Autonomia = 1000 mAh / 50 mA = 20 h

Con ESP32-C6 promedio:
Autonomia = 1000 mAh / 130 mA = 7.7 h

Con ESP32-C6 en escenario conservador:
Autonomia = 1000 mAh / 250 mA = 4 h
```

### Calculo termico del LDO

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

---

## Diseno de PCB

### PCB principal

La PCB principal integra:

- Entrada USB-C.
- Cargador de bateria LiPo.
- Regulador 3.3 V.
- STM32F030C8T6.
- IMU de mano.
- Headers de programacion y comunicacion.
- Conector hacia la PCB del dedo.
- LEDs y botones.

Archivos:

- [PCB principal en PDF](docs/pcb/PCB_main.pdf)
- [PCB principal exportada desde Altium](docs/pcb/PCB_main_altium_export.pdf)

> Nota: para mostrar la PCB directamente en GitHub, exportar desde Altium una captura 2D y una vista 3D con estos nombres:

```text
docs/images/pcb_main_2d.png
docs/images/pcb_main_3d.png
```

```markdown
![PCB principal 2D](docs/images/pcb_main_2d.png)
![PCB principal 3D](docs/images/pcb_main_3d.png)
```

### PCB de IMU del dedo

La PCB externa contiene:

- IMU LSM6DS3TR-C.
- Conector de 6 pines.
- Capacitores de desacople.
- Test points INT1 e INT2.

Conector recomendado:

```text
1 - 3.3V
2 - GND
3 - SCK
4 - MOSI
5 - MISO
6 - CS / IMU_F
```

Archivo:

- [PCB de IMU del dedo en PDF](docs/pcb/PCB_finger_imu.pdf)

Imagen de referencia de ruteo:

![Ruteo PCB IMU dedo](docs/images/finger_imu_pcb_routing.svg)

> Nota: para mostrar la PCB externa directamente en GitHub, exportar desde Altium capturas con estos nombres:

```text
docs/images/pcb_finger_2d.png
docs/images/pcb_finger_3d.png
```

---

## Firmware propuesto

El firmware inicial debe validar la comunicacion con las IMU antes de implementar gestos completos.

Flujo general:

```text
Inicio
  |
  v
Configuracion de reloj a 48 MHz
  |
  v
Inicializacion de GPIO, SPI y UART
  |
  v
Inicializacion de IMU
  |
  v
Lectura del registro WHO_AM_I
  |
  v
Validacion de comunicacion SPI
  |
  v
Lectura de acelerometro y giroscopio
  |
  v
Filtrado de senales
  |
  v
Deteccion de gestos
  |
  v
Envio de datos por UART / eventos futuros de mouse
```

Archivo editable del diagrama:

- [Diagrama de firmware en draw.io](docs/diagrams/Diagrama_Firmware_GestureMouse_STM32.drawio)

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
8. Leer `WHO_AM_I` de la IMU de mano.
9. Leer `WHO_AM_I` de la IMU de dedo.
10. Enviar datos por UART.
11. Probar movimiento de sensores.
12. Implementar deteccion basica de gestos.

---

## Alcance y aplicaciones

### Alcance actual

- Arquitectura del sistema definida.
- Esquematico organizado por bloques.
- PCB principal disenada.
- PCB externa para IMU del dedo.
- Arbol de potencia calculado.
- Firmware base propuesto.
- BLE HID previsto como mejora futura.

### Aplicaciones posibles

- Mouse gestual.
- Control de presentaciones.
- Accesibilidad.
- Control robotico.
- Interfaces de realidad virtual.
- Interaccion humano-maquina sin contacto directo.

---

## Limitaciones

- La comunicacion BLE HID aun no esta implementada en la PCB principal.
- El TP4056 no incluye power-path completo.
- Si la bateria no tiene proteccion, se recomienda usar una bateria protegida o agregar un circuito PCM/BMS 1S.
- La IMU LSM6DS3TR-C en encapsulado LGA puede ser dificil de soldar manualmente.
- Aun falta fabricar y validar fisicamente la PCB.

---

## Trabajo futuro

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

## Estructura recomendada del repositorio

```text
GestureMouse-STM32/
├── README.md
├── INFORME_COMPLETO_GestureMouse_STM32.md
├── docs/
│   ├── images/
│   │   ├── schematic_input_power.svg
│   │   ├── schematic_regulation_cat6219.svg
│   │   ├── regulator_cat6219_connection.svg
│   │   ├── external_imu_architecture.svg
│   │   ├── external_imu_connectors.svg
│   │   ├── finger_imu_pcb_routing.svg
│   │   ├── diagrama_bloques_hardware.png
│   │   ├── arbol_potencia.png
│   │   ├── pcb_main_2d.png
│   │   ├── pcb_main_3d.png
│   │   ├── pcb_finger_2d.png
│   │   └── pcb_finger_3d.png
│   ├── diagrams/
│   ├── schematics/
│   └── pcb/
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
- Mariana-zy. (2026). *GestureMouse-STM32: Diseno de PCB y firmware base para guante controlador gestual* [Repositorio de GitHub]. GitHub. https://github.com/Mariana-zy/GestureMouse-STM32
