# MusE como centro de producción

MusE es actualmente el DAW alrededor del cual se está consolidando ISLA.

## Por qué MusE

La elección no surgió de una comparación puramente técnica. También respondió a una forma concreta de componer.

ISLA necesita un entorno que trate MIDI como material musical de primera clase y que permita integrar con naturalidad:

- patrones y fragmentos;
- edición piano roll;
- instrumentos hardware;
- instrumentos virtuales;
- audio multipista;
- routing;
- automatización;
- plugins libres.

Ardour fue probado extensamente y demostró una enorme solidez en audio, routing y mezcla, pero su paradigma de trabajo resultó menos natural para el flujo compositivo buscado.

La decisión fue simple:

> **No adaptar al músico al DAW; buscar un DAW que se adapte al músico.**

## Integración de hardware

Un sintetizador externo puede representarse mediante dos caminos simultáneos.

Ejemplo con el CZ-101:

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

Esto permite editar la interpretación MIDI y, cuando corresponde, grabar o monitorear la salida real del instrumento.

## Hardware y software en igualdad de condiciones

Dentro del proyecto pueden convivir:

- CZ-101;
- SQ-1;
- K1r;
- Surge XT;
- ISLA 800;
- Drumlabooh;
- kits Hydrogen;
- efectos LV2.

La procedencia tecnológica deja de ser la categoría principal. Cada fuente se transforma simplemente en un instrumento de la sesión.

## Routing como documentación

Uno de los objetivos futuros es documentar configuraciones reproducibles por instrumento:

- puerto MIDI;
- canal;
- entrada de audio;
- ganancia;
- latencia;
- retorno;
- efectos;
- patch/programa inicial.

Esto permitirá reconstruir una sesión sin depender de recordar manualmente el cableado y configuración utilizados.

## Material visual pendiente

En `media/muse/` se incorporarán capturas reales de:

- sesión general;
- pista MIDI del CZ-101;
- pista de retorno de audio;
- routing MIDI;
- routing JACK/audio;
- instrumentos virtuales;
- mezcla híbrida hardware/software.
