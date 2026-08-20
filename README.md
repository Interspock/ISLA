# ISLA

## Una estación libre de producción musical

**ISLA** es un proyecto para construir una estación de producción musical completa sobre GNU/Linux utilizando **software libre, estándares abiertos, hardware reparable y conocimiento reproducible**.

No nació como una distribución ni como una colección de plugins. Nació intentando volver a integrar instrumentos reales de distintas épocas —Casio CZ-101, Ensoniq SQ-1, Kawai K1r y otros— con una estación moderna sin depender de sistemas operativos propietarios, drivers cerrados, activaciones remotas ni formatos cautivos.

En el camino hubo que hacer bastante más que instalar un DAW:

- recuperar y diagnosticar sintetizadores de los años 80 y 90;
- convertir anotaciones manuscritas de patches en datos nuevamente utilizables;
- desarrollar un librarian Web MIDI para el Casio CZ-101;
- extender un driver libre de kernel para que una Midiplus AudioLink Plus II pudiera funcionar correctamente bajo GNU/Linux;
- corregir transmisión MIDI/SysEx hasta obtener transferencias de miles de bytes sin corrupción;
- reconstruir en software la arquitectura de un Korg Poly-800 perdido, dando origen a **ISLA 800**;
- estudiar la recuperación de bancos, secuencias y disquetes históricos del Ensoniq SQ-1;
- integrar hardware, instrumentos virtuales libres y audio multipista alrededor de **MusE**.

El resultado es una idea más amplia que una configuración particular:

> **La computadora y los instrumentos deben estar al servicio del músico, y no el músico sometido a las restricciones de una plataforma, una licencia, un fabricante o un formato cerrado.**

---

## Principios

ISLA intenta sostener cinco principios:

1. **Software libre**  
   Las piezas esenciales deben poder estudiarse, modificarse, compilarse y conservarse.

2. **Estándares abiertos**  
   MIDI, SysEx, WAV, ALSA, JACK, LV2 y otros formatos documentables permiten que tecnologías de décadas diferentes sigan dialogando.

3. **Reparabilidad**  
   La falta de soporte comercial no convierte automáticamente al hardware en residuo electrónico.

4. **Preservación**  
   Un patch, una secuencia, un sonido o un instrumento también son patrimonio tecnológico y musical.

5. **Reproducibilidad**  
   Cada descubrimiento importante debería terminar convertido en código, documentación, scripts, presets, imágenes de disco, archivos SysEx o procedimientos reconstruibles.

---

## La ISLA actual

```text
Controladores MIDI / secuencias
            │
            ▼
          MusE
            │
     ┌──────┴───────────────┐
     │                      │
     ▼                      ▼
Hardware MIDI          Instrumentos libres
CZ-101                 Surge XT
SQ-1                   ISLA 800
K1r                    Drumlabooh
     │                      │
     ▼                      │
Audio analógico             │
     │                      │
     └────► AudioLink ◄─────┘
               │
          driver libre
               │
          ALSA / JACK
               │
            MusE
```

La implementación concreta puede cambiar. La filosofía no depende de poseer los mismos instrumentos.

Otra persona puede construir su propia ISLA con otro hardware, otra interfaz u otros sintetizadores mientras conserve los mismos principios de libertad, interoperabilidad, preservación y control local.

---

## Documentación

- [Historia y filosofía](HISTORY.md)
- [Arquitectura técnica](ARCHITECTURE.md)
- [Preservación tecnológica musical](PRESERVATION.md)
- [Casio CZ-101](docs/cz101.md)
- [Midiplus AudioLink Plus II](docs/audiolink.md)
- [ISLA 800 / Poly-800](docs/isla800.md)
- [Ensoniq SQ-1 y archivos históricos](docs/ensoniq-sq1.md)
- [MusE como centro de producción](docs/muse.md)
- [Archivo visual](media/README.md)

---

## Una cadena tecnológica de más de cuarenta años

Una nota dentro de ISLA puede recorrer actualmente algo parecido a esto:

```text
Akai MPK mini
    ↓ USB MIDI
GNU/Linux
    ↓
MusE
    ↓ ALSA/JACK
AudioLink + driver libre
    ↓ MIDI DIN
Casio CZ-101
    ↓ síntesis Phase Distortion
señal analógica
    ↓
AudioLink
    ↓ conversión A/D
JACK
    ↓
MusE
    ↓
WAV
```

Una tecnología musical de 1985 puede convivir con software escrito hoy porque las capas que las conectan pueden entenderse y conservarse.

---

## Estado

ISLA está en desarrollo activo. Actualmente MusE es el centro de producción que se está consolidando, mientras continúan la restauración y documentación del hardware, la evolución de los instrumentos propios y la recuperación de material histórico.

Este repositorio documenta **el proceso**, incluyendo problemas, experimentos, decisiones y resultados.

---

## Licencias

La documentación, diagramas y fotografías propias de este repositorio se publican bajo **Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)** salvo indicación contraria.

Los proyectos de software enlazados desde ISLA conservan sus respectivas licencias de software libre.

---

> **No se trata solamente de hacer música con software libre. Se trata de garantizar que podamos seguir haciéndola dentro de cuarenta años.**
