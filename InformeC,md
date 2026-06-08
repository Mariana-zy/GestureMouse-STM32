# GestureMouse STM32

## Informe final del proyecto

**GestureMouse STM32** es un sistema embebido portátil tipo guante/controlador gestual, diseñado para detectar movimientos de la mano y de un dedo mediante sensores inerciales IMU. El proyecto utiliza un microcontrolador **STM32F030C8T6** como unidad principal de control, encargado de leer las IMU por comunicación SPI, procesar la información de movimiento y servir como base para generar acciones tipo mouse.

El sistema está pensado para funcionar como un dispositivo portátil alimentado por batería LiPo, con entrada USB-C para carga, regulación a 3.3 V, comunicación con sensores de movimiento y posibilidad de integración con un **ESP32-C6** para comunicación inalámbrica mediante Bluetooth Low Energy.

---

## Documentación del proyecto

Toda la documentación del proyecto se encuentra organizada en la carpeta `docs`, separada por fases de desarrollo.

### Fase 1: Nota de aplicación

En esta fase se planteó la idea inicial del proyecto, se definió el contexto de uso y se analizaron referencias relacionadas con guantes gestuales, sensores de movimiento y comunicación inalámbrica.

- [Nota de aplicación](<docs/Fase1_NotaAplicación/NotaAplicación.pdf>)
- [Enlaces de referencia](<docs/Fase1_NotaAplicación/Enlaces.md>)

### Fase 2: Prueba de concepto

En esta fase se validó la idea del proyecto usando una plataforma de desarrollo existente. Se utilizó el kit **STEVAL-STLKT01V1 SensorTile** de STMicroelectronics para probar lectura de sensores y comunicación Bluetooth mediante una aplicación móvil.

- [Documento de prueba de concepto](<docs/Fase2_PruebaConcepto/PruebaConcepto.pdf>)
- [Video de la prueba de concepto](<docs/Fase2_PruebaConcepto/Video.mp4>)
- [Enlaces usados en la prueba de concepto](<docs/Fase2_PruebaConcepto/Enlaces.md>)

### Fase 3: Diagramas del sistema

Esta fase contiene los diagramas principales del proyecto. Se incluyen los diagramas de bloques de hardware y firmware, además del árbol de potencia usado para analizar el flujo de energía del sistema.

- [Diagramas de bloques de hardware y firmware](<docs/Fase3_Diagramas/DiagramasBloques.pdf>)
- [Árbol de potencia](<docs/Fase3_Diagramas/ÁrbolPotencia.pdf>)

### Fase 4: Diseño electrónico y PCB

En esta fase se documenta el diseño electrónico del sistema. Incluye los criterios de diseño, los esquemáticos y las vistas de PCB. Esta es la etapa central del desarrollo de hardware del proyecto.

- [Criterios de diseño electrónico](<docs/Fase4_PCB's/CriteriosDiseño.pdf>)
- [Esquemáticos del proyecto](<docs/Fase4_PCB's/Esquemáticos.pdf>)
- [Diseño de PCB](<docs/Fase4_PCB's/PCB's.pdf>)

### Fase 5: Presentación del proyecto

Esta fase contiene las diapositivas utilizadas para presentar el proyecto, incluyendo contexto, objetivos, prueba de concepto, arquitectura, diseño electrónico, PCB y conclusiones.

- [Diapositivas de presentación](<docs/Fase5_Presentación/Diapositivas.pdf>)

---

## Contextualización

Las interfaces tradicionales como el mouse y el teclado requieren contacto físico directo. Esto puede limitar algunas formas de interacción, especialmente en aplicaciones de accesibilidad, control remoto, presentaciones, realidad virtual o sistemas robóticos.

El proyecto propone un guante controlador gestual capaz de interpretar movimientos naturales de la mano y convertirlos en acciones tipo mouse. La solución busca ser portátil, alimentada por batería y con posibilidad de comunicación inalámbrica.

---

## Objetivo general

Diseñar un sistema embebido portátil capaz de detectar movimientos de mano y dedo mediante sensores IMU, con el fin de servir como base para un controlador gestual tipo mouse.

---

## Objetivos específicos

- Diseñar una PCB principal para el sistema GestureMouse STM32.
- Implementar entrada de alimentación mediante USB-C.
- Integrar carga de batería LiPo.
- Regular el sistema a 3.3 V.
- Programar el STM32 mediante SWD.
- Leer sensores IMU mediante comunicación SPI.
- Separar físicamente la IMU del dedo en una PCB secundaria.
- Dejar comunicación UART disponible para integración con ESP32-C6.
- Documentar el sistema mediante diagramas, esquemáticos, PCB e informe técnico.

---

## Evolución del proyecto

El proyecto evolucionó desde una idea inicial basada en un guante gestual inalámbrico hasta el diseño de una PCB personalizada.

Inicialmente se consideró usar un **ESP32-C6** como microcontrolador principal debido a que integra Bluetooth Low Energy. Sin embargo, durante el desarrollo se decidió usar el **STM32F030C8T6** como controlador principal, ya que permite organizar mejor la lectura de sensores, la programación por SWD, el manejo de periféricos y la validación de la electrónica base.

El ESP32-C6 se mantiene dentro de la arquitectura del proyecto, pero como módulo dedicado a la comunicación Bluetooth. De esta forma, el STM32 se encarga de leer y procesar los sensores, mientras que el ESP32-C6 puede recibir comandos por UART y transmitirlos al computador como eventos tipo mouse.

---

## Prueba de concepto

Antes del diseño de la PCB personalizada se realizó una prueba de concepto usando el kit **STEVAL-STLKT01V1 SensorTile**. Esta etapa permitió validar que era posible leer sensores de movimiento y transmitir datos mediante Bluetooth Low Energy.

Durante la prueba se utilizó firmware de referencia basado en **FP-SNS-ALLMEMS1**, junto con la aplicación móvil **ST BLE Sensor**. Con esto se logró visualizar información real de sensores como acelerómetro, giroscopio y temperatura.

La prueba de concepto fue importante porque permitió entender el funcionamiento general de una plataforma con IMU, microcontrolador y comunicación inalámbrica. También permitió identificar la necesidad de diseñar una alimentación propia y estable para una versión personalizada del sistema.

Documento y video de esta etapa:

- [Prueba de concepto](<docs/Fase2_PruebaConcepto/PruebaConcepto.pdf>)
- [Video de prueba de concepto](<docs/Fase2_PruebaConcepto/Video.mp4>)

---

## Arquitectura general del sistema

La arquitectura general del sistema se basa en la lectura de sensores IMU mediante SPI, procesamiento local en el STM32 y comunicación posterior hacia un módulo inalámbrico.

```text
Movimiento de mano y dedo
        ↓
IMU LSM6DS3TR-C
        ↓
Comunicación SPI
        ↓
STM32F030C8T6
        ↓
Procesamiento de movimiento
        ↓
UART hacia ESP32-C6
        ↓
Bluetooth Low Energy
        ↓
Computador
```

---

## Principio de funcionamiento de la IMU

Una IMU, o unidad de medición inercial, integra sensores MEMS capaces de medir movimiento en varios ejes. En este proyecto se utiliza la **LSM6DS3TR-C**, que combina un acelerómetro de 3 ejes y un giroscopio de 3 ejes.

El acelerómetro mide aceleración lineal en los ejes X, Y y Z. Cuando el sensor está quieto, también permite estimar inclinación respecto a la gravedad. El giroscopio mide velocidad angular, lo que permite detectar rotaciones y movimientos rápidos.

Al combinar ambas mediciones, el sistema puede identificar movimientos de la mano y del dedo, que posteriormente pueden interpretarse como gestos asociados a acciones tipo mouse.

---

## Diseño por bloques

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

---

## Alimentación del sistema

El sistema recibe alimentación desde un puerto USB-C, usado únicamente para energía y carga de batería. No se utilizan las líneas de datos USB.

La batería LiPo se carga mediante un circuito integrado de carga, y posteriormente el voltaje de la batería se regula a 3.3 V para alimentar el STM32, las IMU y los demás circuitos digitales.

El árbol de potencia resume esta distribución:

- [Árbol de potencia](<docs/Fase3_Diagramas/ÁrbolPotencia.pdf>)

Flujo general de energía:

```text
USB-C 5 V
   ↓
Cargador LiPo
   ↓
Batería LiPo 1S
   ↓
Regulador 3.3 V
   ↓
STM32 + IMU + ESP32-C6
```

---

## Selección del regulador

Inicialmente se consideró el uso del regulador **AMS1117-3.3**, pero se descartó porque su caída de tensión es alta para un sistema alimentado con batería LiPo.

Una batería LiPo de una celda tiene aproximadamente:

```text
4.2 V completamente cargada
3.7 V nominal
```

El AMS1117 puede necesitar alrededor de 4.4 V para garantizar una salida estable de 3.3 V, por lo que no aprovecha correctamente el rango útil de la batería.

Por esta razón se seleccionó el **CAT6219**, un LDO de bajo dropout. Este regulador permite mantener una salida cercana a 3.3 V durante una mayor parte de la descarga de la batería.

La justificación completa de esta decisión se encuentra en:

- [Criterios de diseño electrónico](<docs/Fase4_PCB's/CriteriosDiseño.pdf>)

---

## Diseño de PCB

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

Las vistas de PCB se encuentran en:

- [Diseño de PCB](<docs/Fase4_PCB's/PCB's.pdf>)

---

## Firmware propuesto

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
Lectura WHO_AM_I de la IMU
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

El diagrama de firmware se encuentra junto con el diagrama de hardware en:

- [Diagramas de bloques de hardware y firmware](<docs/Fase3_Diagramas/DiagramasBloques.pdf>)

---

## Validación esperada

La validación del proyecto debe realizarse de forma gradual:

1. Revisar continuidad y ausencia de cortos.
2. Verificar entrada USB-C de 5 V.
3. Comprobar carga de batería LiPo.
4. Medir salida regulada de 3.3 V.
5. Verificar alimentación del STM32.
6. Programar el STM32 por SWD.
7. Validar comunicación SPI.
8. Leer el registro WHO_AM_I de la IMU.
9. Probar lectura de acelerómetro y giroscopio.
10. Enviar datos por UART.
11. Integrar comunicación con ESP32-C6.
12. Implementar gestos básicos tipo mouse.

---

## Alcance actual

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

---

## Aplicaciones posibles

El sistema puede utilizarse como base para:

- Mouse gestual.
- Control de presentaciones.
- Interfaces de accesibilidad.
- Control remoto de dispositivos.
- Interfaces para realidad virtual o aumentada.
- Control de sistemas robóticos.
- Interacción humano-máquina sin contacto directo.

---

## Limitaciones actuales

- El sistema aún debe fabricarse y ensamblarse físicamente.
- La comunicación BLE HID debe integrarse con el ESP32-C6.
- El reconocimiento de gestos aún requiere desarrollo de firmware.
- La IMU en encapsulado LGA puede ser difícil de soldar manualmente.
- La autonomía real debe medirse experimentalmente.
- Se debe validar el comportamiento eléctrico con batería real.

---

## Trabajo futuro

Como continuación del proyecto se propone:

- Fabricar la PCB principal y la PCB secundaria.
- Ensamblar los componentes.
- Validar alimentación, carga y regulación.
- Programar el STM32F030.
- Leer correctamente las IMU mediante SPI.
- Implementar filtrado de señales.
- Desarrollar gestos básicos.
- Integrar el ESP32-C6 para BLE HID.
- Probar el sistema como mouse inalámbrico.
- Mejorar la ergonomía del montaje en el guante.

---

## Reflexión final

La mayor dificultad técnica fue integrar correctamente todos los bloques de hardware para que funcionaran como un sistema portátil. No era solo conectar el STM32 con las IMU, sino asegurar una alimentación estable, una carga de batería adecuada, una regulación correcta a 3.3 V y señales organizadas para programación, comunicación y pruebas.

La parte mejor resuelta fue la organización del sistema por bloques, especialmente la etapa de alimentación y regulación. Esto permitió justificar mejor cada componente y tomar decisiones más adecuadas para un dispositivo alimentado por batería.

El aprendizaje principal fue entender que diseñar una PCB no consiste únicamente en conectar componentes. También implica leer hojas de datos, calcular consumos, revisar encapsulados, pensar en fabricación, organizar el esquemático y planear cómo se va a validar físicamente el circuito.

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
