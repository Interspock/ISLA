# Preservación tecnológica musical

ISLA considera que una obra musical no está formada solamente por un archivo de audio final.

También forman parte de su historia los instrumentos, patches, bancos SysEx, secuencias, samples, soportes magnéticos, configuraciones, drivers, afinaciones, diagramas y procedimientos necesarios para volver a producir ese sonido.

## 1. Preservar comportamiento, no sólo objetos

Un sintetizador puede preservarse de varias maneras:

- conservando y reparando el hardware original;
- documentando su arquitectura;
- almacenando sus patches en formatos abiertos;
- preservando SysEx;
- capturando ROM cuando sea legal hacerlo;
- midiendo su comportamiento;
- recreando su síntesis en software libre.

ISLA 800 es un ejemplo de la última estrategia: cuando el Poly-800 original ya no está disponible, parte de su comportamiento puede reconstruirse a partir de documentación, análisis y material histórico.

## 2. Los patches son patrimonio

Una hoja manuscrita de parámetros puede ser tan importante como una cinta o un archivo WAV.

En el caso del Casio CZ-101, antiguos patches escritos en papel están siendo convertidos nuevamente en datos estructurados capaces de alimentar un librarian y, finalmente, el sintetizador real mediante SysEx.

```text
papel manuscrito
      ↓
interpretación
      ↓
parámetros estructurados
      ↓
librarian libre
      ↓ SysEx
CZ-101
      ↓
sonido recuperado
```

## 3. Preservar soportes antes de interpretarlos

Con medios antiguos —como los disquetes utilizados con el secuenciador Brother asociado al Ensoniq SQ-1— la prioridad inicial no debe ser modificar el contenido ni intentar convertirlo apresuradamente.

La secuencia recomendada es:

1. inspeccionar físicamente el soporte;
2. utilizar hardware confiable;
3. trabajar inicialmente en modo lectura;
4. obtener una imagen tan cercana al medio físico como sea posible;
5. conservar esa imagen sin modificaciones;
6. realizar análisis posteriores sobre copias;
7. documentar herramientas, parámetros y checksums.

Herramientas abiertas de captura de flujo magnético, como Greaseweazle, pueden resultar útiles cuando un controlador de disquetera convencional no puede representar correctamente el formato original.

## 4. Preferencia de formatos

Siempre que sea posible, ISLA favorece formatos simples, documentados y ampliamente implementados.

Ejemplos:

- MIDI SMF para secuencias;
- SysEx para bancos y patches cuando el instrumento lo soporte;
- WAV/FLAC para audio;
- JSON, CSV o texto para metadatos y estructuras propias;
- SVG/PNG para diagramas e imágenes;
- Markdown para documentación;
- Git para código y cambios textuales.

Un formato abierto no garantiza eternidad, pero aumenta enormemente las probabilidades de que el material pueda interpretarse en el futuro.

## 5. Evitar dependencias de activación

Un proyecto que necesita contactar un servidor externo para abrir un instrumento corre el riesgo de quedar incompleto cuando ese servidor desaparezca.

Por esa razón ISLA evita como componentes fundamentales:

- plugins con activación remota obligatoria;
- licencias temporales;
- gestores propietarios imprescindibles;
- almacenamiento exclusivamente cloud;
- formatos cuyo único lector sea una aplicación cerrada.

## 6. Preservar el contexto

Un archivo aislado puede no explicar cómo fue utilizado.

Cada pieza histórica debería acompañarse, cuando sea posible, por información como:

- instrumento de origen;
- fecha aproximada;
- autor o procedencia;
- programa/patch asociado;
- versión de firmware;
- canal MIDI;
- sample rate;
- cadena de efectos;
- equipo utilizado;
- notas sobre incertidumbres.

Esto es especialmente importante al reconstruir sonidos desde grabaciones históricas.

## 7. Checksums e inmutabilidad

Para imágenes de discos, ROM, bancos SysEx y capturas originales se recomienda generar checksums (por ejemplo SHA-256) y conservar una copia maestra de sólo lectura.

Las conversiones y experimentos deben hacerse sobre duplicados.

## 8. Fotografiar también es documentar

Las fotografías del proceso no cumplen sólo una función estética.

Pueden conservar:

- posición de conectores;
- cableado antes del desmontaje;
- referencias de placas;
- valores y orientación de componentes;
- estado previo a una reparación;
- etiquetas de ROM;
- routing físico;
- instrumentos y accesorios asociados.

Por eso el archivo visual de ISLA intenta conservar imágenes originales del proceso junto con pies explicativos.

## 9. El objetivo

La preservación en ISLA no busca congelar la tecnología en un museo.

Busca mantenerla **ejecutable**.

Un patch preservado debería poder volver a cargarse.
Una secuencia debería poder volver a sonar.
Un instrumento debería poder repararse.
Un formato debería poder estudiarse.
Un driver debería poder recompilarse.

Preservar, en este contexto, significa conservar la posibilidad de volver a hacer música.
