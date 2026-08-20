# ISLA — Historia y filosofía

## De la recuperación de instrumentos de los años 80 a la construcción de un entorno libre de producción musical

ISLA nació a partir de una necesidad concreta: construir una estación de producción musical moderna alrededor de instrumentos electrónicos de distintas épocas, sin abandonar el control sobre la tecnología utilizada.

El objetivo fue creciendo rápidamente. Ya no se trataba solamente de lograr que un sintetizador antiguo pudiera grabarse en una computadora o de encontrar un DAW que funcionara correctamente bajo GNU/Linux. La pregunta pasó a ser otra:

> **¿Es posible construir hoy un entorno completo de producción musical basado exclusivamente en software libre, estándares abiertos, hardware recuperable y conocimiento reproducible?**

ISLA es, hasta el momento, la respuesta práctica a esa pregunta.

El proyecto abarca hoy varias capas que normalmente se consideran independientes: hardware musical de las décadas de 1980 y 1990, interfaces de audio modernas, controladores MIDI, drivers de kernel, MIDI y SysEx, sintetizadores virtuales, instrumentos sampleados, plugins, estaciones de trabajo de audio, herramientas de edición y librarian, restauración y preservación de patches históricos e infraestructura GNU/Linux.

En algunos casos fue posible utilizar software libre existente. En otros hubo que adaptarlo. Y cuando una pieza fundamental no existía, se construyó.

El criterio que unifica todas esas decisiones es sencillo:

> **La computadora y los instrumentos deben estar al servicio del músico, y no el músico sometido a las restricciones de una plataforma, una licencia, un fabricante o un formato cerrado.**

---

## 1. Software libre como infraestructura

El software libre no se eligió simplemente por su precio.

Se eligió por algo más importante: **la posibilidad de entenderlo, conservarlo, modificarlo y reconstruirlo**.

Cada componente importante debería, idealmente, poder sobrevivir independientemente de la empresa o persona que lo creó.

GNU/Linux proporciona la base de ese modelo. Sobre ella pueden convivir ALSA, JACK, MIDI, LV2, plugins libres, SoundFonts, archivos WAV, SysEx, proyectos almacenables localmente, código fuente compilable y configuraciones reproducibles.

El objetivo final es que otra persona pueda reconstruir una ISLA dentro de diez o veinte años sin necesitar una cuenta de usuario en una plataforma comercial.

---

## 2. El estudio como una isla

El nombre **ISLA** representa bien la arquitectura.

La estación intenta ser autosuficiente. Los elementos fundamentales viven localmente. Los instrumentos pueden ejecutarse sin conexión a Internet. Los presets pueden conservarse como archivos. Los drivers forman parte del sistema. Los proyectos musicales no dependen de servicios externos.

Incluso la disposición del software intenta reflejar ese concepto. Se creó una jerarquía independiente bajo `/opt/isla-audio`, con áreas destinadas a DAW, plugins, instrumentos, samples, presets, archivos históricos, manifiestos y utilidades.

El objetivo no es sólo la organización. Es poder transportar la estación.

Una instalación debe poder documentarse, copiarse, versionarse o reconstruirse en otra computadora GNU/Linux sin comenzar nuevamente desde cero.

---

## 3. El hardware como patrimonio tecnológico

Una característica particular del proyecto es que no nació alrededor de instrumentos nuevos.

Parte importante de ISLA consiste precisamente en volver a integrar máquinas que durante décadas pertenecieron a otro mundo tecnológico:

- Casio CZ-101;
- Ensoniq SQ-1;
- Kawai K1r;
- controlador Akai MPK mini;
- módulos y sintetizadores hardware;
- Midiplus AudioLink Plus II.

Algunos todavía funcionan completamente. Otros necesitan reparación. Otros tienen botones endurecidos, baterías agotadas, memorias que ya no retienen patches o mecanismos que requieren mantenimiento.

ISLA considera que estas limitaciones no convierten al instrumento en obsoleto. Muchas veces sólo significan que hay que construir nuevamente el puente tecnológico que lo conecta con el presente.

---

## 4. Casio CZ-101: arqueología digital

Uno de los primeros grandes ejemplos fue el **Casio CZ-101**.

El instrumento utilizado en ISLA había sobrevivido físicamente durante décadas, pero presentaba varios problemas típicos de esa edad: suciedad, contactos deteriorados, portapilas afectado por derrames, memoria sin una conservación confiable, problemas mecánicos y componentes que requerían revisión.

Sin embargo, la electrónica principal seguía funcionando. Y, algo especialmente importante, también funcionaba MIDI.

A partir de allí comenzó una recuperación que fue tanto física como digital.

### Recuperar sonidos desde un papel

Había además otro tipo de patrimonio.

No sólo sobrevivió el sintetizador. Sobrevivieron **hojas escritas décadas atrás con parámetros de patches**.

Antes de que almacenar miles de presets fuera trivial, guardar un sonido podía significar copiar manualmente decenas de parámetros: DCO, DCW, DCA, envolventes, formas de onda, modulaciones, niveles y configuraciones de línea.

Esos papeles eran, en la práctica, código fuente escrito a mano para un sintetizador.

ISLA comenzó a convertir esa información nuevamente en datos digitales.

La idea dejó de ser solamente recrear un sonido manualmente desde el panel. Se construyó la base para representar los patches estructuradamente y volver a cargarlos mediante MIDI y SysEx.

De esa forma un papel escrito aproximadamente cuarenta años antes puede convertirse nuevamente en sonido.

### El librarian del CZ

De esa investigación surgió también un **librarian/editor para el Casio CZ-101**.

En lugar de depender de una aplicación propietaria abandonada o de software diseñado para otra plataforma, se desarrolló una aplicación web basada en tecnologías abiertas.

El navegador utiliza Web MIDI y puede comunicarse directamente con el sintetizador. Durante su desarrollo fue necesario investigar mensajes MIDI, Program Change, estructura SysEx del CZ, recepción y transmisión de patches y representación de parámetros.

El librarian no intenta reemplazar al CZ. Hace exactamente lo contrario:

> **permite que el CZ siga siendo utilizable.**

---

## 5. MIDI como puente generacional

Uno de los elementos que hicieron posible esta integración es MIDI.

El protocolo apareció comercialmente en 1983. Más de cuarenta años después continúa permitiendo que un Casio de 1985, un Ensoniq posterior, un Kawai, un controlador USB moderno y una computadora GNU/Linux de 64 bits puedan comunicarse.

Para ISLA, MIDI representa algo fundamental: **el valor de los estándares abiertos y duraderos**.

No fue necesario convertir los sintetizadores históricos a una tecnología nueva. La infraestructura moderna se adaptó a un protocolo que ya estaba correctamente diseñado para ser interoperable.

---

## 6. AudioLink Plus II: cuando el driver no existe

El desafío más profundo apareció con la interfaz **Midiplus AudioLink Plus II**.

El dispositivo podía utilizarse en otros sistemas operativos mediante software del fabricante, pero no existía una solución funcional equivalente bajo GNU/Linux para todas sus capacidades.

Para muchos proyectos este punto habría significado simplemente cambiar la interfaz.

ISLA tomó el camino opuesto.

> **Si el hardware funciona, ¿por qué descartarlo?**

La solución fue trabajar sobre **Ozzy**, un driver libre, y extenderlo para soportar correctamente la AudioLink Plus II.

Esto implicó trabajo a nivel kernel y USB: identificación del dispositivo, análisis de endpoints, inicialización, handshake, audio USB, MIDI, buffering y transmisión SysEx.

Durante las pruebas apareció además un problema especialmente interesante: mensajes SysEx relativamente largos eran truncados. Un paquete transmitido podía llegar parcialmente al otro extremo. Eso hacía imposible confiar en la interfaz para administrar patches completos de sintetizadores.

Se investigó entonces el camino completo de transmisión MIDI dentro del driver. Finalmente se reescribieron partes de la gestión MIDI, introduciendo buffering, cola y manejo correcto de las fases de transmisión.

Se desarrollaron además herramientas de prueba de loopback y captura.

Los resultados terminaron siendo concluyentes: transferencias de 80, 512, 1024 y 4096 bytes podían completarse con **TX == RX**.

El hardware que inicialmente no tenía soporte satisfactorio bajo Linux terminó formando parte nativa de la estación.

No mediante un wrapper cerrado. No ejecutando un driver de otro sistema operativo. Mediante **código libre integrado en GNU/Linux**.

Ese caso estableció otro principio de ISLA:

> **La falta de soporte comercial no convierte automáticamente al hardware en basura electrónica.**

Aquí la libertad del software también se convierte en **sustentabilidad del hardware**.

---

## 7. ISLA 800: cuando tampoco existe el sintetizador

Otra pieza importante del proyecto nació de una ausencia distinta.

Durante los años ochenta se utilizó un **Korg Poly-800** que pertenecía a Eduardo, compañero y amigo de aquella época. El instrumento ya no está disponible, pero quedaron recuerdos de su sonido e incluso grabaciones antiguas realizadas mediante una cadena extremadamente precaria desde el punto de vista actual: Poly-800 → consola → parlantes de sala → micrófono de cámara de video → grabación mono.

Desde ese material se comenzó un proceso inverso.

No se trataba simplemente de encontrar un plugin parecido. La pregunta era:

> **¿podemos reconstruir conceptualmente el instrumento?**

Así nació **ISLA 800**.

ISLA 800 intenta reproducir la arquitectura sonora del Poly-800 a partir de documentación, ROM disponibles, análisis de parámetros, estructura de DCO, formas de onda, filtro, envolventes, detune y comportamiento polifónico.

La idea no es samplear un Poly-800. Es modelar su forma de producir sonido.

El proyecto avanzó además con un requisito importante: debía ser suficientemente eficiente para ejecutarse en hardware relativamente modesto. Para ello se construyó un benchmark específico del núcleo DSP y se comprobó que el instrumento podía ejecutar varias voces a muchas veces tiempo real.

La eficiencia del software también forma parte del diseño de ISLA.

### Una reconstrucción con dimensión humana

Uno de los sonidos históricos conservados provenía de aquel Poly-800 de Eduardo. Existía el recuerdo de que posiblemente había sido construido a partir del preset 78 del instrumento.

Décadas después, una pequeña grabación aislada permite nuevamente analizar ataque, espectro, envolvente, filtro, modulación y carácter de los osciladores.

La ingeniería digital se convierte aquí en una forma de arqueología sonora.

No se intenta sólo emular un sintetizador. Se intenta recuperar **un sonido concreto de una experiencia musical que ya ocurrió**.

El código termina siendo también una herramienta de memoria.

---

## 8. Ensoniq SQ-1 y los disquetes como cápsulas de tiempo

Otro instrumento incorporado al proyecto es el **Ensoniq SQ-1**.

El sintetizador todavía funciona, aunque su batería de respaldo está agotada y una tecla grave necesita reparación.

Pero su historia abre otro frente de preservación.

Durante los años en que se utilizaba el SQ-1, los patches y secuencias se almacenaban mediante un secuenciador hardware **Brother** con disquetes de 3½ pulgadas.

Todavía se conservan el Brother, los disquetes, patches, bancos SysEx y secuencias.

El problema es que el formato utilizado por el Brother no era directamente legible por una PC convencional.

Por eso se comenzó a estudiar la preservación de esos discos. La estrategia prevista sigue la misma lógica aplicada al resto de ISLA:

1. intentar leerlos mediante hardware convencional bajo GNU/Linux;
2. evitar cualquier operación que pueda alterar los originales;
3. realizar imágenes de bajo nivel;
4. utilizar, si fuera necesario, herramientas abiertas como Greaseweazle;
5. estudiar posteriormente el formato lógico.

Aquellos disquetes ya no son simplemente medios antiguos. Son **contenedores de ejecución musical histórica**.

---

## 9. Kawai K1r: otra interfaz para otra época

El **Kawai K1r** presentaba un problema mucho más sencillo pero conceptualmente similar.

Su electrónica funciona correctamente, pero los botones frontales comienzan a endurecerse. Antes de depender exclusivamente del panel físico se buscó una alternativa de edición remota.

Se probó un editor basado en Java bajo GNU/Linux y se confirmó que el K1r podía configurarse por MIDI.

Otra vez, el instrumento original permanece intacto. La computadora extiende su interfaz.

> **Modernizar la interacción sin destruir la identidad del instrumento.**

---

## 10. Instrumentos virtuales libres

ISLA tampoco pretende ser exclusivamente un museo de hardware.

La estación necesita instrumentos modernos, pero intenta mantener el mismo criterio de libertad.

Entre los componentes utilizados o evaluados se encuentran Surge XT, Drumlabooh, kits Hydrogen, instrumentos propios, ISLA 800 y samples almacenados en formatos convencionales.

La batería, por ejemplo, puede construirse mediante Drumlabooh cargando kits abiertos en lugar de depender de una colección cerrada asociada a un DAW determinado.

Esto tiene una ventaja importante:

> **Los instrumentos no pertenecen al DAW. El DAW es solamente el lugar donde se conectan.**

---

## 11. La búsqueda del DAW correcto

La primera integración profunda se realizó alrededor de **Ardour 9.7**.

Ardour demostró ser extremadamente sólido para grabación, audio multipista, routing, JACK, plugins, automatización y edición profesional.

Sin embargo apareció una cuestión esencial: una herramienta puede ser técnicamente excelente y, aun así, no representar correctamente la forma de pensar musical del usuario.

El flujo buscado para ISLA es fuertemente orientado a patrones, fragmentos musicales, loops, secciones, construcción progresiva, MIDI manipulable e instrumentos hardware y virtuales conviviendo.

Se experimentó ampliamente con Ardour, pero el flujo resultaba demasiado indirecto para esa forma particular de composición.

Eso llevó a una decisión importante:

> **No adaptar el músico al DAW; buscar un DAW que se adapte al músico.**

---

## 12. MusE como centro actual de ISLA

La exploración condujo finalmente a **MusE**.

MusE comparte varios principios importantes con ISLA: GNU/Linux como plataforma natural, software libre, fuerte orientación MIDI, audio multipista, instrumentos virtuales, integración con JACK, routing, soporte de hardware externo, edición piano roll y automatización.

Las primeras pruebas ya permitieron montar el flujo elemental:

**control MIDI → sintetizador hardware → retorno de audio → pista del DAW.**

Por ejemplo, para el CZ-101 se puede disponer una pista MIDI que transmite las notas y una pista de audio que recibe físicamente la salida del CZ mediante la AudioLink.

Así el sintetizador hardware aparece dentro del proyecto casi como si fuera otro instrumento virtual. La diferencia es que su motor fue fabricado en 1985 y está dentro de una caja con teclado.

MusE representa, por el momento, el punto donde convergen todos los caminos anteriores.

---

## 13. El estudio híbrido

ISLA termina cuestionando también la vieja división entre hardware y software.

Desde el DAW pueden coexistir un CZ-101 real, un SQ-1 real, un K1r real, Surge XT, ISLA 800, baterías sampleadas, efectos LV2, MIDI USB, MIDI DIN y audio analógico convertido por la AudioLink.

Musicalmente todos son simplemente fuentes sonoras.

Cada pista sabe dónde envía MIDI, de dónde recibe audio, qué canal utiliza, qué efectos procesa y dónde termina su señal.

El estudio deja así de estar organizado alrededor de la procedencia tecnológica del instrumento.

Está organizado alrededor de **la música**.

---

## 14. La computadora tampoco debe ser especial

Otro objetivo fue evitar la necesidad de una workstation costosa.

ISLA se desarrolló y probó en computadoras ThinkPad de generaciones anteriores, entre ellas T520 y X230.

Un estudio digital no debería quedar inutilizable cada pocos años simplemente porque cambió el mercado informático.

GNU/Linux permite aprovechar hardware maduro durante muchísimo tiempo. Y cuando el software propio se diseña con criterios razonables de eficiencia, resulta posible ejecutar sintetizadores, MIDI, audio multipista y efectos en equipos que el mercado convencional considera antiguos.

Por eso los benchmarks de DSP, el control de carga y la selección cuidadosa de plugins forman parte del proyecto.

---

## 15. Reproducibilidad

Uno de los objetivos centrales de ISLA es que el resultado no quede atrapado en una única máquina.

Para eso cada descubrimiento intenta convertirse en alguno de estos elementos:

- código fuente;
- repositorio Git;
- script;
- configuración;
- patch;
- archivo SysEx;
- documentación;
- manifiesto;
- procedimiento reproducible.

Si mañana desaparece la instalación actual, debería ser posible reconstruirla.

No necesariamente pulsando un único botón, pero sí siguiendo información que permanezca bajo control de quien la utiliza.

---

## 16. Preservar también los errores y los descubrimientos

ISLA se construyó mediante experimentación.

Hubo mensajes SysEx truncados, dispositivos USB que no respondían como indicaba la expectativa inicial, problemas de routing, DAWs que resultaban conceptualmente incómodos, sintetizadores que encendían pero no entregaban audio, botones endurecidos, baterías agotadas y módulos de kernel que requerían cambios.

Cada problema aportó información.

En un ecosistema cerrado, cuando algo falla el usuario normalmente espera una actualización del fabricante.

En un ecosistema abierto, el fallo puede investigarse.

Puede observarse USB. Puede estudiarse ALSA. Puede modificarse un driver. Puede instrumentarse el código. Puede compilarse nuevamente. Puede medirse.

La estación deja entonces de ser una caja negra.

---

## 17. Conocimiento en lugar de dependencia

Una plataforma propietaria ofrece comodidad a cambio de dependencia.

ISLA intenta obtener comodidad **a partir del conocimiento**.

Para lograr que una interfaz funcione hubo que entender parte de USB y del kernel. Para recuperar un sintetizador hubo que estudiar SysEx. Para recrear otro hubo que estudiar DSP. Para rescatar disquetes probablemente será necesario estudiar codificación magnética y formatos antiguos.

Pero cada dificultad resuelta produce algo que permanece.

La licencia de software puede desaparecer. El conocimiento aprendido no.

---

## 18. Una cadena tecnológica de cuarenta años

En ISLA conviven tecnologías creadas en épocas extraordinariamente diferentes.

Un posible recorrido de una nota es hoy:

**Akai MPK mini → USB MIDI → GNU/Linux → MusE → ALSA/JACK → driver libre de AudioLink → MIDI DIN → Casio CZ-101 → síntesis Phase Distortion de 1985 → señal analógica → AudioLink → conversión A/D → JACK → MusE → plugin libre → archivo WAV.**

La nota atraviesa más de cuarenta años de historia tecnológica.

Y prácticamente cada transición utiliza estándares documentables.

Eso es probablemente una de las mejores demostraciones de por qué los estándares abiertos importan.

---

## 19. Soberanía tecnológica aplicada a la música

La palabra soberanía puede parecer excesivamente grande para hablar de un estudio musical, pero describe con precisión lo que se busca.

Soberanía significa que el músico puede conservar sus proyectos, comprender sus herramientas, reparar su infraestructura, copiarla, modificarla, construir nuevos instrumentos, recuperar máquinas antiguas, decidir cuándo actualizar y decidir cuándo **no** actualizar.

No significa rechazar la tecnología moderna.

Significa utilizarla sin entregar el control.

---

## 20. ISLA como proceso

ISLA no está terminado. Probablemente nunca lo esté.

Actualmente MusE aparece como candidato a convertirse en su centro de producción, pero el proyecto continuará evolucionando alrededor de él.

Quedan muchas líneas abiertas: completar la restauración del CZ-101, continuar el librarian y la biblioteca de patches, reconstruir sonidos históricos, evolucionar ISLA 800, reparar el SQ-1, preservar los disquetes Brother, recuperar secuencias y bancos SysEx, limpiar el K1r, ampliar la biblioteca de baterías, documentar routing y consolidar `/opt/isla-audio`.

Pero el problema fundamental ya fue resuelto.

Las piezas pueden comunicarse.

---

## 21. Lo que finalmente se construyó

Visto retrospectivamente, ISLA comenzó pareciendo un proyecto para armar una computadora destinada a hacer música.

Terminó convirtiéndose en algo bastante distinto.

Se construyó:

**un driver cuando no había driver;  
un sintetizador cuando ya no estaba el sintetizador;  
un librarian cuando la interfaz original resultaba insuficiente;  
una representación digital cuando sólo quedaba un papel;  
un puente hacia disquetes cuando el formato dejó de ser común;  
una infraestructura cuando los programas aislados no alcanzaban.**

Y alrededor de todas esas piezas apareció una idea coherente.

La tecnología musical puede conservarse. Puede estudiarse. Puede repararse. Puede reconstruirse.

Y puede hacerse sin pedir permiso.

---

## Conclusión

ISLA demuestra que una estación musical libre no tiene por qué ser una versión limitada de un estudio propietario.

Puede ser exactamente lo contrario.

Puede integrar máquinas que ningún fabricante moderno contempla. Puede mantener vivo hardware de cuatro décadas. Puede incorporar instrumentos diseñados desde cero. Puede utilizar protocolos que sobrevivieron generaciones. Puede permitir modificar hasta el último nivel del sistema, desde una envolvente de sintetizador hasta el código del driver USB.

Y, sobre todo, puede garantizar algo que resulta cada vez más extraño en la informática contemporánea:

> **que quien crea la música sea también dueño efectivo de las herramientas con las que la crea.**

ISLA no persigue la nostalgia tecnológica.

Utiliza el pasado, el presente y el software libre para construir continuidad.

Un Casio CZ-101 de los años ochenta, un patch recuperado de una hoja manuscrita, una Midiplus cuyo driver hubo que escribir, un Poly-800 reconstruido en código y MusE ejecutándose sobre GNU/Linux pueden formar parte del mismo instrumento ampliado.

Ese instrumento es ISLA.

Y mientras existan el código fuente, los estándares abiertos, la documentación y el conocimiento necesario para reconstruirla, **ISLA no pertenece a una computadora concreta ni a una versión concreta de un programa**.

Pertenece a quien pueda comprenderla, copiarla y volver a hacerla funcionar.
