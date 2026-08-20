# ISLA 800

ISLA 800 es una reconstrucción libre inspirada en la arquitectura del Korg Poly-800 y, al mismo tiempo, un proyecto de preservación sonora.

## Origen

Durante los años 80 se utilizó un Poly-800 perteneciente a Eduardo, compañero y amigo de aquella etapa musical.

El instrumento original ya no está disponible, pero sobrevivieron recuerdos, referencias y grabaciones históricas. Algunas fueron registradas mediante una cadena extremadamente rudimentaria:

```text
Poly-800
   ↓
consola
   ↓
parlantes de sala
   ↓
micrófono de cámara de video
   ↓
grabación mono
```

Aun con esas limitaciones, ese audio contiene información suficiente para intentar reconstruir características importantes del sonido.

## Objetivo

ISLA 800 no busca samplear un Poly-800.

Busca modelar su forma de producir sonido a partir de:

- arquitectura de osciladores;
- formas de onda;
- mezcla y niveles;
- filtro;
- envolventes;
- modulación;
- detune;
- comportamiento polifónico;
- documentación y material de ROM disponible para estudio.

## El patch 78

Existe el recuerdo de que uno de los sonidos históricos utilizados por Eduardo pudo haber sido construido a partir del preset 78 de fábrica.

Una grabación aislada de ese sonido se está utilizando como referencia auditiva para analizar ataque, brillo, envolvente, filtrado, modulación y comportamiento general.

Esta parte del proyecto tiene una dimensión distinta a la simple emulación:

> **el código se convierte también en una herramienta de memoria.**

## Eficiencia

ISLA busca poder ejecutarse sobre hardware informático modesto. Por eso ISLA 800 incluye un benchmark específico del núcleo DSP.

Las pruebas realizadas durante el desarrollo mostraron ejecución de múltiples voces a muchas veces tiempo real, dejando margen suficiente para utilizarlo dentro de una sesión completa de producción.

La eficiencia forma parte de la filosofía del proyecto: no resolver mediante fuerza bruta computacional aquello que puede resolverse diseñando mejor el software.

## Material visual pendiente

En `media/isla800/` se incorporarán:

- referencias históricas del instrumento cuando su licencia lo permita;
- material propio relacionado con la reconstrucción;
- arquitectura del motor en SVG;
- capturas del instrumento;
- benchmarks;
- comparación de parámetros;
- espectros o mediciones de sonidos históricos y reconstruidos.
