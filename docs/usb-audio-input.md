# Entrada USB auxiliar y topología USB

Una de las extensiones más simples de ISLA terminó produciendo varias lecciones importantes sobre audio digital, USB, clocks, ruido y medición.

El objetivo era sumar una entrada mono adicional sin consumir una de las cuatro entradas fijas de la Midiplus AudioLink Plus II.

La solución probada utiliza un dongle USB de audio genérico con entrada de micrófono, incorporado a JACK mediante `alsa_in` mientras la AudioLink continúa siendo la interfaz principal y el reloj maestro.

## Arquitectura

```text
AudioLink Plus II
      │
      │  JACK master
      │  48 kHz
      ▼
     JACK
      ▲
      │
   alsa_in
      ▲
      │
USB Audio Device
entrada mic mono
```

La ventaja de `alsa_in` es precisamente que el segundo dispositivo no necesita compartir el mismo clock físico con la AudioLink. `alsa_in` adapta continuamente la diferencia entre ambos relojes.

La invocación utilizada es:

```bash
alsa_in \
  -j USB-IN \
  -d hw:CARD=Device,DEV=0 \
  -r 48000 \
  -c 1
```

Después de arrancar, JACK expone:

```text
USB-IN:capture_1
```

La opción `-c 1` es importante: el dispositivo de prueba ofrece una sola entrada y, sin especificarla, `alsa_in` puede intentar abrir dos canales.

## Mixer del dongle

El estado inicial del mezclador explicaba gran parte del ruido observado.

La entrada de micrófono estaba configurada con aproximadamente **+27 dB de ganancia de captura**. Con esa ganancia el preamplificador barato parecía extremadamente ruidoso.

La configuración estable fue:

```bash
amixer -c Device sset 'Mic' 0dB cap
amixer -c Device sset 'Auto Gain Control' off
```

Con `Mic Capture = 0 dB` y `AGC = off`, el soplido subjetivo cayó drásticamente.

La lección fue simple: antes de juzgar el ruido de una interfaz conviene inspeccionar el mixer ALSA real.

```bash
amixer -c Device scontents
```

## Bias para micrófono electret

La entrada de micrófono del dongle no es una entrada de línea ni una entrada de instrumento.

Se midieron aproximadamente **2.4 V DC entre tip y sleeve**, correspondientes al bias o *plug-in power* previsto para alimentar un micrófono electret.

Para utilizar la entrada con una fuente externa se construyó un adaptador sencillo:

```text
fuente / guitarra activa
        │
        │ TIP
        ▼
      (-) 10 µF (+)
        │
        ▼
   tip entrada mic USB

SLEEVE ─────────────── SLEEVE
```

Se utilizó:

- hembra TS de 6.35 mm;
- capacitor electrolítico de 10 µF en serie con la señal;
- positivo del capacitor hacia el dongle USB;
- negativo hacia la fuente de audio;
- sleeve unido directamente a sleeve.

El capacitor bloquea el bias DC para que no llegue a la fuente conectada.

No transforma impedancias ni convierte mágicamente la entrada de micrófono en una entrada Hi-Z. Por eso esta adaptación resulta especialmente apropiada para una guitarra electroacústica o instrumento con **preamp activo**, cuya salida tolera mejor una carga baja que un pickup pasivo conectado directamente.

La prueba musical con la guitarra queda separada de la caracterización eléctrica: primero se comprobó que la entrada era estable y medible; después corresponde verificar nivel, headroom e impedancia con el instrumento real.

## El falso “ruido” de -36 dBFS

Con la ganancia en 0 dB apareció algo extraño: el oído percibía silencio, pero el vúmetro mostraba una señal continua alrededor de **-36 dBFS**.

Se grabaron diez segundos directamente con ALSA, sin MusE ni JACK:

```bash
arecord -D hw:CARD=Device,DEV=0 \
  -f S16_LE \
  -c 1 \
  -r 48000 \
  -d 10 \
  silencio-usb.wav
```

Las estadísticas originales fueron:

```text
DC offset   0.015080
Pk lev dB     -36.28
RMS lev dB    -36.43
```

El valor RMS coincidía prácticamente con el nivel de la propia continua. No era un piso de ruido audible de -36 dBFS: era **DC offset**.

Después de un high-pass de 30 Hz y descartando el primer segundo para evitar el transitorio de asentamiento del filtro:

```bash
sox silencio-usb.wav -n highpass 30 trim 1 8 stats
```

se obtuvo:

```text
DC offset  -0.000000
Pk lev dB     -71.12
RMS lev dB    -83.87
RMS Pk dB     -83.25
RMS Tr dB     -85.04
```

Es decir: el ruido AC real estaba aproximadamente **47 dB por debajo** de lo que sugería inicialmente el vúmetro.

Esta prueba es un buen ejemplo de por qué un medidor de nivel puede ser engañoso cuando existe offset DC.

## Eliminar DC dentro de MusE

La solución práctica es colocar como primer insert un filtro pasa-altos muy bajo.

Con **LSP Parametric Equalizer x8 Mono** se probó un high-pass en torno a **16–20 Hz**. Esa frecuencia está muy por debajo de la fundamental de una guitarra estándar y elimina eficazmente la continua sin modificar de manera relevante el contenido musical.

```text
USB-IN
   ↓
Input de MusE
   ↓
HPF 16–20 Hz
   ↓
resto de FX
```

El medidor de entrada del plugin puede continuar mostrando el offset porque está antes del filtro; lo importante es observar la salida.

## 50 Hz: el cuerpo como acoplador capacitivo

Tocar con el dedo el tip de un cable abierto conectado a esta entrada produce el clásico zumbido de red.

No hace falta tocar un conductor de 230 V. El cuerpo humano tiene capacitancia parásita respecto de la instalación eléctrica y se acopla al campo eléctrico alterno de 50 Hz.

```text
instalación 230 V / 50 Hz
          │
          │ capacitancia parásita
          ▼
       cuerpo
          │
        dedo
          │
         TIP
          │
       preamp
```

El analizador de espectro mostró claramente el pico de 50 Hz. Al atenuar la fundamental y aumentar la escala visual aparecieron múltiples componentes discretas adicionales: armónicos de red y otras interferencias presentes en el ambiente.

Este experimento permite ver en práctica algo que suele aprenderse sólo en teoría: una forma de onda periódica deformada puede expresarse como suma de componentes sinusoidales.

## El hub que no era inocente

En esta estación la topología USB resultó crítica.

La ThinkPad T520 tiene controladores USB distintos. La configuración estable separa:

```text
EHCI interno
   └── AudioLink Plus II
       JACK master

ExpressCard xHCI
   ├── controlador MIDI USB
   └── entrada USB auxiliar
```

Con los dispositivos conectados directamente a los puertos de la ExpressCard, la prueba quedó en **XRUN = 0**.

Luego se agregó un hub por comodidad mecánica. En la topología probada, el hub introdujo una cadena adicional de hubs y la entrada secundaria comenzó a comportarse de forma inestable durante una prueba.

El log de `alsa_in` mostró:

```text
err = -32
err = -11
err = -11
err = -11
...
```

Interpretación Linux/ALSA:

```text
-32  EPIPE   stream roto / XRUN
-11  EAGAIN  no hay datos disponibles en ese instante
```

Al desenchufar finalmente el dispositivo aparecieron `-19` (`ENODEV`), como era esperable porque el hardware ya no existía.

Volver a conectar MPK y dongle directamente a la ExpressCard eliminó la tormenta de XRUNs.

No se generaliza este resultado a todos los hubs USB. Sí se documenta una regla práctica:

> **En una estación de audio en tiempo real, la topología USB forma parte de la arquitectura y debe medirse.**

Para inspeccionarla:

```bash
lsusb -t
```

El puerto físico visible no alcanza para entender qué está compartiendo realmente un dispositivo. Hay que observar hubs, buses y controladores PCI.

## Integración operativa

El launcher de la estación automatiza las partes repetitivas del procedimiento:

- verifica que AudioLink y controlador MIDI principal estén en controladores USB separados;
- prepara la ThinkPad para modo DAW;
- deshabilita temporalmente cámara y Bluetooth internos;
- arranca JACK sobre la AudioLink cuando es necesario;
- detecta el dongle USB auxiliar;
- configura `Mic Capture = 0 dB` y `AGC = off`;
- arranca `alsa_in` como cliente `USB-IN` mono a la frecuencia de JACK;
- comprueba que `USB-IN:capture_1` exista;
- al salir, termina sólo el `alsa_in` que inició y restaura el hardware deshabilitado.

La automatización importa porque una estación reproducible no debería depender de recordar manualmente una docena de pasos antes de cada sesión.

## Qué se aprendió

Esta entrada auxiliar terminó siendo útil no por ser sofisticada, sino porque obligó a separar problemas que inicialmente parecían uno solo:

- ganancia analógica excesiva;
- bias DC para electret;
- offset DC generado por el camino de entrada;
- ruido AC real;
- clocks independientes;
- topología USB;
- hubs y controladores;
- ruido de red de 50 Hz;
- interferencia electromagnética del entorno.

El resultado es una entrada secundaria muy económica integrada sin sacrificar las entradas fijas de la AudioLink y, sobre todo, un procedimiento que puede reproducirse y verificarse con herramientas estándar de GNU/Linux.
