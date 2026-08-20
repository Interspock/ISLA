# Arquitectura técnica de ISLA

Este documento describe la arquitectura conceptual de una implementación de ISLA. No pretende fijar una lista obligatoria de hardware o software: define capas, interfaces y criterios de diseño.

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
│ Interfaces USB / MIDI DIN / audio analógico         │
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

## 3. Audio

La interfaz principal utilizada durante el desarrollo es una **Midiplus AudioLink Plus II**.

El soporte GNU/Linux se consiguió extendiendo el driver libre Ozzy. Esto implicó incorporar soporte específico del dispositivo y corregir el camino MIDI/SysEx.

ISLA considera al driver parte de la estación. Si una pieza del stack no existe, puede formar parte del trabajo del proyecto implementarla en lugar de reemplazar hardware funcional.

## 4. MIDI

MIDI cumple varias funciones:

- interpretación desde controladores;
- secuenciación desde MusE;
- selección de programas;
- edición remota;
- transferencia SysEx;
- sincronización de instrumentos históricos con software moderno.

La topología física puede incluir MIDI USB y MIDI DIN 5 pines.

Un principio importante es mantener identificable cada destino por puerto y canal, evitando que la configuración quede implícita.

## 5. Hardware externo

### Casio CZ-101

- fuente sonora hardware;
- control MIDI;
- Program Change;
- SysEx;
- librarian Web MIDI separado del DAW;
- restauración física en curso;
- recuperación de patches históricos.

### Ensoniq SQ-1

- fuente sonora hardware;
- teclado potencialmente utilizable como controlador;
- preservación de patches y secuencias históricas;
- recuperación de soportes magnéticos pendiente.

### Kawai K1r

- módulo MIDI externo;
- edición remota desde GNU/Linux;
- mantenimiento físico del panel pendiente.

## 6. Instrumentos virtuales

ISLA intenta que los instrumentos sean independientes del DAW.

Ejemplos actuales:

- Surge XT;
- ISLA 800;
- Drumlabooh;
- kits Hydrogen;
- samples WAV;
- plugins LV2 y otros plugins libres compatibles.

La regla práctica es favorecer formatos que puedan conservarse localmente y reinstalarse sin depender de servidores de activación.

## 7. Jerarquía portable

Parte del entorno se organiza bajo:

```text
/opt/isla-audio/
├── ardour/          # legado/experimentos previos
├── muse/            # cuando corresponda
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

## 8. Flujo de señal híbrido

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
                           │
                           ▼
                         MusE
                           │
                      FX / mezcla
                           │
                           ▼
                         WAV
```

## 9. Reproducibilidad

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
- árbol `/opt/isla-audio`;
- repositorios y commits concretos cuando sea relevante.

## 10. Qué NO debe ser parte obligatoria de ISLA

ISLA no debería depender de:

- DRM;
- activaciones online permanentes;
- cuentas obligatorias para abrir proyectos locales;
- formatos que sólo pueda interpretar una aplicación propietaria sin especificación pública;
- drivers cerrados sin alternativa mantenible;
- servicios cloud necesarios para producir o recuperar una obra.

## 11. Principio arquitectónico

La arquitectura debe permitir reemplazar una capa sin destruir las demás.

Cambiar de DAW no debería destruir los patches.
Cambiar de computadora no debería inutilizar el hardware.
Cambiar de interfaz no debería convertir los proyectos en ilegibles.
Que desaparezca un fabricante no debería borrar un instrumento.

Ese desacoplamiento es una de las formas concretas de soberanía tecnológica que ISLA intenta construir.
