# Arquitectura técnica de ISLA

Este documento describe la arquitectura conceptual de una implementación de ISLA. No pretende fijar una lista obligatoria de hardware o software: define capas, interfaces y criterios de diseño.

La experiencia práctica mostró además que una estación de audio no termina en el DAW. Drivers, clocks, controladores USB, hubs, alimentación, ruido analógico y compatibilidad electromagnética forman parte de la arquitectura real.

## 1. Capas

```text
┌──────────────────────────────────────────────────────┐
│                   PRODUCCIÓN MUSICAL                 │
│                       MusE                           │
├──────────────────────────────────────────────────────┤
│ MIDI / Audio / Automatización / Plugins / Routing   │
├──────────────────────────────────────────────────────┤
│ ALSA                  JACK / PipeWire-JACK*          │
├──────────────────────────────────────────────────────┤
│ Drivers libres / kernel GNU/Linux                   │
├──────────────────────────────────────────────────────┤
│ USB / controladores PCI / MIDI DIN / audio analógico│
├──────────────────────────────────────────────────────┤
│ Hardware vintage        Instrumentos virtuales       │
│ CZ-101 / SQ-1 / K1r     Surge XT / ISLA 800 / drums │
└──────────────────────────────────────────────────────┘
```

`*` La implementación concreta puede variar según el sistema. ISLA no requiere una única tecnología de servidor de audio mientras la pila siga siendo abierta y reproducible.

## 2. DAW

El centro actual de producción es **MusE**.

Se eligió por su combinación de:

- orientación MIDI;
- audio multipista;
- software libre;
- soporte natural de GNU/Linux;
- routing;
- automatización;
- plugins;
- integración de hardware externo.

El objetivo es que un sintetizador hardware pueda tratarse dentro del proyecto de forma conceptualmente similar a un instrumento virtual.

Ejemplo:

```text
Pista MIDI CZ-101
      │
      ▼
MIDI OUT / canal n
      │
      ▼
Casio CZ-101
      │ audio analógico
      ▼
AudioLink IN
      │
      ▼
Pista de audio CZ-101
```

MusE dejó de ser un candidato en evaluación y pasó a ser el centro operativo de la estación.

## 3. Audio principal

La interfaz principal utilizada durante el desarrollo es una **Midiplus AudioLink Plus II**.

El soporte GNU/Linux se consiguió extendiendo el driver libre Ozzy. Esto implicó incorporar soporte específico del dispositivo y corregir tanto el camino MIDI/SysEx como un caso posterior de MIDI IN después de resets USB por cambio de sample rate.

ISLA considera al driver parte de la estación. Si una pieza del stack no existe, puede formar parte del trabajo del proyecto implementarla en lugar de reemplazar hardware funcional.

En la implementación principal la AudioLink actúa como **reloj maestro de JACK**.

Configuración de trabajo actual:

```text
sample rate: 48 kHz
period:      512 frames
periods:     3
inputs:      4
outputs:     4
```

Estos valores describen la instalación actual, no un requisito universal de ISLA.

## 4. Entrada de audio secundaria

Una entrada USB genérica de micrófono se incorporó como canal mono adicional sin ocupar una de las cuatro entradas de la AudioLink.

La arquitectura es:

```text
AudioLink Plus II
      │
      │ JACK master
      ▼
     JACK
      ▲
      │ resampling / drift correction
   alsa_in
      ▲
      │
USB Audio Device
```

La invocación utilizada es:

```bash
alsa_in -j USB-IN -d hw:CARD=Device,DEV=0 -r 48000 -c 1
```

El dispositivo secundario tiene su propio clock. `alsa_in` permite hacerlo convivir con el master sin fingir que ambos osciladores son idénticos.

La entrada de micrófono del dongle entrega aproximadamente 2.4 V de bias para electret. Para una fuente externa se utiliza un capacitor electrolítico de 10 µF en serie, con el positivo hacia el dongle, bloqueando DC antes de la fuente conectada.

La caracterización reveló además un offset DC importante en el dominio digital. Una captura en silencio mostró:

```text
DC offset   0.015080
RMS lev dB    -36.43
```

Después de un high-pass de 30 Hz y descartando el transitorio inicial:

```text
DC offset  -0.000000
RMS lev dB    -83.87
```

Por eso el primer procesamiento en MusE es un high-pass muy bajo, alrededor de 16–20 Hz.

La documentación completa está en [`docs/usb-audio-input.md`](docs/usb-audio-input.md).

## 5. MIDI

MIDI cumple varias funciones:

- interpretación desde controladores;
- secuenciación desde MusE;
- selección de programas;
- edición remota;
- transferencia SysEx;
- sincronización de instrumentos históricos con software moderno.

La topología física puede incluir MIDI USB y MIDI DIN 5 pines.

Un principio importante es mantener identificable cada destino por puerto y canal, evitando que la configuración quede implícita.

### MIDI IN y resets de la AudioLink

El intento de utilizar el SQ-1 como controlador principal reveló una condición particular del hardware/driver.

Normalmente la AudioLink entrega MIDI IN en registros USB de cinco bytes: cuatro slots MIDI y un byte fijo `0xEB` de framing. Después de determinados resets por cambio de sample rate se observaron transfers cortas, a menudo de un byte raw.

El parser original descartaba implícitamente cualquier transferencia menor de cinco bytes. La corrección en Ozzy permite pasar esos bytes cortos directamente a ALSA, manteniendo el parser normal para el framing de cinco bytes.

La corrección quedó validada en cambios 44.1 kHz ↔ 96 kHz.

Commit de referencia en Ozzy:

```text
41afa0470becb9ec3703c351f73f10f29b72fb8d
Fix AudioLink MIDI input after sample-rate reset
```

Este caso refuerza un criterio del proyecto: cuando una ruta MIDI falla, el diagnóstico debe atravesar todas las capas antes de atribuir el problema al instrumento o al DAW.

## 6. Hardware externo

### Casio CZ-101

- fuente sonora hardware;
- control MIDI;
- Program Change;
- SysEx;
- librarian Web MIDI separado del DAW;
- recuperación física y documentación del instrumento;
- recuperación de patches históricos;
- envío a edit buffer y slots internos;
- control de afinación mediante el librarian.

### Ensoniq SQ-1

- fuente sonora hardware;
- teclado amplio utilizable como controlador MIDI;
- integración MIDI IN validada después de corregir el caso de reset de la AudioLink;
- preservación de patches y secuencias históricas;
- recuperación de soportes magnéticos todavía como línea de preservación.

### Kawai K1r

- módulo MIDI externo;
- edición remota desde GNU/Linux;
- mantenimiento físico del panel pendiente.

## 7. Instrumentos virtuales

ISLA intenta que los instrumentos sean independientes del DAW.

Ejemplos actuales:

- Surge XT;
- ISLA 800;
- Drumlabooh;
- kits Hydrogen;
- samples WAV;
- plugins LV2 y otros plugins libres compatibles.

Drumlabooh se utiliza también con salidas múltiples para separar piezas de batería y procesarlas mediante buses de MusE.

La regla práctica es favorecer formatos que puedan conservarse localmente y reinstalarse sin depender de servidores de activación.

## 8. Topología USB

La experiencia mostró que **USB debe documentarse como parte del routing de audio**.

Dos conectores físicamente cercanos pueden terminar en hubs o controladores PCI distintos. Y un hub agregado por comodidad mecánica puede cambiar el comportamiento temporal de una ruta de audio.

En la ThinkPad T520 usada como implementación principal, la distribución estable separa la AudioLink del controlador que contiene el controlador MIDI USB y la entrada auxiliar:

```text
EHCI interno
   └── AudioLink Plus II
       JACK master

ExpressCard xHCI
   ├── controlador MIDI USB
   └── entrada USB auxiliar
```

La topología se inspecciona con:

```bash
lsusb -t
```

Durante una prueba, introducir una cadena de hubs en la rama de la entrada auxiliar coincidió con una tormenta de errores de `alsa_in` y XRUNs. Al volver a los puertos directos de la ExpressCard, la prueba equivalente quedó en XRUN = 0.

No se concluye que todos los hubs sean inadecuados. Se concluye que **la topología debe verificarse en la máquina real**.

## 9. Orquestación de arranque

La estación utiliza un launcher para transformar una instalación general de GNU/Linux en “modo DAW” de manera reproducible.

El flujo actual incluye:

```text
verificar hardware
      ↓
verificar controladores USB críticos
      ↓
deshabilitar temporalmente cámara / Bluetooth
      ↓
verificar o iniciar JACK
      ↓
configurar entrada USB auxiliar si existe
      ↓
iniciar alsa_in
      ↓
validar puertos JACK
      ↓
iniciar MusE
      ↓
cleanup y restauración al salir
```

Esto evita depender de pasos manuales y reduce variaciones entre sesiones.

## 10. Jerarquía portable

Parte del entorno se organiza bajo:

```text
/opt/isla-audio/
├── ardour/          # legado/experimentos previos
├── muse/
├── plugins/
│   ├── lv2/
│   └── vst3/
├── instruments/
├── samples/
├── presets/
├── archive/
├── manifests/
└── tools/
```

El árbol exacto puede evolucionar. La intención es separar la estación musical de la distribución base y facilitar inventario, migración y reconstrucción.

## 11. Flujo de señal híbrido

```text
                    ┌─────────────┐
                    │   MusE      │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       CZ-101             SQ-1          ISLA 800
          │                │                │
          │ audio          │ audio          │ digital
          ▼                ▼                │
        AudioLink ◄────────┘                │
          │                                 │
          └──────────────► JACK ◄───────────┘
                           ▲
                           │
                      alsa_in / USB-IN
                           │
                           ▼
                         MusE
                           │
                      FX / mezcla
                           │
                           ▼
                         WAV
```

## 12. Ruido, DC e interferencia

ISLA intenta distinguir fenómenos que suelen mezclarse bajo la palabra “ruido”.

Un vúmetro alto puede ser DC y no hiss. Un zumbido de 50 Hz puede llegar por acoplamiento capacitivo a través del cuerpo humano. Una transmisión celular puede entrar como RF de modo común y demodularse accidentalmente en una etapa analógica no lineal.

Por eso el diagnóstico combina:

- escucha;
- medición directa con ALSA;
- estadísticas con SoX;
- análisis espectral;
- inspección de mixer;
- topología USB;
- pruebas físicas de distancia y cableado.

El DSP debe corregir lo que corresponde al dominio digital; no puede sustituir una mala topología USB ni impedir una demodulación RF que ya ocurrió antes del ADC.

## 13. Reproducibilidad

Una instalación de ISLA debería poder describirse mediante:

- versión del sistema operativo;
- kernel;
- drivers adicionales;
- versión de MusE;
- plugins e instrumentos;
- puertos MIDI;
- dispositivos ALSA;
- parámetros del servidor de audio;
- sample rate y buffers;
- topología USB y controladores PCI relevantes;
- clientes auxiliares como `alsa_in`;
- mixer ALSA de dispositivos no triviales;
- árbol `/opt/isla-audio`;
- repositorios y commits concretos cuando sea relevante.

## 14. Qué NO debe ser parte obligatoria de ISLA

ISLA no debería depender de:

- DRM;
- activaciones online permanentes;
- cuentas obligatorias para abrir proyectos locales;
- formatos que sólo pueda interpretar una aplicación propietaria sin especificación pública;
- drivers cerrados sin alternativa mantenible;
- servicios cloud necesarios para producir o recuperar una obra.

## 15. Principio arquitectónico

La arquitectura debe permitir reemplazar una capa sin destruir las demás.

Cambiar de DAW no debería destruir los patches.  
Cambiar de computadora no debería inutilizar el hardware.  
Cambiar de interfaz no debería convertir los proyectos en ilegibles.  
Que desaparezca un fabricante no debería borrar un instrumento.

Ese desacoplamiento es una de las formas concretas de soberanía tecnológica que ISLA intenta construir.
