# Ensoniq SQ-1 y archivos históricos

El Ensoniq SQ-1 aporta a ISLA un problema diferente: no sólo preservar un instrumento, sino también recuperar el ecosistema de almacenamiento utilizado con él.

## Estado del instrumento

El SQ-1 continúa funcionando, aunque requiere mantenimiento:

- batería de respaldo agotada;
- primera tecla DO grave sin respuesta;
- revisión interna pendiente.

## Patches y secuencias históricas

Durante su uso original se almacenaron patches, bancos SysEx y secuencias mediante un secuenciador hardware Brother con disquetes de 3½ pulgadas.

Todavía se conservan tanto la unidad Brother como numerosos disquetes.

Estos soportes contienen material potencialmente irremplazable.

## Problema de formato

Los disquetes no eran legibles de forma convencional desde una PC, lo que sugiere un formato lógico o físico propio del equipo Brother.

La prioridad de ISLA es preservar primero el soporte y analizar después.

## Estrategia de recuperación

```text
disquete original
      ↓
inspección física
      ↓
lectura no destructiva
      ↓
imagen maestra
      ↓
checksum
      ↓
copias de trabajo
      ↓
análisis de formato
      ↓
extracción de SysEx / secuencias
```

El primer intento se realizará con una disquetera convencional bajo GNU/Linux. Si el controlador estándar no puede representar correctamente la codificación del medio, se evaluará captura de flujo magnético mediante herramientas abiertas como Greaseweazle.

## Principio de preservación

Los discos originales no deben utilizarse como superficie de experimentación.

Una vez obtenida una imagen confiable, cualquier análisis, conversión o reparación lógica debe realizarse sobre copias.

## Material visual pendiente

En `media/ensoniq/` se incorporarán:

- fotografías del SQ-1;
- interior y placas;
- batería;
- mecanismo de teclado durante la reparación;
- secuenciador Brother;
- disquetes originales;
- capturas de imágenes y herramientas de análisis.
