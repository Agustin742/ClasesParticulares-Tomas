# Devolución — Ejercicios de la Clase 4 (patrones)

## Titular: 6 de 6, y encontramos tu patrón de error

De vuelta: **acertaste el patrón en los seis**, incluidos los dos "colados" de la Clase 3 (el Adapter del ej. 5 y el Decorator del ej. 6). Llevás **13 de 13** entre las dos entregas. **Elegís bien**.

Pero ahora se ve algo más útil que el puntaje, porque tenemos dos entregas para comparar. Fijate:

- **Clase 3, ej. 7 (Factory Method):** cruzaste **creador** y **producto** — le pegaste `lanzarOleada()` al producto.
- **Clase 4, ej. 3 (Builder):** cruzaste **builder** y **producto** — hiciste que `Formulario` dependa del builder, cuando es al revés.

Es **el mismo error dos veces**: el patrón lo reconocés, pero cuando hay **dos árboles** (el que fabrica y lo fabricado) se te da vuelta **quién usa a quién**. Ese es *el* tema de esta devolución, y es uno solo. Todo lo demás son detalles.

La otra: **la justificación se te achicó**. En la Clase 3 escribías una frase por ejercicio; acá tenés dos justificaciones muy buenas (Abstract Factory y Composite).

---

## Ejercicio 1 — Logger único → **Singleton** ✅

Lo que dibujaste: `Logger` con `- instancia: Logger`, `- lineas: number`, **constructor privado** `- Logger()`, `+ getInstancia(): Logger`, `+ getLineas(): number`, y la flecha de la clase **a sí misma**. La estructura está: constructor privado + instancia guardada + acceso por método estático es exactamente el patrón, y la autoasociación la dibujaste bien.

**A pulir (importante):**
- **`instancia` y `getInstancia()` tienen que ser `static`.** Es *la* característica del patrón y en el diagrama no está: si `instancia` es de instancia, cada objeto tiene la suya y no hay unicidad — y si `getInstancia()` no es estático, necesitás un `Logger` para pedir el `Logger`. En UML lo estático va **subrayado**. Subrayá las dos.
- **Al logger le falta loguear.** Tenés `getLineas()` pero no el método que escribe (`escribir(linea: string): void`). El enunciado arranca con "escribe en un archivo": el diagrama tiene toda la maquinaria de la instancia única y ninguna de las responsabilidades reales.
- **La justificación se queda en la mitad.** Dijiste "nos pide tener un solo logger" ✅, pero el enunciado también pide *"que cualquier módulo pueda conseguirlo sin recibirlo por parámetro"*. Son **las dos mitades** de Singleton: instancia única **+ punto de acceso global**. Nombrá las dos.

> **Y esto en el parcial suma:** aclará que Singleton **viola SRP** (la clase guarda logs *y* administra su propia instancia) y **DIP** (quien lo usa no declara que depende de él), y que se justifica acá **solo** porque el enunciado exige el acceso global. La cátedra tiene el cuadro de ventajas/desventajas explícito en el PDF.

---

## Ejercicio 2 — Retail / Mayorista → **Abstract Factory** ✅ (el más completo de los 6)

Lo que dibujaste: `<<interface>> FabricaApp` con `crearPrecio() / crearCarrito() / crearMail()`; `Retail` y `Mayorista` como fábricas concretas; y **tres familias de productos** (`CalculoPrecio`, `GeneradorMail`, `PoliticaCarrito`), cada una con su variante `Retail*` y `Mayorista*`, más las dependencias «create» de cada fábrica a sus productos. **Los dos árboles completos, los tres productos, las seis variantes y las flechas de creación.** Es un montón de diagrama y está bien armado.

**Y la justificación es la mejor de la entrega:** *"pide que los dos clientes se formen con sus respectivas especificaciones sin mezclarse"*. Eso **es** el argumento de Abstract Factory — la coherencia de la familia, que es justo lo que el enunciado exige con *"nunca un mail informal con precios sin IVA"*. Así se justifica un patrón: no repetiste el nombre, dijiste **qué propiedad del problema** lo pide.

**A pulir:**
- **Los atributos están en la clase equivocada.** Pusiste `muestraIva: boolean` en `CalculoPrecio`, `tieneTope: boolean` en `PoliticaCarrito`, `tipoMail: string` en `GeneradorMail` — o sea, en los **padres abstractos**. Pero el padre no debería saber que existe la variación: si `RetailCalculoPrecio` **es** el que muestra IVA, eso no es un `boolean` en el padre, es **lo que hace el hijo**. Un `boolean muestraIva` en el padre es un `if` esperando a nacer, y el patrón existe justamente para no tener ese `if`. Los padres se quedan **solo con la operación** (`generarPrecio(): Precio`) y cada hijo la implementa a su manera.
- Detalle de nombres: `crearPrecio(): CalculoPrecio` no crea un precio, crea **el calculador**. `crearCalculoPrecio()` (o `crearCalculador()`) se lee mejor.

---

## Ejercicio 3 — Formulario de la inmobiliaria → **Builder** ✅ (patrón correcto, modelado al revés)

**Este es el importante.** El patrón está bien elegido y bien justificado ("hay que crear un formulario con cosas obligatorias y opcionales" ✅ — esa es la señal). Pero el UML tiene **la dependencia dada vuelta** y los métodos no pueden funcionar.

Lo que dibujaste: `Formulario` con los 10 atributos, y `Formulario.mostrarFormulario(): BuilderFormulario` con una flecha de dependencia **hacia** `<<interface>> BuilderFormulario`, que tiene `agregarDireccion(): string`, `agregarPrecio(): number`, … y `construir(): Formulario`.

**A pulir (esto es el nudo):**

1. **La flecha va al revés.** `Formulario` es **el producto**: es lo que se fabrica, y **no tiene por qué saber que existe un builder**. Hoy tu producto depende de su fábrica. La única flecha entre los dos es la de `construir(): Formulario` — **del builder al producto**, no al revés. Sacá `mostrarFormulario()` de `Formulario`: quien usa el builder es **el cliente** (el código que arma el alta), no el formulario.

2. **Los métodos del builder devuelven el tipo equivocado y no reciben nada.** Tenés `agregarDireccion(): string`. Dos problemas juntos: el dato **entra por parámetro** y lo que se **devuelve es el propio builder**, para poder encadenar. Va así:

   ```typescript
   agregarDireccion(direccion: string): BuilderFormulario
   agregarPrecio(precio: number): BuilderFormulario
   indicarAmbientes(cantidad: number): BuilderFormulario
   aceptaMascotas(): BuilderFormulario
   construir(): Formulario   // el único que NO devuelve el builder
   ```

   Tal como está dibujado, **no hay forma de que un dato entre al builder** (ningún método recibe parámetros) y **no se puede encadenar** (cada uno devuelve un tipo distinto). El encadenado es el 90% de por qué elegís Builder:

   ```typescript
   new FormularioBuilder()
     .agregarDireccion("Av. Siempreviva 742")
     .agregarPrecio(150000)
     .indicarAmbientes(3)
     .aceptaMascotas()
     .construir();
   ```

   Se lee, y **lo que no aplica no se escribe** — que era exactamente el problema del constructor de 10 parámetros.

3. **Falta el builder concreto.** Solo tenés la interfaz. Hace falta `FormularioBuilder implements BuilderFormulario`, que es quien **guarda el estado parcial** mientras lo vas armando y lo entrega en `construir()`.

4. Dos detalles de prolijidad: `precio: number` está **dos veces** (en atributos y otra vez abajo), y `direccion: number` debería ser `string`.

> **Este es el mismo cruce del ej. 7 de la Clase 3.** Allá `lanzarOleada()` terminó pegado al producto en vez del creador; acá `mostrarFormulario()` quedó pegado al producto en vez del cliente. **Regla para los dos:** el **producto es tonto** — no conoce a quien lo fabrica. Las flechas van **siempre** del que fabrica hacia lo fabricado. Si tu producto tiene un método que devuelve la fábrica, algo se dio vuelta.

---

## Ejercicio 4 — Empleados y equipos → **Composite** ✅

Lo que dibujaste: `<<interface>> GestionPersonal` con `calcularSalario(): number`; `Persona` (nombre, dni, salario) y `Equipo` (numeroGrupo) implementándola; y **la agregación de `Equipo` hacia `GestionPersonal`**. Esa última relación es **la firma del patrón** y la pusiste: `Equipo` contiene cosas del tipo de la interfaz, no `Persona[]` — por eso un equipo puede tener **otros equipos** adentro y la recursión sale sola, sin un `if`. Bien.

Justificación correcta también: *"necesitamos tener grupos y personas dentro de otros"* — eso es el árbol.

**A pulir:**
- **Falta la multiplicidad.** En la punta de la agregación va `*` (o `0..*`). Sin eso, el diagrama dice que un equipo tiene **un** miembro, y todo el patrón depende de que tenga **muchos**. Es un caracter y cambia el significado.
- **Falta el atributo y los métodos de gestión de hijos.** `- miembros: GestionPersonal[]` en `Equipo`, más `agregar(m: GestionPersonal)` y `quitar(m: GestionPersonal)`. Un árbol que no se puede armar no es un árbol. Ojo que van **solo en `Equipo`** (la rama), no en la interfaz: `Persona` es una hoja y no tiene hijos que agregar.
- **El nombre de la interfaz.** `GestionPersonal` nombra un *módulo*, no una *cosa*. El componente de un Composite tiene que nombrar **lo que la hoja y la rama son o saben hacer** por igual: `Costeable`, `ItemDeNomina`, `Empleado`. Probá leerlo en voz alta: "una `Persona` es un `Costeable`" ✅ / "una `Persona` es una `GestionPersonal`" ❌. Si no se lee, el nombre está mal.
- `cantidadParticipantes()` en `Equipo` está bien, pero pensá si no querés que sea **recursiva** también (contar los de los subequipos). Si la respuesta es sí, es candidata a subir a la interfaz.

> **Desempate para tener listo:** es **Composite y no Decorator** aunque los dos se compongan de su propia interfaz. `Equipo` contiene **muchos** hijos y **no les agrega nada** — los **agrupa**. Un Decorator envuelve **uno** solo y le **suma** comportamiento. Composite es un **árbol**; Decorator, una **pila**.

---

## Ejercicio 5 — Servicio SOAP del cliente → **Adapter** ✅

Lo que dibujaste: `<<interface>> FuenteDatos` con `obtener(): Registro[]`; `FormatoAdapter implements FuenteDatos` con `- adaptador: NuevoFormato` y `obtener(): Registro[]`; y la agregación a `NuevoFormato`, que expone `fetchAll()`. **Estructura correcta.**

**Buena noticia:** `obtener()` devuelve `Registro[]`, no `void`. **Ese era el vicio que te marqué en la Clase 3 y lo corregiste.** 👏

**A pulir:**
- **`fetchAll(): void` — el vicio sobrevivió acá.** El servicio SOAP **devuelve XML**; si devuelve `void`, el adapter no tiene nada que traducir y `obtener()` no puede devolver nada. Va `fetchAll(): string` (o `XmlDoc`). Y fijate que ahí se ve el patrón entero en dos tipos: **entra** `fetchAll(): string` (XML, nombres ajenos) → **sale** `obtener(): Registro[]` (tu formato). Esa diferencia de tipos **es** la traducción.
- **`- adaptador: NuevoFormato` está mal nombrado.** `NuevoFormato` no es el adaptador: es el **adaptado** (*adaptee*), el que no encaja. El **adaptador** es `FormatoAdapter`, la clase donde está ese atributo. Llamalo `- adaptado: NuevoFormato` o `- servicio: NuevoFormato`. Suena a detalle pero es el mismo cruce de roles del ej. 3: el nombre delata quién pensás que hace qué.
- **La justificación es circular.** *"Adapter ya que debo adaptar un nuevo cliente a lo que ya tengo"* = "es Adapter porque adapto". Eso en el parcial no suma: repite el nombre en vez de señalar el **hecho del enunciado** que lo pide. Serviría: *"dos interfaces incompatibles (`fetchAll()`/XML vs `obtener()`/`Registro[]`) y el servicio del cliente no lo puedo modificar → traduzco. No es Decorator porque no le sumo comportamiento: le **cambio** la interfaz."*

---

## Ejercicio 6 — Exportar nota cifrada/comprimida/con marca de agua → **Decorator** ✅ (impecable de nuevo)

Lo que dibujaste: `<<interface>> Exportable` con `exportar(): string`; `Nota` implementándola; `DecoratorExportacion` **abstracto** que **implementa** `Exportable` *y* **contiene** un `# nota: Exportable`; y `NotaCifrada / NotaComprimida / NotaConMarcaAgua` heredando de él. **Perfecto, otra vez.** La doble relación del decorador (ES un `Exportable` y a la vez TIENE un `Exportable`) es lo que hace que se apilen —`new NotaCifrada(new NotaComprimida(new Nota()))`— y la clavaste sin dudar. Es el segundo Decorator perfecto en dos entregas: **este patrón ya lo tenés**.

**A pulir:**
- **La justificación es la palabra "decorator". Nada más.** Es el único de los seis sin una sola razón. Y es una lástima, porque es el que mejor entendés: sabés *por qué* es Decorator, solo que no lo escribiste. **En un parcial, eso es medio punto regalado.** La frase que faltaba: *"comportamientos opcionales y combinables (2³ = 8 combinaciones) elegidos en runtime, sobre una clase que no quiero tocar. No es Strategy porque Strategy tiene **un** algoritmo activo por vez y lo **reemplaza**; acá los tres se apilan **a la vez**. Strategy intercambia; Decorator acumula."*
- Detalle chico: hiciste que `Nota` implemente `Exportable`, pero el enunciado dice *"`Nota` ya existe y funciona; no la querés tocar"* — y para que implemente la interfaz, la tocás. Es una simplificación defendible y no te la van a bajar. La alternativa purista es un `ExportadorBase implements Exportable` que **envuelve** la `Nota` y la deja intacta. Mencionalo y quedás impecable.

---

## Para llevarte

1. **13 de 13 entre las dos entregas.** No elegís mal. Dejá de dudar del reconocimiento: tu problema no está ahí.
2. **El error real, y es uno solo: se te cruza quién fabrica y qué se fabrica.** Pasó con Factory Method (Clase 3, ej. 7) y con Builder (Clase 4, ej. 3). **El producto es tonto: no conoce a quien lo fabrica.** Antes de entregar cualquier patrón creacional, hacé una sola pregunta: *"¿alguna flecha sale del producto hacia la fábrica?"* Si sale, está al revés. Los ejercicios del Bloque A son todos sobre esto.
3. **La justificación se te achicó, y es lo único que te separa de la nota completa.** Tenés la prueba de que sabés hacerlo: las de **Abstract Factory** ("sin mezclarse") y **Composite** ("grupos dentro de otros") son excelentes — señalan el hecho del enunciado, no repiten el nombre. Las de Adapter ("porque adapto") y Decorator ("decorator") no dicen nada. **Escribí las seis como escribiste esas dos.** Fórmula: *"Es X porque el enunciado dice «[cita]», que es la señal de X. No es Y porque [el desempate]."*
4. **Vicios técnicos concretos** (rápidos de arreglar, caen siempre): `static` subrayado en Singleton; los métodos del Builder reciben **parámetro** y devuelven **el builder**; multiplicidad `*` en la agregación del Composite; y **`void` donde tiene que volver algo** — lo corregiste en `obtener()` pero se te escapó en `fetchAll()`.

---

# Ejercicios de refuerzo

Para cada uno: **1)** qué patrón y **por qué no el parecido** (una frase, con la fórmula del punto 3); **2)** el UML. Soluciones al final del todo.

## Bloque A — Quién fabrica y qué se fabrica

> Tu punto flaco, en los tres sabores. En cada uno quiero ver explícito: **qué clase es la que FABRICA**, **qué clase es EL PRODUCTO**, y **hacia dónde apuntan las flechas entre las dos**.

**A1.** *(El ej. 3, ahora bien modelado.)* Rehacé el Builder del formulario inmobiliario. Quiero ver: la interfaz `BuilderFormulario` con los métodos **recibiendo parámetro y devolviendo el builder**, el **builder concreto** que guarda el estado parcial, `construir(): Formulario`, y `Formulario` **sin una sola flecha saliendo hacia el builder**. Escribí también la llamada encadenada del cliente para una propiedad con dirección, precio, 3 ambientes y mascotas.

**A2.** Una app de facturación arma la factura de un pedido. Emitir es **siempre igual** —validar el pedido, numerar el comprobante y registrarlo en el libro IVA—, pero según la condición fiscal hay que crear una `FacturaA`, una `FacturaB` o una `FacturaC`. ¿Quién es el creador, quién el producto, y dónde vive `emitir()`?

**A3.** Un editor exporta el documento a PDF. El PDF tiene: título y contenido (obligatorios), y opcionales encabezado, pie de página, marca de agua, numeración, índice y metadatos de autor. ¿Builder o Abstract Factory? Justificá el desempate **antes** de dibujar.

## Bloque B — Composite vs Decorator (los dos se componen de su propia interfaz)

**B1.** Un sistema de archivos tiene archivos y carpetas. Una carpeta contiene archivos **y otras carpetas**, sin límite. Hay que poder pedir el **tamaño en bytes** de cualquier cosa: un archivo suelto, una carpeta, o el disco entero.

**B2.** Tenés una clase `Peticion` con `enviar()` que ya funciona. Según el caso, el envío puede ir con reintentos, con log de auditoría, con caché, o varias de esas a la vez, y quien la usa arma la combinación en el momento.

## Bloque C — La frase que te falta

Sin dibujar nada. Para **cada uno de los 6 ejercicios que entregaste**, escribí **una sola oración** con la fórmula: *"Es X porque el enunciado dice «[cita textual]», que es la señal de X. No es Y porque [desempate]."* Diez minutos. Es el ejercicio más corto y el que más te va a rendir en el parcial.

---

# Soluciones de los ejercicios de refuerzo

> No mires hasta haberlos hecho.

**A1 → Builder** *(el ej. 3 bien modelado)*.
- **Producto:** `Formulario`, con sus atributos. **No tiene ninguna flecha hacia el builder.** No sabe que el builder existe.
- **Interfaz builder:** `BuilderFormulario` con `agregarDireccion(dir: string): BuilderFormulario`, `agregarPrecio(p: number): BuilderFormulario`, `indicarAmbientes(n: number): BuilderFormulario`, `aceptaMascotas(): BuilderFormulario`, … y `construir(): Formulario`.
- **Builder concreto:** `FormularioBuilder implements BuilderFormulario`, guarda el estado parcial y arma el `Formulario` en `construir()`.
- **Flechas:** realización `FormularioBuilder ---▷ BuilderFormulario`; dependencia `FormularioBuilder ---> Formulario` (por `construir()`). **Ninguna al revés.**
- **Cliente:** `new FormularioBuilder().agregarDireccion("Av. Siempreviva 742").agregarPrecio(150000).indicarAmbientes(3).aceptaMascotas().construir()`
- **Lo que corregís:** los métodos **reciben** el dato y **devuelven el builder**; `mostrarFormulario()` **no va** en `Formulario`.

**A2 → Factory Method.**
- **Creador:** `Facturador` (abstracto), con el **método fijo** `emitir()` —validar → numerar → registrar en libro IVA— que adentro llama al **método fábrica abstracto** `crearFactura(): Factura`. `emitir()` **no se redefine**.
- **Producto:** interfaz `Factura`, con `FacturaA`, `FacturaB`, `FacturaC`.
- **Creadores concretos:** `FacturadorA`, `FacturadorB`, `FacturadorC`, cada uno redefine **solo** `crearFactura()`.
- **`emitir()` vive en el CREADOR**, nunca en la factura. *No es Abstract Factory:* varía **un solo** producto (la factura), no una familia.

**A3 → Builder.** **Un solo** producto (el PDF) con **muchas partes opcionales** (2 obligatorias + 6 opcionales) que se arma **paso a paso**. *No es Abstract Factory:* ahí habría **varios** productos que tienen que ser **coherentes entre sí** y no mezclarse; acá hay uno solo y el problema es el **constructor telescópico**, no la coherencia de una familia.
> El desempate de Builder vs Abstract Factory en una línea: **¿cuántos productos hay?** Uno con partes opcionales → Builder. Varios que no se pueden mezclar → Abstract Factory.

**B1 → Composite.** *Estructurar.* Jerarquía **todo-parte**, **profundidad arbitraria**, y la misma operación —`tamanio(): number`— sobre la hoja y sobre la rama. `interface ItemDeDisco { tamanio(): number }`; `Archivo` (hoja) devuelve sus bytes; `Carpeta` (rama) tiene `- items: ItemDeDisco[]` **con multiplicidad `*`** y **suma los de sus hijos** → recursión sin un solo `if`. *No es Decorator:* `Carpeta` contiene **muchos** hijos y no les **agrega** nada; los agrupa.

**B2 → Decorator.** *Estructurar.* Comportamientos **opcionales y combinables** (reintentos + log + caché) elegidos **en runtime**, sobre una clase que **no se toca**, **manteniendo** `enviar()`. `interface Enviable`, `abstract class DecoradorPeticion implements Enviable` que **contiene** un `Enviable`; `ConReintentos`, `ConAuditoria`, `ConCache` se apilan. *No es Composite:* envuelve **uno** solo y le **suma**; no arma ningún árbol.

> El truco del Bloque B: **¿cuántos contiene y para qué?** Muchos, para tratarlos igual → **Composite** (árbol). Uno, para sumarle algo → **Decorator** (pila).
