# MusE como centro de producción

MusE es actualmente el DAW alrededor del cual se está consolidando ISLA.

![Routing real del CZ-101 en MusE](../media/muse/01-cz-routing.jpg)

*Controlador MIDI, AudioLink, pista MIDI del CZ y retorno de audio conviven dentro del mismo ruteo.*

## Por qué MusE

ISLA necesita un entorno que trate MIDI como material musical de primera clase y que permita integrar patrones, piano roll, hardware, instrumentos virtuales, audio multipista, routing, automatización y plugins libres.

Ardour fue probado extensamente y demostró enorme solidez, pero su paradigma resultó menos natural para el flujo compositivo buscado.

> **No adaptar al músico al DAW; buscar un DAW que se adapte al músico.**

## Integración de hardware

```text
                 ┌──────────────┐
                 │    MusE      │
                 └──────┬───────┘
                        │ MIDI
                        ▼
                   Casio CZ-101
                        │ audio
                        ▼
                    AudioLink
                        │
                        ▼
                 pista audio MusE
```

Dentro de una misma sesión pueden convivir CZ-101, SQ-1, K1r, Surge XT, ISLA 800, Drumlabooh, kits Hydrogen y efectos LV2.
