# Devolución — Ejercicios de la Clase 3 (patrones)

## Titular: 7 de 7 con el patrón correcto

Lo primero y más importante: **acertaste el patrón en los siete**, incluidos los tres que mandaste solo con el nombre y sin justificar (Decorator del ej. 3 y los dos Factory Method de los ej. 4 y 7). O sea: **no elegís mal, lo que te falta es la frase que te da la certeza**. Eso se arregla con dos desempates, no con estudiar siete patrones de nuevo (están al final, en "Para llevarte").

Un dato que conviene que veas: los que dudaste fueron los **dos Factory Method** y el **Decorator**; los que justificaste con seguridad fueron los **dos Strategy**, el Adapter y el Observer. Con **Strategy no tuviste ningún problema** —lo clavaste dos veces—, así que tu duda con Factory Method **no es que lo confundas con Strategy**. Lo que todavía no tenés firme es el **modelado** de Factory Method: en el ej. 7 se te cruzaron los roles (creador vs producto) y en el ej. 4 faltó el método fijo. Ahí es donde reforzamos.

---

## Ejercicio 1 — Préstamo con fórmulas de interés → **Strategy** ✅

Lo que dibujaste: `Prestamo` compone `PrestamoStrategy` con `setPrestamoStrategy()`, y `TasaFija / TasaVariable / TasaPromo` implementan la interfaz. **Correcto y bien justificado** ("cambiar el comportamiento sin recrearse").

**A pulir:**
- `calcularInteres()` está devolviendo `void`. Un cálculo tiene que **devolver algo** → `calcularInteres(capital, plazo): number`.

---

## Ejercicio 2 — Tablero de trading → **Observer** ✅

Lo que dibujaste: `AppTrading` con la lista `observador: Observador[]` y `agregarObservador()`; la interfaz `Observador` con `actualizacion()`; y `TotalCartera / Alertas / Grafico` implementándola. **Correcto y bien justificado.**

**A pulir:**
- El enunciado pide **poder quitar vistas mientras la app corre**, y solo tenés `agregarObservador()`. Falta el *detach*: agregá `quitarObservador(o: Observador)`.
- Conviene un método `notificar()` explícito que recorra la lista y llame a `actualizacion()` de cada uno (hoy queda implícito en `cambioLaAccion()`).

---

## Ejercicio 3 — Mensaje cifrado/comprimido/firmado → **Decorator** ✅ (el mejor de los 7)

Lo que dibujaste: `IMensaje` con `contenido()`; `Mensaje` como componente concreto; `DecoradorMensaje` **abstracto** que **implementa** `IMensaje` *y* **contiene** un `IMensaje`; y `Firmado / Comprimido / Cifrado` heredando de él. **Está perfecto de libro.** Esa doble relación del decorador (ES un `IMensaje` y a la vez TIENE un `IMensaje`) es la firma del patrón, y la clavaste.

> **Este lo dudaste y era el más sólido. Mandalo con confianza.** El desempate que te faltó escribir: es **Decorator y no Adapter** porque las capas **mantienen** la interfaz `contenido()` y le **suman**; Adapter la **cambiaría** para traducir algo que no encaja.

---

## Ejercicio 4 — Framework de ventanas modales → **Factory Method** ✅

Lo que dibujaste: `FabricaFramework` (creador abstracto) con `crearContenido(): Contenido`; `CreadorAlerta / CreadorLogin / CreadorFormulario`; y `Contenido` como producto con `Alerta / Login / Formulario`. **Patrón correcto.**

**A pulir (importante):**
- Falta **el método fijo que usa la fábrica**. El enunciado dice que "el procedimiento para mostrar la ventana es idéntico": ese `mostrar()` (crear → posicionar → animar) va **dentro de `FabricaFramework`** y adentro llama a `crearContenido()`. Sin ese método fijo, no se ve *para qué* existe la fábrica — que es justo lo que justifica el patrón.

---

## Ejercicio 5 — Módulo de reportes con servicio externo → **Adapter** ✅

Lo que dibujaste: `FormatoAdapter implements FuenteDatos`, compone `NuevoFormato` y traduce `obtener()` → `fetchRecords()`. **Correcto y bien justificado** ("recibir otro formato y no reescribirlo").

**A pulir:**
- `obtener()` debería devolver `Registro[]`, no `void`. El adapter **traduce el formato**, y eso se ve en el tipo de retorno: entra `fetchRecords()` con otros nombres de campo, sale `Registro[]` con los tuyos.

---

## Ejercicio 6 — Navegación GPS (ruta rápida/corta/sin peajes) → **Strategy** ✅

Lo que dibujaste: `Gps` compone `GpsStrategy` con `setGpsStrategy()`; `EvitarPeaje / MasCorta / MasRapida` implementan `ejecutar() / calcularCosto() / calcularTiempo()`. **Correcto y bien justificado** ("cambios sin reescribir").

**A pulir:**
- Mismo detalle de tipos: pensá los retornos reales (`obtenerRuta(): Ruta`) en vez de `void`. Una estrategia que "calcula la ruta" tiene que **devolver la ruta**.

---

## Ejercicio 7 — Oleadas de enemigos por nivel → **Factory Method** ✅ (con roles cruzados)

Lo que dibujaste: `FabricaEnemigo` con `crearEnemigos()`; `CreadorNivelCueva / Bosque / Castillo`; y una interfaz `Enemigos` con `posicionDeAparicion() / hacerAparecer() / activarIA()`, realizada por `NivelCueva / Bosque / Castillo`. **Patrón correcto, pero mezclaste quién es el producto.**

**A pulir (importante):**
- El **producto** debería ser el **enemigo** (`Enemigo` como interfaz, con `Lobo / Murcielago / Caballero`), no el nivel. Lo que se **crea** es el enemigo.
- El **creador** es `Nivel`, y ahí va el **método fijo** `lanzarOleada()` (posición → aparecer → activar IA) + el abstracto `crearEnemigo()`. En tu diagrama esos métodos de oleada quedaron pegados al producto; van en el creador.
- Regla para no cruzarlos: *"¿qué cosa fabrica el patrón?"* → esa es el **producto**. Acá fabricás enemigos.

---

## Para llevarte

1. **Sos preciso eligiendo — 7 de 7.** El déficit es de *justificación*, no de criterio. Cuando dudás, igual acertás; solo te falta poder explicar por qué.
2. **Enfocá dos cosas puntuales, no siete patrones:**
   - **Modelar bien Factory Method:** quién es el **creador** (lleva el **método fijo** que usa la fábrica), quién es el **producto** (lo que se fabrica), sin cruzarlos.
   - **Decorator vs Adapter** (los dos envuelven). *Adapter **cambia** la interfaz (traduce); Decorator la **mantiene** (suma).*
3. **Vicio técnico repetido:** varios métodos quedaron en `void` cuando deberían **devolver** algo; y en los dos Factory Method **falta el método fijo** (`mostrar()`, `lanzarOleada()`) que usa la fábrica — que es justo lo que justifica el patrón.

---

# Ejercicios de refuerzo

Pensados para el punto flaco: **poder justificar el desempate**. Para cada uno decí (en una frase) **qué patrón** y **por qué no el parecido**. Después bosquejá el UML. Las soluciones están al final del todo.

## Bloque A — Factory Method: quién es el creador y quién el producto

> El patrón ya lo reconocés (lo acertaste en los ej. 4 y 7). Lo que hay que aceitar es el **modelado**, que es donde se te cruzó. Para cada caso: **1)** confirmá que es Factory Method; **2)** decí qué clase es el **CREADOR** (la que lleva el **método fijo** + el **método fábrica abstracto**) y qué es el **PRODUCTO** (interfaz + clases concretas); **3)** bosquejá el UML con los **dos árboles** (creadores y productos) y la dependencia «create».

**A1.** Un lector de archivos abre documentos. El procedimiento de abrir es **siempre el mismo** —validar permisos, cargar los bytes y registrar la apertura en el historial—, pero según la extensión hay que crear un `DocumentoTexto`, un `DocumentoPlanilla` o un `DocumentoPDF`.

**A2.** Una plataforma de envíos genera la etiqueta de despacho según el correo elegido: OCA, Andreani o Correo Argentino, cada uno con su propio formato de etiqueta. El procedimiento de despachar es siempre igual: pesar el paquete, generar la etiqueta y registrar el envío.

**A3.** *(El ej. 7, ahora para modelar bien.)* Un juego lanza la oleada de enemigos según el nivel. El procedimiento de la oleada es idéntico —elegir la posición de aparición, hacerlos aparecer y activarles la IA—, pero en el bosque salen lobos, en la cueva murciélagos y en el castillo caballeros. Modelalo poniendo cada clase en su rol: quiero ver dónde va `lanzarOleada()` y quién es el producto.

## Bloque B — Decorator vs Adapter (los dos envuelven)

**B1.** Tenés una clase `Reproductor` con `reproducir()` que ya anda. Según el caso, la reproducción puede ir con subtítulos, con ecualizador, con control de volumen normalizado, o varias de esas a la vez, y quien reproduce decide la combinación en el momento.

**B2.** Integrás una librería de mapas cuyo objeto expone `renderMap(lat, lng, zoomLevel)`, pero tu sistema en todos lados espera hablarle a una interfaz propia `Mapa.mostrar(coordenadas)`. No podés tocar la librería y el resto del código tiene que seguir igual.

---

# Soluciones de los ejercicios de refuerzo

> No mires hasta haberlos hecho.

**A1 → Factory Method.**
- **Creador:** `LectorDeArchivos` (abstracto) con el **método fijo** `abrir()` —valida permisos → carga bytes → registra en el historial— que adentro llama al **método fábrica abstracto** `crearDocumento(): Documento`. `abrir()` **no se redefine**.
- **Producto:** interfaz `Documento`, con `DocumentoTexto`, `DocumentoPlanilla`, `DocumentoPDF`.
- **Creadores concretos:** `LectorTexto`, `LectorPlanilla`, `LectorPDF`, cada uno redefine solo `crearDocumento()`.

**A2 → Factory Method.**
- **Creador:** `PlataformaDeEnvios` (abstracto) con el **método fijo** `despachar()` —pesar → generar etiqueta → registrar— que llama a `crearEtiqueta(): Etiqueta` abstracto.
- **Producto:** interfaz `Etiqueta`, con `EtiquetaOCA`, `EtiquetaAndreani`, `EtiquetaCorreoArg`.
- **Creadores concretos:** `EnvioOCA`, `EnvioAndreani`, `EnvioCorreoArg`.

**A3 → Factory Method** *(el ej. 7 bien modelado)*.
- **Creador:** `Nivel` (abstracto) con el **método fijo** `lanzarOleada()` —posición → aparecer → activar IA— que llama a `crearEnemigo(): Enemigo` abstracto.
- **Producto:** interfaz `Enemigo`, con `Lobo`, `Murcielago`, `Caballero`.
- **Creadores concretos:** `NivelBosque`, `NivelCueva`, `NivelCastillo`, cada uno devuelve su enemigo.
- **Lo que corregís del ej. 7:** el producto es el **enemigo** (no el nivel), y `lanzarOleada()` vive en el **creador** (no pegado al producto).

> El truco de todo el Bloque A: **el producto es lo que la fábrica FABRICA** (el documento, la etiqueta, el enemigo), no la clase que lo usa. Y el **método fijo** (`abrir` / `despachar` / `lanzarOleada`) va en el **CREADOR** y llama al método fábrica abstracto. Si ubicás bien esas dos cosas, el UML sale solo.

**B1 → Decorator.** *Estructurar.* Funcionalidades **opcionales y combinables** (subtítulos + ecualizador + volumen), elegidas **en runtime**, sobre una clase que **no se toca** y **manteniendo** `reproducir()`. No es Adapter porque no traducís ninguna interfaz incompatible: la interfaz se **conserva** y se le **suma**.

**B2 → Adapter.** *Estructurar.* Dos interfaces que **no encajan** (`renderMap(lat, lng, zoom)` vs `mostrar(coordenadas)`) y una librería que **no controlás**. `class MapaAdapter implements Mapa` compone la librería y traduce `mostrar()` → `renderMap()`. No es Decorator porque no sumás comportamiento: **cambiás** la interfaz para que encaje.

> El truco del Bloque B: **¿la interfaz cambia o se mantiene?** Cambia (traduce) → Adapter. Se mantiene (suma) → Decorator.
