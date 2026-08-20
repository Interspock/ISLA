# Archivo visual de ISLA

Este directorio reúne fotografías, capturas y diagramas originales utilizados para documentar el proyecto.

La prioridad es utilizar **material propio del proceso real** antes que imágenes promocionales o fotografías tomadas de Internet.

Las imágenes no cumplen sólo una función decorativa: deben ayudar a conservar información técnica e histórica.

## Estructura prevista

```text
media/
├── cz101/
│   ├── exterior
│   ├── interior
│   ├── placas
│   ├── cartucho
│   ├── patches-manuscritos
│   └── librarian
├── audiolink/
│   ├── hardware
│   ├── usb-debug
│   ├── driver
│   └── sysex-loopback
├── isla800/
│   ├── arquitectura
│   ├── interfaz
│   ├── benchmark
│   └── analisis-sonoro
├── ensoniq/
│   ├── sq1
│   ├── interior
│   ├── brother
│   └── diskettes
├── midi/
│   ├── midi-thru
│   └── topologia
└── muse/
    ├── overview
    ├── midi-routing
    ├── audio-routing
    └── sesiones
```

Git no conserva directorios vacíos, por lo que las carpetas se irán creando al incorporar las primeras imágenes.

## Criterios para cada imagen

Cada fotografía debería tener, cuando sea relevante:

- fecha aproximada;
- dispositivo;
- etapa del proceso;
- breve explicación;
- autor/fuente;
- licencia;
- advertencia si la imagen contiene información histórica cuya interpretación todavía es incierta.

El pie de imagen debe explicar **por qué importa**, no limitarse a repetir lo que se ve.

Ejemplo:

```markdown
![Hoja original de patches del CZ-101](media/cz101/patches-originales.jpg)

*Anotaciones conservadas desde los años ochenta. Estos parámetros,
escritos originalmente para conservar sonidos sin almacenamiento digital
externo, están siendo transformados nuevamente en datos estructurados y
patches SysEx.*
```

## Fotografías de placas y reparaciones

En fotografías internas conviene, cuando sea posible:

1. tomar una vista general antes de desmontar;
2. fotografiar conectores y orientación de cables;
3. tomar primeros planos de referencias de placas y componentes;
4. conservar una imagen del estado original antes de limpiar o reparar;
5. documentar el estado posterior.

Estas fotografías pueden ser útiles años después para reconstruir conexiones o revisar una reparación.

## Imágenes externas

Si fuera necesario utilizar una imagen ajena, debe documentarse explícitamente su fuente y licencia.

No se incorporarán automáticamente imágenes comerciales encontradas en la web sólo porque estén públicamente accesibles.

## Licencia

Salvo indicación contraria, las fotografías y diagramas propios incorporados al proyecto se publicarán bajo **CC BY-SA 4.0**, igual que la documentación.
