# Clase 5 — Los 4 que faltan + cierre de la unidad

> **Unidad:** 4 (Patrones de diseño) · **Lenguaje de ejemplos:** TypeScript

---

## Clase 5 — Los 4 que faltan + cierre de la unidad

**Programación II · Unidad 4**

> *"En el parcial no te van a pedir que recites el patrón. Te van a dar un enunciado y vas a tener que reconocerlo, elegirlo y —sobre todo— saber por qué no es el que se le parece."*

Última clase de patrones. Cerramos el catálogo con los **4 que faltan** —**Facade, Chain of Responsibility, Template Method y State**— y después dejamos de sumar teoría para hacer lo único que se evalúa: **leer casos, reconocer el patrón y justificar**. El resto de la clase es práctica y **comparación entre los que se confunden**.

---

## Dónde estamos parados

La cátedra evalúa **13 patrones**. Ya viste **9**:

| Familia | Ya vistos | Hoy |
|---|---|---|
| **Creacionales (4)** | Singleton · Factory Method · Abstract Factory · Builder | — (completa) |
| **Estructurales (4)** | Adapter · Composite · Decorator | **Facade** |
| **Comportamiento (5)** | Strategy · Observer | **Chain of Responsibility · Template Method · State** |

Hoy van **4** y con eso **cierra el temario evaluable**: 13 de 13. El resto de la clase es **reconocer y desempatar**.

> Ojo: hoy los patrones nuevos son **casi todos de comportamiento** (3 de 4). Tiene sentido: comportamiento es la familia de *"¿quién hace qué y cuándo?"*, y ahí es donde más patrones se confunden entre sí.

---

## Arrancamos: los 9 en una línea

Antes de sumar, quiero escucharte los **9 que ya viste**. No la definición: **la señal delatora**, la frase de un enunciado que te hace pensar en cada uno. Decilos vos, después chequeamos.

**Creacionales:** Singleton · Factory Method · Abstract Factory · Builder
**Estructurales:** Adapter · Composite · Decorator
**Comportamiento:** Strategy · Observer

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Singleton** → *"una única instancia"* accesible desde todo el sistema · **Factory Method** → un `new`/`switch` que decide **qué clase concreta crear** (un producto) · **Abstract Factory** → una **familia** de productos que varían juntos y no se mezclan · **Builder** → objeto con **muchas partes opcionales**, armado paso a paso · **Adapter** → dos interfaces que **no encajan** y una no la controlás · **Composite** → **árbol** todo-parte, tratar igual a la hoja y a la rama · **Decorator** → **sumar comportamientos combinables** sin tocar la clase · **Strategy** → elegir **cómo** hacer algo entre varios algoritmos, cambiable en runtime · **Observer** → un cambio tiene que **avisarle a varios** que se suscriben y se van.

---

## El método, por última vez

Todo lo de hoy —y todo el parcial— se apoya en el mismo reflejo. Frente a un enunciado, **¿de qué se queja el problema?**

- *"No sé qué instanciar / el `new` está desparramado / quiero controlar la creación"* → **Creacional**.
- *"Tengo que hacer encajar / envolver / esconder / representar un todo-y-sus-partes"* → **Estructural**.
- *"Tengo que elegir un comportamiento / avisar / coordinar quién hace qué / según en qué situación esté"* → **Comportamiento**.

> Y las dos herramientas de siempre: **programar contra una interfaz** y **composición**. Si te perdés, buscá esas dos y el patrón aparece. La familia te dice **por dónde buscar**; el desempate te dice **cuál es**.

---

## Estructurales — Facade

**Familia:** estructural.

**Intención:** ofrecer una **interfaz simple y de alto nivel** a un subsistema complejo, **escondiendo** sus partes detrás de una sola puerta de entrada.

Es, conceptualmente, **el patrón más simple de los 13**. No agrega funcionalidad, no traduce nada: solo **simplifica el acceso**.

> Analogía: el **mozo** del restaurante. Vos pedís "milanesa con papas". No hablás con el parrillero, con el que pela las papas, con el de la caja ni con el que lava. El mozo (fachada) **articula** con todos ellos. La cocina sigue existiendo y siendo compleja: vos simplemente no la tocás.

---

## Facade — señal delatora

Sospechá Facade cuando…

- El cliente tiene que **orquestar muchas clases en un orden específico** para hacer algo simple, y **ese código de orquestación se repite**.
- Querés **desacoplar** tu código de una biblioteca o subsistema grande del que usás **el 10%**.
- El enunciado dice: *"esconder la complejidad"*, *"un punto de entrada único"*, *"que el cliente no tenga que conocer los detalles internos"*, *"simplificar el uso"*.

---

## Facade — el código

```typescript
// --- El subsistema: varias clases, cada una con su propia complejidad
class Codec           { cargar(archivo: string)             { return `bytes(${archivo})`; } }
class ExtractorAudio  { extraer(bytes: string)              { return `audio[${bytes}]`; } }
class Compresor       { comprimir(x: string, fmt: string)   { return `${x}→${fmt}`; } }
class MarcaDeAgua     { aplicar(x: string)                  { return `${x}+marca`; } }

// --- La FACHADA: una sola puerta, un solo método de alto nivel
class ConversorDeVideo {
  private codec     = new Codec();
  private audio     = new ExtractorAudio();
  private compresor = new Compresor();
  private marca     = new MarcaDeAgua();

  convertir(archivo: string, formato: string): string {
    const bytes      = this.codec.cargar(archivo);           // el orden de los pasos,
    const pista      = this.audio.extraer(bytes);            // que antes tenía que saber
    const comprimido = this.compresor.comprimir(pista, formato);  // el cliente,
    return this.marca.aplicar(comprimido);                   // ahora vive acá adentro
  }
}

// El cliente: una línea, cero conocimiento del subsistema
const resultado = new ConversorDeVideo().convertir("clase-04.avi", "mp4");
```

Sin la fachada, **cada cliente** que quiera convertir un video tiene que conocer las 4 clases, sus métodos y **el orden correcto**. Con la fachada, conoce **una**.

---

## Razoná — Facade vs Adapter (los dos "envuelven")

Adapter (Clase 3) y Facade **envuelven otra cosa y le ponen una interfaz por delante**. Se confunden. Desempatalos vos:

- En **Adapter**, ¿la interfaz que ofrecés la elegís vos o **te la impone** el código que ya existe y no querés tocar?
- En **Facade**, ¿existía **antes** una interfaz que tenías que respetar, o la **inventás** vos para que sea cómoda?
- **Adapter** envuelve típicamente **una** clase; **Facade** envuelve **muchas**. ¿Por qué esa diferencia es una consecuencia, y no la definición?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Adapter** existe porque hay una **incompatibilidad**: el cliente ya habla `Pago.pagar(monto)` y el SDK habla `createPayment({...})`. La interfaz destino **ya estaba dada**; tu trabajo es **traducir**.
>
> **Facade** existe porque hay **complejidad**: nadie te obliga a nada, **inventás** una interfaz cómoda que **no existía**. Tu trabajo es **simplificar**.
>
> En una línea: **Adapter traduce porque no encaja. Facade simplifica porque es engorroso.** (Y ojo con **Decorator**: ese ni traduce ni simplifica — **mantiene** la interfaz y **suma comportamiento**.)

---

## Facade — UML

> [IMAGEN: diagrama de clases UML del patrón Facade. A la izquierda, una caja "Client" con una única flecha de dependencia punteada que apunta a la caja del centro. En el centro, la clase "ConversorDeVideo" con el estereotipo «Facade» y un solo método público: + convertir(archivo, formato). A la derecha, un rectángulo grande de fondo gris claro con el título "SUBSISTEMA" que encierra cuatro cajas pequeñas: "Codec" con cargar(), "ExtractorAudio" con extraer(), "Compresor" con comprimir() y "MarcaDeAgua" con aplicar(). Desde "ConversorDeVideo" salen cuatro flechas de asociación, una hacia cada clase del subsistema. CLAVE VISUAL: el Client tiene UNA sola flecha (hacia la fachada) y ninguna flecha lo cruza hacia adentro del recuadro gris — debe verse de un vistazo que el cliente NO conoce el subsistema. Una nota junto al recuadro gris dice: "la fachada no le prohíbe a nadie usar el subsistema directo; solo ofrece el camino fácil".]

**Conexión SOLID:** DIP y bajo acoplamiento (el cliente depende de una sola clase, no de cuatro). Ojo con **SRP**: si la fachada empieza a hacer *todo*, se convierte en un "objeto dios". La fachada **orquesta**, no **implementa**.

---

## Checkpoint — los 4 estructurales

Con Facade se te completan los estructurales. Los cuatro **envuelven o reacomodan** objetos. La pregunta que los separa:

| Patrón | ¿Qué le hace a la interfaz? | ¿Para qué? |
|---|---|---|
| **Adapter** | La **cambia** (traduce) | Hacer encajar algo incompatible |
| **Decorator** | La **mantiene** | Sumar comportamiento apilable |
| **Facade** | Inventa una **nueva y más simple** | Esconder complejidad |
| **Composite** | La **comparte** entre hoja y rama | Tratar igual al todo y a la parte |

Decime vos, en voz alta: *"tengo una clase que envuelve a otra…"* ¿qué **pregunta** te hacés para saber cuál de los cuatro es?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> La pregunta: **¿qué problema tenía el cliente?** ¿No podía *hablarle* (Adapter)? ¿Quería *más* de lo mismo (Decorator)? ¿Se *ahogaba* en detalles (Facade)? ¿Tenía que recorrer un *árbol* (Composite)?

---

## Comportamiento — Chain of Responsibility

**Familia:** comportamiento.

**Intención:** pasar una petición **a lo largo de una cadena** de manejadores. Cada uno decide si **la procesa** o si **la pasa al siguiente**. El emisor no sabe **quién** va a atenderla.

> Analogía: un **trámite** en la facultad. Lo presentás en la ventanilla; el de la ventanilla ve si le corresponde a él, si no lo pasa al jefe de departamento, y si tampoco, al decano. Vos entregaste **un solo papel** y no sabés (ni te importa) quién terminó resolviéndolo.

---

## Predecí — el `if` anidado que crece

Un sistema de aprobación de compras, tal como se escribe la primera vez:

```typescript
interface Solicitud {
  monto: number;
  autenticado: boolean;
  bloqueado: boolean;
}

function aprobar(s: Solicitud): string {
  if (!s.autenticado) return "Rechazada: usuario no autenticado";
  else {
    if (s.monto <= 0) return "Rechazada: monto inválido";
    else {
      if (s.monto > 100000) return "Rechazada: supera el límite automático";
      else {
        if (s.bloqueado) return "Rechazada: usuario bloqueado";
        // ...y seguimos sumando niveles
      }
    }
  }
  return "Aprobada";
}
```

- La sucursal de Córdoba necesita **el mismo control pero sin el del límite**, y la de Rosario quiere **uno nuevo al principio**. ¿Qué hacés con esta función? ¿La copiás?
- ¿Y si te piden **cambiar el orden** de los controles?
- Fijate una cosa: **cada `if` hace lo mismo estructuralmente** — evalúa una condición y, si pasa, *le cede el turno al siguiente control*. ¿Y si cada control fuera **un objeto** que conoce **al que le sigue**?

---

## Chain of Responsibility — señal delatora

Sospechá CoR cuando…

- Una petición debe pasar por **una secuencia de pasos/controles/filtros**, y cualquiera puede **cortar** el flujo.
- El **orden** de esos pasos **cambia** o se **configura**, y se agregan y sacan pasos seguido.
- El emisor **no debe saber** quién va a atender la petición.
- Palabras que lo delatan: **cadena**, **escalar**, **derivar**, **niveles de aprobación**, **filtros sucesivos**, **middleware**, **si no puede, que lo pase al siguiente**.

---

## Chain of Responsibility — el código

```typescript
interface Solicitud { monto: number; autenticado: boolean; bloqueado: boolean; }

abstract class Handler {
  private siguiente: Handler | null = null;

  // devuelve el handler recibido → permite encadenar: a.encadenar(b).encadenar(c)
  encadenar(h: Handler): Handler {
    this.siguiente = h;
    return h;
  }

  // comportamiento por defecto: si no corté, le paso la solicitud al que sigue.
  // Si no hay siguiente, es que pasó todos los controles.
  manejar(s: Solicitud): string {
    return this.siguiente ? this.siguiente.manejar(s) : "Aprobada";
  }
}

class ValidarAutenticacion extends Handler {
  manejar(s: Solicitud): string {
    if (!s.autenticado) return "Rechazada: usuario no autenticado";   // ← CORTA la cadena
    return super.manejar(s);                                          // ← la SIGUE
  }
}
class ValidarMonto extends Handler {
  manejar(s: Solicitud): string {
    if (s.monto <= 0) return "Rechazada: monto inválido";
    return super.manejar(s);
  }
}
class ValidarLimite extends Handler {
  manejar(s: Solicitud): string {
    if (s.monto > 100000) return "Rechazada: supera el límite de aprobación automática";
    return super.manejar(s);
  }
}

// Armo la cadena. El ORDEN es un dato, no está cableado en el código:
const cadena = new ValidarAutenticacion();
cadena.encadenar(new ValidarMonto()).encadenar(new ValidarLimite());

console.log(cadena.manejar({ monto:   5000, autenticado: true,  bloqueado: false })); // Aprobada
console.log(cadena.manejar({ monto: 500000, autenticado: true,  bloqueado: false })); // Rechazada: límite
console.log(cadena.manejar({ monto:   5000, autenticado: false, bloqueado: false })); // Rechazada: no autenticado
```

La sucursal de Córdoba arma **otra cadena** con los mismos objetos en otro orden. **Cero código nuevo.**

---

## CoR — UML y la confusión con Decorator

> [IMAGEN: diagrama de clases UML del patrón Chain of Responsibility. Arriba, una clase abstracta "Handler" (nombre en itálica) con un atributo privado "- siguiente: Handler" y los métodos + encadenar(h) y + manejar(p). LA RELACIÓN CLAVE: desde "Handler" sale una flecha de agregación (rombo vacío) que vuelve a apuntar a la propia clase "Handler", etiquetada "siguiente 0..1" — es autorreferencial y es lo que forma la cadena; dibujarla bien visible. Debajo, tres clases concretas conectadas a Handler por herencia (línea con triángulo vacío): "ValidarAutenticacion", "ValidarMonto" y "ValidarLimite", cada una redefiniendo manejar(p). A la izquierda, una caja "Client" con una flecha de dependencia punteada hacia el PRIMER eslabón. ABAJO, aparte del diagrama de clases, una tira horizontal ilustrativa que muestre el recorrido de una solicitud: una cajita "Solicitud" entrando a "ValidarAutenticacion" → flecha a "ValidarMonto" → flecha a "ValidarLimite" → flecha a "Aprobada"; y desde ValidarMonto, una flecha ROJA que se desvía hacia abajo y termina en "Rechazada", con la etiqueta "cualquier eslabón puede CORTAR la cadena".]

> **La cátedra compara CoR con Decorator explícitamente** (y es la trampa favorita en los parciales). Los dos arman una **cadena de objetos que se contienen entre sí**. La diferencia es la **intención**:
>
> | | **Decorator** | **Chain of Responsibility** |
> |---|---|---|
> | Para qué | **Agregar** funcionalidad | **Pasamanos**: que alguien atienda |
> | ¿Se ejecutan todos? | **Sí**, todas las capas suman | **No**: uno puede **cortar** |
> | Resultado | El objeto envuelto, **potenciado** | La petición, **atendida por uno** |
>
> Decorator **acumula**; CoR **descarta**.

**Conexión SOLID:** SRP (cada control es una clase con **un** motivo de cambio) y OCP (control nuevo = clase nueva, sin tocar los otros).

---

## Comportamiento — Template Method

**Familia:** comportamiento.

**Intención:** definir el **esqueleto de un algoritmo** en una clase base, dejando que las **subclases redefinan algunos pasos** sin cambiar la estructura general.

La cátedra lo define perfecto: *"un proceso siempre requiere que se ejecuten los mismos pasos, pero admitiendo algunas variaciones en la implementación, todas ellas delegables a subclases"*.

> Analogía: una **receta base de torta**. Batir, mezclar, hornear 40 minutos, desmoldar. Los pasos y **el orden** no cambian nunca. Lo que cambia es **con qué** los rellenás: chocolate, vainilla, limón. Nadie hornea antes de mezclar.

---

## Razoná — ¿qué se repite y qué varía?

Importar alumnos desde CSV y desde JSON. Los dos importadores hacen esto:

```
1. leer el archivo
2. parsear el contenido
3. validar que haya datos
4. guardar en la base
5. devolver "importados N registros"
```

- ¿Cuáles de esos 5 pasos son **idénticos** en CSV y en JSON, y cuáles **cambian**?
- Si escribís las dos clases por separado, ¿cuánto código **duplicás**? ¿Qué pasa el día que cambia la forma de guardar en la base?
- ¿Y si el **orden** de los pasos es lo que hay que **proteger** — que nadie guarde sin validar? ¿Dónde ponés el orden para que una subclase **no pueda** romperlo?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> Ahí está el patrón: el **orden vive en la clase base**, en un método que **no se redefine** (el *template method*). Solo los **agujeros** (leer, parsear) quedan abstractos para que la subclase los rellene.

---

## Template Method — señal delatora

Sospechá Template Method cuando…

- Hay un **procedimiento fijo** con **pasos que varían** según el caso: importar datos, generar un reporte, procesar un pago, correr un test.
- Varias clases hacen **casi lo mismo**, y la diferencia está en **2 o 3 pasos** en el medio.
- El enunciado dice: *"el procedimiento es siempre el mismo"*, *"los mismos pasos"*, *"la estructura no cambia, cambia la implementación de algunos pasos"*.

---

## Template Method — el código

```typescript
abstract class ImportadorDeDatos {

  // EL TEMPLATE METHOD: acá vive el ORDEN. No es abstracto y NO se redefine.
  importar(ruta: string): string {
    const crudo = this.leer(ruta);              // paso VARIABLE (abstracto)
    const datos = this.parsear(crudo);          // paso VARIABLE (abstracto)

    if (!this.validar(datos)) return "Datos inválidos";   // paso con DEFAULT (hook)

    this.persistir(datos);                      // paso FIJO (igual para todos)
    return `Importados ${datos.length} registros`;
  }

  // los agujeros que la subclase DEBE rellenar
  protected abstract leer(ruta: string): string;
  protected abstract parsear(crudo: string): string[];

  // "hook": tiene comportamiento por defecto, la subclase PUEDE cambiarlo
  protected validar(datos: string[]): boolean { return datos.length > 0; }

  // paso común: se escribe UNA sola vez, acá
  private persistir(datos: string[]): void {
    console.log(`Guardando ${datos.length} filas en la base...`);
  }
}

class ImportadorCSV extends ImportadorDeDatos {
  protected leer(ruta: string): string      { return "nombre,edad\nTomas,22\nAna,25"; }
  protected parsear(crudo: string): string[] { return crudo.split("\n").slice(1); }  // salteo el encabezado
}

class ImportadorJSON extends ImportadorDeDatos {
  protected leer(ruta: string): string       { return '[{"nombre":"Tomas"},{"nombre":"Ana"}]'; }
  protected parsear(crudo: string): string[] { return JSON.parse(crudo).map((o: any) => o.nombre); }
  protected validar(datos: string[]): boolean { return datos.length > 1; }  // este hook lo pisa
}

console.log(new ImportadorCSV().importar("alumnos.csv"));   // Importados 2 registros
console.log(new ImportadorJSON().importar("alumnos.json")); // Importados 2 registros
```

`importar()` es **la única puerta**. Ninguna subclase puede persistir sin validar, ni parsear antes de leer: **el orden está blindado en la clase base**.

---

## Template Method — UML y la confusión con Strategy

> [IMAGEN: diagrama de clases UML del patrón Template Method. Arriba, una clase abstracta "ImportadorDeDatos" (nombre en itálica) con estos métodos, y es importante distinguirlos visualmente: "+ importar(ruta)" en NEGRITA y con una nota que dice "TEMPLATE METHOD: define el orden, no se redefine"; "# leer(ruta)" en itálica (abstracto); "# parsear(crudo)" en itálica (abstracto); "# validar(datos)" en redonda normal, con la etiqueta "hook: tiene default"; y "- persistir(datos)" en redonda, con la etiqueta "privado: igual para todos". Debajo, dos clases "ImportadorCSV" e "ImportadorJSON" conectadas por herencia (línea con triángulo vacío hacia la clase abstracta); ImportadorCSV redefine leer() y parsear(); ImportadorJSON redefine leer(), parsear() y validar(). Al costado del método importar(), un recuadro tipo pseudocódigo enumera sus líneas en orden: "1. leer() 2. parsear() 3. validar() 4. persistir()", con flechitas que salen de los pasos 1, 2 y 3 hacia abajo indicando "esto lo completa la subclase". Nota final: "la herencia es la herramienta: los pasos se redefinen, el esqueleto NO".]

> **Template Method vs Strategy** — el otro par clásico. Los dos permiten "variar el comportamiento":
>
> | | **Template Method** | **Strategy** |
> |---|---|---|
> | Herramienta | **Herencia** | **Composición** |
> | Qué varía | **Algunos pasos** de un algoritmo fijo | **El algoritmo entero** |
> | Cuándo se decide | En **compilación** (elegís la subclase) | En **runtime** (cambiás el objeto) |
> | Quién manda | La **clase base** llama a la subclase | El **cliente** le pasa la estrategia al contexto |
>
> Regla corta: **si el esqueleto es fijo y varían los rellenos → Template Method. Si lo que se reemplaza es la cosa entera y en caliente → Strategy.**

**Conexión SOLID:** OCP (nuevo formato = subclase nueva) y DRY (los pasos comunes se escriben una vez). Cuidado con **LSP**: una subclase **no debe** redefinir el template method ni romper el contrato de los pasos.

---

## Comportamiento — State

**Familia:** comportamiento.

**Intención:** permitir que un objeto **cambie su comportamiento cuando cambia su estado interno**. Da la sensación de que el objeto **cambió de clase**.

La idea: cada **estado** es **una clase**. El objeto principal (el **contexto**) **delega** su comportamiento en el objeto-estado que tiene puesto en ese momento.

> Analogía: un **semáforo**. En rojo, "avanzar" no hace nada. En verde, "avanzar" te deja pasar. El semáforo es el mismo objeto, pero **responde distinto** según en qué estado está — y además **él mismo sabe** a qué estado le toca ir después.

---

## Pará y pensá — el `switch` de estados

Un pedido de e-commerce pasa por: **Nuevo → Pagado → Enviado**, y puede cancelarse. Así se escribe la primera vez:

```typescript
class Pedido {
  private estado = "nuevo";

  pagar() {
    switch (this.estado) {
      case "nuevo":   this.estado = "pagado"; break;
      case "pagado":  console.log("ya está pagado"); break;
      case "enviado": console.log("ya fue enviado"); break;
    }
  }
  enviar() {
    switch (this.estado) {   // el MISMO switch, otra vez
      case "nuevo":   console.log("no se puede enviar sin pagar"); break;
      case "pagado":  this.estado = "enviado"; break;
      case "enviado": console.log("ya fue enviado"); break;
    }
  }
  cancelar() { /* ... y otro switch idéntico */ }
}
```

- Entra un estado nuevo: **"en preparación"**. ¿Cuántos `switch` tenés que tocar? ¿Qué pasa si te olvidás de uno? ¿El compilador te avisa?
- ¿Dónde está escrita la regla *"de Pagado solo se puede pasar a Enviado"*? ¿Está en **un** lugar o **desparramada** por todos los `switch`?
- Última: si **cada estado fuera una clase**, ¿qué métodos tendría? ¿Y **quién** decidiría a qué estado pasar después?

---

## State — señal delatora

Sospechá State cuando…

- El objeto tiene un **ciclo de vida** con **etapas** y **lo que se puede hacer depende de la etapa**: pedido, ticket de soporte, reproductor (play/pausa/stop), documento (borrador/revisión/publicado), cajero automático.
- Ves **el mismo `switch (estado)` repetido** en varios métodos.
- Hay **transiciones con reglas**: *"de acá solo se puede pasar a…"*.
- Palabras que lo delatan: **estado**, **etapa**, **flujo**, **según en qué situación esté**, **no se puede X hasta que Y**, **máquina de estados**.

---

## State — el código

```typescript
// Cada estado sabe DOS cosas: qué se puede hacer, y a qué estado se pasa después.
interface EstadoPedido {
  nombre(): string;
  pagar(p: Pedido): void;
  enviar(p: Pedido): void;
}

// El CONTEXTO: no tiene ni un solo if. Delega todo en su estado actual.
class Pedido {
  private estado: EstadoPedido = new Nuevo();

  cambiarEstado(e: EstadoPedido): void { this.estado = e; }   // lo usan los estados

  pagar():  void { this.estado.pagar(this); }                 // delega
  enviar(): void { this.estado.enviar(this); }                // delega
  estadoActual(): string { return this.estado.nombre(); }
}

class Nuevo implements EstadoPedido {
  nombre() { return "Nuevo"; }
  pagar(p: Pedido)  { console.log("Pago recibido."); p.cambiarEstado(new Pagado()); }  // ← transición
  enviar(p: Pedido) { console.log("No se puede enviar un pedido sin pagar."); }
}

class Pagado implements EstadoPedido {
  nombre() { return "Pagado"; }
  pagar(p: Pedido)  { console.log("Este pedido ya está pagado."); }
  enviar(p: Pedido) { console.log("Pedido despachado."); p.cambiarEstado(new Enviado()); }
}

class Enviado implements EstadoPedido {
  nombre() { return "Enviado"; }
  pagar(p: Pedido)  { console.log("Ya está pagado."); }
  enviar(p: Pedido) { console.log("Ya fue enviado."); }
}

const p = new Pedido();
p.enviar();                      // No se puede enviar un pedido sin pagar.
p.pagar();                       // Pago recibido.      → ahora es Pagado
p.enviar();                      // Pedido despachado.  → ahora es Enviado
console.log(p.estadoActual());   // Enviado
```

Estado nuevo (`EnPreparacion`) = **una clase nueva**. `Pedido` **no se toca**. Y la regla *"de Pagado se pasa a Enviado"* está escrita **en un solo lugar**: adentro de `Pagado`.

---

## State — UML

> [IMAGEN: diagrama de clases UML del patrón State. A la izquierda, la clase "Pedido" (el CONTEXTO) con un atributo privado "- estado: EstadoPedido" y los métodos públicos + pagar(), + enviar(), + cambiarEstado(e) y + estadoActual(). De "Pedido" sale una flecha de composición (rombo relleno) hacia la derecha, apuntando a una interfaz «interface» "EstadoPedido", etiquetada "estado" — es la relación principal. La interfaz "EstadoPedido" declara nombre(), pagar(p: Pedido) y enviar(p: Pedido). Debajo de la interfaz, tres clases conectadas por realización (línea punteada con triángulo vacío): "Nuevo", "Pagado" y "Enviado". LO QUE HAY QUE DESTACAR: desde cada clase de estado sale una flecha de dependencia punteada que VUELVE hacia "Pedido", etiquetada "cambiarEstado(new ...)" — porque son los estados los que deciden la transición; esta flecha de vuelta es la firma visual de State y es lo que lo diferencia de Strategy. Al costado, un pequeño diagrama de transiciones aparte (tres círculos: Nuevo → Pagado → Enviado, con las flechas etiquetadas "pagar()" y "enviar()") para mostrar la máquina de estados. Nota: "el contexto no tiene NI UN if: solo delega".]

**Conexión SOLID:** SRP (cada estado, una clase), OCP (estado nuevo sin tocar el contexto) y adiós a los `switch` repetidos.

---

## State vs Strategy — el par que más se confunde

Mirá los dos UML. **Son iguales**: un contexto que compone una interfaz, y varias clases que la implementan. Y sin embargo son patrones distintos. Antes de que te lo diga, buscá vos las diferencias:

- ¿Quién decide **cuál** de las clases concretas se usa? En Strategy, ¿de dónde sale la estrategia? En State, ¿de dónde sale el estado siguiente?
- ¿`EnvioEstandar` **sabe** que existe `EnvioExpress`? ¿`Pagado` **sabe** que existe `Enviado`?
- ¿Un `Carrito` con envío express **prohíbe** alguna operación? ¿Un `Pedido` en estado `Nuevo` **prohíbe** alguna operación?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | | **Strategy** | **State** |
> |---|---|---|
> | Qué representa | **Cómo** hacer algo (algoritmos) | **En qué situación** estoy (etapas) |
> | Quién elige | **El cliente**, desde afuera | **El propio estado**, desde adentro |
> | ¿Se conocen entre sí? | **No.** `EnvioExpress` ignora que existe `EnvioEstandar` | **Sí.** `Pagado` crea un `Enviado` |
> | Transiciones | No hay: las estrategias son **intercambiables** | Sí: hay un **orden permitido** |
>
> La pregunta que los desempata: **¿hay reglas sobre a qué se puede pasar después?** Si sí → **State**. Si cualquiera vale en cualquier momento → **Strategy**.

---

## Los 13 completos

Con Facade, CoR, Template Method y State, **cerraste el catálogo**. Antes de la práctica, la foto entera:

| Familia | Patrones |
|---|---|
| **Creacionales (4)** | Singleton · Factory Method · Abstract Factory · Builder |
| **Estructurales (4)** | Adapter · Composite · Decorator · **Facade** |
| **Comportamiento (5)** | Strategy · Observer · **Chain of Responsibility** · **Template Method** · **State** |

> A partir de acá **no sumamos teoría**. Todo lo que queda es entrenar el ojo: reconocer, elegir y **desempatar**. Empezamos por lo que más puntos te da y más se confunde: los **pares**.

---

## Pares que se confunden — por qué esto es la clase

En un parcial, **acertar el nombre no alcanza**: casi siempre hay un patrón parecido que también "casi encaja", y el punto está en saber **por qué no es ese**. Vamos a recorrer los **5 pares (y tríos) que más caen**, cada uno con un mini-caso para que desempates vos.

La regla de oro para todos: **no compares las estructuras (los UML se parecen), compará las intenciones.** *¿Qué problema tenía el cliente?*

---

## Par 1 — Factory Method vs Abstract Factory vs Builder

Los tres **fabrican objetos** y evitan el `new` cableado. El trío creacional que más se mezcla.

| | **Factory Method** | **Abstract Factory** | **Builder** |
|---|---|---|---|
| Qué produce | **Un** producto, varias variantes | Una **familia** de productos coherentes | **Un** producto complejo, por partes |
| El problema | *"¿qué clase concreta instancio?"* | *"¿cómo garantizo que todo sea del mismo juego?"* | *"el constructor tiene 10 parámetros opcionales"* |
| Palabra clave | variante, tipo | familia, no se mezclan | paso a paso, opcional |

**Mini-caso.** *"El sistema exporta a PDF, a Word o a HTML según lo que elija el usuario; en los tres casos es un solo documento de salida."*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Factory Method.** **Un** producto (el documento exportado) con **variantes** (PDF/Word/HTML). No es Abstract Factory porque no hay una *familia* de productos que tengan que ser coherentes entre sí; no es Builder porque no lo armás *por partes opcionales*, elegís una variante entera.

---

## Par 2 — Adapter vs Decorator vs Facade

Los tres **envuelven** otra cosa y le ponen una interfaz por delante. El trío estructural clásico.

| | **Adapter** | **Decorator** | **Facade** |
|---|---|---|---|
| Qué le hace a la interfaz | La **cambia** (traduce) | La **mantiene** y suma | Inventa una **nueva y más simple** |
| Por qué existe | Dos interfaces **no encajan** | Querés **más** de lo mismo, combinable | El subsistema es **engorroso** |
| A cuántos envuelve | **Una** clase (la incompatible) | **Una**, apilable sobre otra igual | **Muchas** (todo el subsistema) |

**Mini-caso.** *"Tengo que usar una librería de pagos con 8 clases y un orden de llamadas complicado; quiero que mi código llame a `cobrar(monto)` y listo, y la interfaz `cobrar` la invento yo."*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Facade.** La pista está en *"la interfaz la invento yo"* (no me la impone nadie → no es Adapter) y en *"8 clases + orden complicado"* (escondo un subsistema → no es Decorator, que suma comportamiento sobre una sola cosa). **Adapter traduce porque no encaja; Facade simplifica porque es engorroso; Decorator acumula.**

---

## Par 3 — Chain of Responsibility vs Decorator

Los dos arman una **cadena de objetos que se contienen entre sí**. El UML es casi igual. Es **la trampa favorita de la cátedra**.

| | **Decorator** | **Chain of Responsibility** |
|---|---|---|
| ¿Se ejecutan todos? | **Sí**, cada capa **suma** | **No**, uno puede **cortar** |
| Intención | **Potenciar** el objeto | Que **alguien atienda** la petición |
| Resultado | El objeto envuelto, con más features | La petición, resuelta por **uno** |

**Mini-caso.** *"Cada request web pasa por: chequeo de autenticación, chequeo de permisos y límite de rate. Si alguno falla, la request se rechaza y los siguientes no corren."*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Chain of Responsibility.** La frase *"si alguno falla… los siguientes no corren"* es la firma: **alguien corta la cadena**. En Decorator **todas** las capas se ejecutan y suman. **Decorator acumula; CoR descarta.**

---

## Par 4 — Strategy vs State

Contexto que compone una interfaz + varias clases que la implementan. **UML idéntico**, intención opuesta.

| | **Strategy** | **State** |
|---|---|---|
| Qué representa | **Cómo** hacer algo (algoritmos) | **En qué etapa** estoy (ciclo de vida) |
| Quién elige la clase concreta | **El cliente**, desde afuera | **El propio estado**, desde adentro |
| ¿Se conocen entre sí? | **No** | **Sí** (`Pagado` crea un `Enviado`) |
| ¿Hay transiciones con reglas? | No | **Sí** |

**Mini-caso.** *"Un documento puede estar en Borrador, En revisión o Publicado. Desde Borrador se puede mandar a revisión, pero no publicar directo. Cada acción hace algo distinto según el estado."*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **State.** Hay **ciclo de vida** con **transiciones prohibidas** (*"no publicar directo desde Borrador"*), y el próximo estado lo decide el estado actual. En Strategy no hay etapas ni reglas de "a dónde puedo pasar": cualquier algoritmo vale en cualquier momento. **La pregunta que los parte: ¿hay reglas sobre a qué se puede pasar después?**

---

## Par 5 — Template Method vs Strategy

Los dos "varían el comportamiento". Se diferencian por la **herramienta** que usan.

| | **Template Method** | **Strategy** |
|---|---|---|
| Herramienta | **Herencia** (subclases) | **Composición** (le paso un objeto) |
| Qué varía | **Algunos pasos** de un algoritmo fijo | **El algoritmo entero** |
| Cuándo se decide | En **compilación** (elegís la subclase) | En **runtime** (cambiás el objeto) |
| Quién manda | La **base** llama a la subclase | El **cliente** le pasa la estrategia |

**Mini-caso.** *"Generar un reporte es siempre: traer datos → armar encabezado → volcar filas → guardar. Lo único que cambia entre PDF, Excel y HTML es cómo se arma el encabezado y con qué extensión se guarda."*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Template Method.** El **esqueleto es fijo** (mismos pasos, mismo orden) y solo cambian **algunos pasos** → herencia con un template method. En Strategy reemplazarías **el algoritmo entero** en runtime, no rellenarías huecos de uno fijo. **Esqueleto fijo + rellenos → Template Method; algoritmo entero intercambiable en caliente → Strategy.**

---

## Los desempates, todos juntos

La chuleta de una línea por par. Si te sabés esto, el parcial de patrones ya está.

| Par | La pregunta que lo parte |
|---|---|
| **Factory Method vs Abstract Factory** | ¿**un** producto o una **familia**? |
| **Abstract Factory vs Builder** | ¿varios productos **coherentes** o uno con **partes opcionales**? |
| **Adapter vs Facade** | ¿**traduce** porque no encaja o **simplifica** porque es engorroso? |
| **Adapter vs Decorator** | ¿**cambia** la interfaz o la **mantiene** y suma? |
| **Composite vs Decorator** | ¿**árbol** (muchos hijos) o **pila** (uno envuelto)? |
| **CoR vs Decorator** | ¿alguien **corta** o **todos suman**? |
| **Strategy vs State** | ¿**el cliente** elige o **el estado** decide el siguiente (hay transiciones)? |
| **Template Method vs Strategy** | ¿**herencia** (rellenar pasos) o **composición** (cambiar el algoritmo entero)? |

---

## Repaso relámpago de los 13 — creacionales y estructurales

Quiz oral. Te voy a decir el nombre y **vos me tirás intención en una línea + señal delatora**, sin mirar. Después chequeamos.

**Singleton · Factory Method · Abstract Factory · Builder · Adapter · Composite · Decorator · Facade**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> - **Singleton** — una **única instancia** global · *"una sola instancia en todo el sistema"*.
> - **Factory Method** — decidir **qué clase concreta** crear (un producto) · *"según el tipo, crear un X distinto"*.
> - **Abstract Factory** — crear una **familia coherente** de productos · *"una variante completa que no se mezcla"*.
> - **Builder** — armar **paso a paso** un objeto con muchas partes · *"muchos atributos opcionales / constructor telescópico"*.
> - **Adapter** — **traducir** entre dos interfaces incompatibles · *"no encaja y no lo puedo tocar"*.
> - **Composite** — **árbol** todo-parte, hoja y rama tratadas igual · *"contiene otros del mismo tipo, a cualquier profundidad"*.
> - **Decorator** — **sumar** comportamientos combinables sin subclasear · *"opcional, combinable, en runtime, sin tocar la clase"*.
> - **Facade** — **esconder** un subsistema tras una puerta simple · *"un punto de entrada único, esconder la complejidad"*.

---

## Repaso relámpago de los 13 — comportamiento

Ídem, la familia que más entró hoy.

**Strategy · Observer · Chain of Responsibility · Template Method · State**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> - **Strategy** — elegir **cómo** hacer algo entre algoritmos intercambiables · *"distintas formas de calcular/ordenar/pagar, elegible en runtime"*.
> - **Observer** — un cambio **avisa a varios** suscriptos · *"cuando pasa X, notificar a todos los interesados"*.
> - **Chain of Responsibility** — la petición **recorre una cadena** y alguien **corta** · *"escala / si no puede, que lo pase al siguiente"*.
> - **Template Method** — **esqueleto fijo**, la subclase rellena pasos · *"el procedimiento es el mismo, cambia cómo se hace cada paso"*.
> - **State** — el comportamiento **depende de la etapa**, con transiciones · *"según el estado / no se puede X hasta Y / máquina de estados"*.

---

## Ejercicio en vivo — cómo lo jugamos

Cuatro casos, **sin escribir nada**: todo hablado. Por cada uno, tres respuestas:

1. **¿De qué se queja el problema?** ¿Crear, estructurar o comunicar? → la **familia**.
2. **¿Qué patrón?** Y **qué palabra exacta** del enunciado te lo delató.
3. **¿Por qué no el parecido?** Te digo yo cuál es el parecido. **Esta es la respuesta que vale.**

> Van de menor a mayor dificultad. Si el 3 no te sale, el 2 no cuenta: en el parcial hay que saber **descartar** el que se le parece.

---

## Ejercicio en vivo — Caso 1

**El checkout.** Para procesar una venta, hoy el checkout tiene que llamar, en este orden exacto, a: `Stock.reservar()`, `Precios.calcular()`, `Pagos.cobrar()`, `Facturacion.emitir()`, `Mail.enviar()` y `Stock.confirmar()`. Ese bloque de seis llamadas está **copiado** en la web, en la app móvil y en el panel de administración. Si cambia el orden o se suma un paso, hay que tocar los tres lugares.

*El parecido a descartar: **Adapter**.*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **→ Facade.** *Estructurar.* Seis clases, un **orden exacto**, y el bloque **duplicado en tres clientes**. Una clase `CheckoutFacade` con `procesarVenta(carrito)` que orquesta los seis pasos adentro; web, móvil y admin llaman **una línea**.
>
> **¿Por qué no Adapter?** Nadie te obliga a respetar una interfaz existente ni hay incompatibilidad — **inventás** una interfaz cómoda que no existía. **Facade simplifica; Adapter traduce.**

---

## Ejercicio en vivo — Caso 2

**El banco.** Una fintech evalúa solicitudes de crédito. Antes de aprobar una, la solicitud pasa por una serie de controles: verificación de identidad, chequeo de deudas en el Veraz, control de ingreso mínimo y control de tope de monto. Si **cualquiera** falla, la solicitud se **rechaza ahí mismo** y los controles siguientes **ni se ejecutan**. Además, **cada sucursal usa controles distintos y en distinto orden**: Córdoba no aplica el tope, Rosario suma uno de antigüedad laboral al principio.

*El parecido a descartar: **Decorator**.*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **→ Chain of Responsibility.** *Comunicar.* La solicitud **recorre una secuencia**, cualquiera puede **cortar**, y el **orden se configura por sucursal**. Firma: *"si cualquiera falla, se rechaza ahí mismo y los siguientes ni se ejecutan"*.
>
> **¿Por qué no Decorator?** En Decorator **todas las capas se ejecutan y suman** (nadie se saltea). Acá el primer control que falla **corta** y los demás no corren. Además, Decorator existe para *potenciar*; acá no potenciás la solicitud, la **filtrás**. **Decorator acumula; CoR descarta.**

---

## Ejercicio en vivo — Caso 3

**El delivery.** Un pedido pasa por **En preparación → En camino → Entregado**, y puede **cancelarse**. Se puede cancelar mientras está *en preparación*, pero **no** una vez que salió. Se puede marcar *entregado* **solo si** está *en camino*. Hoy el código tiene un `estado: string` y **cuatro métodos, cada uno con el mismo `switch` de cinco casos**.

*El parecido a descartar: **Strategy**.*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **→ State.** *Comunicar.* **Ciclo de vida con etapas**, **lo que se puede hacer depende de la etapa**, **transiciones prohibidas** (*"no se puede cancelar una vez que salió"*) y el síntoma delator: el **`switch` repetido en los cuatro métodos**.
>
> **¿Por qué no Strategy?** (1) En Strategy **el cliente elige** el algoritmo desde afuera; acá **el estado elige el siguiente** desde adentro. (2) Las estrategias **no se conocen ni tienen orden**; los estados **sí**: *"de En camino solo se pasa a Entregado"*. (3) Strategy contesta *"¿cómo hago X?"*; State, *"¿qué puedo hacer ahora?"*. **La pregunta que los parte: ¿hay reglas sobre a qué se puede pasar después? Acá sí → State.**

---

## Ejercicio en vivo — Caso 4

**El editor de texto.** El editor exporta un documento. Al exportar, el usuario marca con checkboxes si quiere que vaya **numerado**, **con marca de agua**, **con índice automático**, o **cualquier combinación** de las tres. La clase `Documento` ya existe, funciona y **no se toca**. La combinación la elige el usuario **en el momento**, no está definida de antemano.

*El parecido a descartar: **Chain of Responsibility**.*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **→ Decorator.** *Estructurar.* Funcionalidades **opcionales y combinables** (2³ = 8 combinaciones), elegidas **en runtime**, sobre una clase que **no se toca**. Con herencia te explotarían las subclases (`NumeradoConIndice`, `NumeradoConMarcaYIndice`…).
>
> **¿Por qué no Chain of Responsibility?** Es el Caso 2 **dado vuelta**: acá **todas las capas se ejecutan y cada una suma** algo al documento. **Nadie corta** — si marcó las tres, las tres se aplican. En CoR **uno solo atiende** y la cadena se interrumpe. **Decorator acumula, CoR descarta.**
>
> **El detalle del día:** en el Caso 2 y en el Caso 4 el parecido era **el mismo par (Decorator vs CoR)**, una vez para cada lado. Si podés explicar por qué el 2 es CoR y el 4 es Decorator, ese par ya no te lo saca nadie.

---

## Cierre

Cerraste la unidad. **13 de 13**, y —más importante— con el **método** para reconocerlos:

- Frente a un enunciado: **¿crear, estructurar o comunicar?** → familia. Después, la **señal delatora** → patrón. Y por último, **por qué no el parecido** → el desempate, que es lo que da el punto.
- Todos salen de las mismas dos herramientas: **programar contra una interfaz** y **composición**. Los UML se parecen mucho entre patrones distintos; por eso **se compara la intención, no la estructura**.
- Ningún patrón es gratis: Singleton viola SRP y DIP, Composite tensiona ISP, Facade puede volverse "objeto dios". Saber explicar el **trade-off** vale más que recitar la definición.

> Lo que **no** hay que llevarse: la idea de que hay que meter patrones. *"Su mera existencia no implica su uso."* Un `if` bien puesto le gana a un patrón forzado. En el parcial, un patrón **mal justificado** resta más de lo que un nombre correcto suma.

**Después de clase:** ficha final de los 13, ejercicios de los 4 nuevos, un **simulacro de parcial integrador** y una batería de *"¿qué patrón es?"* para la noche anterior.

---

## Después de clase — Ficha final de los 13

Para repasar la noche antes del parcial. Por patrón: **intención en una línea + señal delatora + la trampa típica** (con qué se confunde).

| Patrón | Intención (1 línea) | Señal delatora | Trampa típica |
|---|---|---|---|
| **Singleton** | Una **única** instancia global | *"una sola instancia en todo el sistema"* | vs. variable global (es un objeto: se hereda, es lazy) |
| **Factory Method** | Decidir **qué clase** concreta crear (un producto) | *"según el tipo, crear un X distinto"* | vs. **Abstract Factory** (uno vs. familia) |
| **Abstract Factory** | Crear una **familia coherente** de productos | *"variante completa que no se mezcla"* | vs. **Factory Method** / **Builder** |
| **Builder** | Armar **paso a paso** un objeto con muchas partes | *"muchos opcionales / constructor telescópico"* | vs. **Abstract Factory** (uno con partes vs. varios coherentes) |
| **Adapter** | **Traducir** entre interfaces incompatibles | *"no encaja y no lo puedo tocar"* | vs. **Facade** / **Decorator** |
| **Composite** | **Árbol** todo-parte, hoja y rama tratadas igual | *"contiene otros del mismo tipo, a cualquier profundidad"* | vs. **Decorator** (árbol vs. pila) |
| **Decorator** | **Sumar** comportamientos combinables | *"opcional, combinable, en runtime, sin tocar la clase"* | vs. **CoR** (suma vs. corta) / **Composite** |
| **Facade** | **Esconder** un subsistema tras una puerta simple | *"punto de entrada único, esconder complejidad"* | vs. **Adapter** (simplifica vs. traduce) |
| **Strategy** | Elegir **cómo** hacer algo entre algoritmos | *"distintas formas de X, elegible en runtime"* | vs. **State** / **Template Method** |
| **Observer** | Un cambio **avisa a varios** suscriptos | *"cuando pasa X, notificar a los interesados"* | vs. mediador/pub-sub (dirección del aviso) |
| **Chain of Responsibility** | La petición **recorre una cadena**, alguien **corta** | *"escala / si no puede, que lo pase al siguiente"* | vs. **Decorator** (corta vs. suma) |
| **Template Method** | **Esqueleto fijo**, la subclase rellena pasos | *"el procedimiento es el mismo, cambia el cómo"* | vs. **Strategy** (herencia vs. composición) |
| **State** | El comportamiento **depende de la etapa** | *"según el estado / no se puede X hasta Y"* | vs. **Strategy** (transiciones con reglas) |

---

## Después de clase — Ejercicios

Para cada uno: **1)** ¿el problema es de *crear*, *estructurar* o *comunicar*? **2)** ¿qué patrón, y **por qué no el parecido**? **3)** bosquejá el UML y un esqueleto en TypeScript.

**A.** El chat de soporte deriva las consultas: primero las atiende un bot; si no puede, pasa a un agente de nivel 1; si el agente no puede, escala a nivel 2; y si tampoco, a un supervisor. Cada nivel decide si resuelve o escala. El cliente que manda la consulta **no sabe** quién va a terminar atendiéndolo.

**B.** El sistema genera reportes en PDF, Excel y HTML. En los tres, el procedimiento es **idéntico**: traer los datos de la base, armar el encabezado, volcar las filas, agregar el pie con la fecha y guardar el archivo. Lo único que cambia es **cómo** se arma el encabezado, **cómo** se vuelca cada fila y **con qué extensión** se guarda. Hoy hay tres clases con el mismo código copiado y ya se desincronizaron.

**C.** Un reproductor de música tiene los botones *play*, *pausa* y *stop*. Lo que hace cada botón depende de la situación: si está detenido, *play* arranca la canción; si está reproduciendo, *play* no hace nada y *pausa* la suspende; si está pausado, *play* la retoma desde donde iba. Además, desde *detenido* no se puede pasar a *pausado*. El código actual tiene un `switch (this.situacion)` en cada uno de los tres métodos.

**D.** Para hacer un backup, tu módulo tiene que llamar en orden a `Compresor.zip()`, `Cifrador.encriptar()`, `Nube.subir()` y `Registro.log()`. Ese bloque está pegado en tres comandos distintos del sistema. Querés que cada comando llame a `hacerBackup(datos)` y nada más, y que la interfaz `hacerBackup` la definís vos.

---

## Después de clase — Soluciones

**A → Chain of Responsibility.** *Comunicar:* la palabra **"escala"** y la estructura *"si no puede, que lo pase al siguiente"* son la firma exacta. `abstract class Soporte` con `siguiente: Soporte`; `Bot`, `Nivel1`, `Nivel2`, `Supervisor`; cada uno resuelve o llama a `super.atender()`. El emisor **no sabe** quién atiende.
> *No es Decorator:* acá **uno solo** termina atendiendo (la cadena **se corta**); en Decorator **todos** los eslabones aportan.

**B → Template Method.** *Comportamiento:* **"el procedimiento es idéntico"** y **"lo único que cambia es cómo"** — la definición del patrón, palabra por palabra. Clase abstracta `Reporte` con el template method `generar()` (trae datos → encabezado → filas → pie → guardar) y los pasos abstractos `armarEncabezado()`, `volcarFila()`, `extension()`. `ReportePDF`, `ReporteExcel`, `ReporteHTML` los redefinen. El código duplicado que "se desincronizó" **desaparece**: vive una sola vez en la clase base.
> *No es Strategy:* no se cambia el algoritmo **entero en runtime**; se rellenan **algunos pasos** de un esqueleto fijo, y se decide **eligiendo la subclase**. Template Method usa **herencia**; Strategy, **composición**.

**C → State.** *Comportamiento:* **ciclo de vida** (detenido / reproduciendo / pausado), **lo que hace cada acción depende de la etapa**, hay **transiciones prohibidas** (*"desde detenido no se puede pasar a pausado"*) y el síntoma es el **`switch` repetido en cada método**. `interface EstadoReproductor { play(r), pausa(r), stop(r) }` con `Detenido`, `Reproduciendo`, `Pausado`; el `Reproductor` **delega** y no tiene ni un `if`.
> *No es Strategy:* el reproductor **no elige** su estado desde afuera — **cada estado decide el siguiente** (`Reproduciendo.pausa()` hace `r.cambiarEstado(new Pausado())`). Hay **transiciones con reglas**; en Strategy cualquier algoritmo vale en cualquier momento.

**D → Facade.** *Estructurar:* cuatro clases, un **orden fijo**, el bloque **duplicado en tres comandos**, y la interfaz `hacerBackup` **la inventás vos**. `class BackupFacade { hacerBackup(datos) { … } }` orquesta los cuatro pasos adentro; los comandos llaman una línea.
> *No es Adapter:* no hay ninguna interfaz preexistente que tengas que respetar ni incompatibilidad que traducir — **inventás** una puerta cómoda sobre un subsistema. **Facade simplifica; Adapter traduce.**

---

## Después de clase — Simulacro de parcial (integrador)

**Un solo enunciado, extenso, que integra cuatro patrones.** Este es el formato que más cae: un caso realista donde conviven varios patrones que resuelven **problemas distintos** dentro del mismo sistema. El trabajo es (a) darte cuenta de que son **problemas separados**, (b) nombrar cada patrón con su señal delatora, (c) descartar el parecido de cada uno y (d) bosquejar cómo se **conectan** entre sí. Leelo entero antes de mirar la solución.

**El sistema de tickets de soporte.**

Estás diseñando desde cero el módulo de soporte de una empresa. El código viejo es un desastre y te dan cuatro requerimientos.

**(1)** La **configuración** del sistema —mail del administrador, límites de adjuntos, plantillas de respuesta— se carga **una sola vez** al arrancar, desde un archivo, y tiene que ser **la misma para toda la aplicación**: cualquier módulo (el de tickets, el de reportes, el de notificaciones) la consulta, y nunca puede haber dos configuraciones distintas dando vueltas. Hoy cada módulo se arma la suya leyendo el archivo por su cuenta, y ya pasó que quedaron desincronizadas.

**(2)** Al **crear un ticket** hay que armar un objeto con muchos campos: título y descripción son **obligatorios**, pero además puede llevar prioridad, categoría, adjuntos, etiquetas, SLA y cliente asociado, todos **opcionales**. Hoy existe un `new Ticket(titulo, desc, prioridad, categoria, adjuntos, etiquetas, sla, cliente)` de **8 parámetros** que nadie recuerda en qué orden va, y para un ticket simple hay que pasar media docena de `undefined`.

**(3)** Un ticket recién creado, **antes de entrar a la cola**, pasa por una serie de **filtros de admisión**: detección de spam, verificación de que el cliente esté activo y chequeo de duplicados. Si **cualquiera** de ellos lo marca, el ticket se **descarta ahí mismo** y los filtros siguientes **ni se ejecutan**. Además, **distintas colas usan filtros distintos y en distinto orden**: la cola de "clientes premium" se saltea el filtro de spam, la de "público general" suma uno de detección de insultos al principio.

**(4)** Una vez admitido, el ticket tiene un **ciclo de vida**: **Abierto → En progreso → Resuelto → Cerrado**. Desde *Cerrado* no se puede volver a *Abierto*, y solo se puede marcar *Resuelto* si está *En progreso*. Lo que se puede hacer con el ticket **depende de en qué etapa esté**, y el código viejo lo resuelve con el **mismo `switch` de cuatro casos repetido en cada método** (`tomar`, `resolver`, `cerrar`, `reabrir`).

**Consigna.** ¿Cuántos patrones hay y cuáles? Para cada uno: familia, señal delatora y **por qué no el parecido**. Y lo más importante: explicá **cómo se encadenan en el ciclo de vida de un ticket** (en qué orden entra en juego cada patrón).

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Son cuatro patrones, y cada requerimiento es uno.** Lo valioso no es solo nombrarlos: es ver que **cada uno resuelve un problema distinto** y que se **encadenan** a lo largo de la vida del ticket.
>
> **(1) → Singleton.** *Crear.* *"una sola vez"* + *"la misma para toda la aplicación"* + *"cualquier módulo la consulta"* = instancia única + punto de acceso global. Constructor privado, `private static instancia`, `static getInstancia()`.
> - *No es una variable global suelta:* es un **objeto** (puede ser lazy, implementar una interfaz).
> - *Trade-off que hay que decir sí o sí:* **viola SRP y DIP** (la clase guarda config *y* administra su instancia; quien la usa no declara que depende de ella). Sería mejor **inyectarla por constructor**; se justifica acá solo porque el enunciado exige el acceso global explícito.
>
> **(2) → Builder.** *Crear.* **Constructor telescópico** de manual (2 obligatorios + 6 opcionales, `undefined` por todos lados). `new TicketBuilder().conTitulo(...).conDescripcion(...).conPrioridad("alta").conSLA(24).construir()`: se lee, el orden no importa y lo que no aplica no se escribe.
> - *No es Abstract Factory:* hay **un solo** producto (el ticket) con muchas **partes opcionales**, no varios productos que deban ser coherentes entre sí.
>
> **(3) → Chain of Responsibility.** *Comunicar.* Filtros **en secuencia**, cualquiera **corta** (*"si cualquiera lo marca, se descarta y los siguientes ni se ejecutan"*), y el **orden se configura por cola**. `abstract class FiltroAdmision` con `siguiente`; `Spam`, `ClienteActivo`, `Duplicado`, `Insultos`. Cada cola arma **otra cadena** con los mismos objetos en otro orden: cero código nuevo.
> - *No es Decorator:* acá los filtros **cortan** (el primero que marca frena la cadena); en Decorator **todos** se ejecutan y suman. **CoR descarta; Decorator acumula.**
>
> **(4) → State.** *Comunicar.* **Ciclo de vida** con **transiciones prohibidas** (*"desde Cerrado no se vuelve a Abierto"*, *"solo se resuelve si está En progreso"*), comportamiento que **depende de la etapa**, y el síntoma delator: el **`switch` repetido en los cuatro métodos**. `interface EstadoTicket` con `Abierto`, `EnProgreso`, `Resuelto`, `Cerrado`; el `Ticket` (contexto) **delega** y no tiene ni un `if`. Cada estado decide el siguiente desde adentro.
> - *No es Strategy:* hay **transiciones con reglas** y **el estado elige el siguiente**, no el cliente desde afuera. *"¿qué puedo hacer ahora?"*, no *"¿cómo hago X?"*.
>
> **Cómo se encadenan (lo que vale doble):** seguí la vida de **un** ticket y los cuatro patrones aparecen **en orden**:
> 1. Al arrancar la app, la **config** (Singleton) ya está disponible para todos.
> 2. Llega una consulta → el **Builder** arma el `Ticket` con los campos que haya.
> 3. Ese ticket entra a la **cadena de filtros** (CoR): o lo descarta alguno, o pasa a la cola.
> 4. Si pasó, empieza a vivir su **ciclo de vida** (State): Abierto → En progreso → Resuelto → Cerrado.
>
> Fijate que **cada patrón vive en una familia y una etapa distinta**: dos creacionales (armar el sistema y armar el ticket) y dos de comportamiento (admitir y gobernar el ciclo). No se pisan porque **atacan problemas distintos**. Y los dos de *comunicar* conviven sin chocar: **CoR filtra la entrada** (una sola pasada, alguien corta) y **State gobierna el después** (etapas con transiciones que se repiten en el tiempo). Reconocer que son **cuatro problemas separados** —y en qué orden entran— es exactamente lo que distingue un 10 de un 6.

---

## Simulacro — el UML resuelto

Así se ve el diseño completo. Fijate cómo los **cuatro patrones** conviven en el mismo diagrama, cada uno en su recuadro, y cómo se **encadenan** siguiendo la vida del ticket (numerado 1 → 4).

> [IMAGEN: diagrama de clases UML integrador del sistema de tickets, organizado en CUATRO bloques rotulados, uno por patrón, y con una franja numerada 1→4 arriba que marca el orden en que entran en juego. BLOQUE 1 «Singleton» (arriba a la izquierda, rotulado "1. al arrancar la app"): una sola caja "Configuracion" con "- instancia: Configuracion" subrayado (estático), "- Configuracion()" como constructor privado (resaltar el signo menos) y "+ getInstancia(): Configuracion" subrayado; una flecha curva de autorreferencia sobre el atributo instancia. BLOQUE 2 «Builder» (arriba al centro, rotulado "2. al crear el ticket"): la clase "TicketBuilder" con métodos encadenables conTitulo(), conDescripcion(), conPrioridad(), conCategoria(), conSLA() y construir(): Ticket; una flecha de composición (rombo relleno) y una de dependencia punteada «create» desde "TicketBuilder" hacia la clase producto "Ticket". La clase "Ticket" es el centro del diagrama y se comparte con el Bloque 4 (ver abajo). BLOQUE 3 «Chain of Responsibility» (a la derecha, rotulado "3. filtros de admisión"): una clase abstracta "FiltroAdmision" (nombre en itálica) con "- siguiente: FiltroAdmision" y "+ encadenar(f)" y "+ admitir(t: Ticket)"; una flecha de agregación (rombo vacío) autorreferencial etiquetada "siguiente 0..1"; debajo, por herencia (triángulo vacío), las clases "Spam", "ClienteActivo", "Duplicado" e "Insultos"; una tira horizontal ilustrativa muestra el ticket entrando a la cadena y una flecha ROJA que se desvía a "Descartado" con la etiqueta "cualquier filtro corta". BLOQUE 4 «State» (abajo al centro, rotulado "4. ciclo de vida"): la clase "Ticket" (el contexto, la misma caja del Bloque 2) con "- estado: EstadoTicket" y los métodos "+ tomar()", "+ resolver()", "+ cerrar()", "+ reabrir()" y "+ cambiarEstado(e)"; una flecha de composición (rombo relleno) desde "Ticket" hacia la interfaz «interface» "EstadoTicket" (con tomar(t), resolver(t), cerrar(t), reabrir(t)); debajo, por realización (punteada, triángulo vacío), las clases "Abierto", "EnProgreso", "Resuelto" y "Cerrado"; desde cada estado, una flecha de dependencia punteada que VUELVE a "Ticket" etiquetada "cambiarEstado(new ...)". Al costado del Bloque 4, un mini-diagrama de transiciones aparte: cuatro círculos "Abierto → EnProgreso → Resuelto → Cerrado" con las flechas etiquetadas, y una X sobre la flecha que iría de "Cerrado" de vuelta a "Abierto" para marcar la transición prohibida. FLECHA DE FLUJO que cruza el diagrama uniendo los bloques en orden: del Builder (2) sale el "Ticket" recién creado → entra a la cadena de filtros (3) → si pasa, empieza su ciclo de vida (4); y una nota indica que la "Configuracion" (1) es consultada por todos los bloques. Usar un color de fondo distinto por bloque para que se lea de un vistazo que son cuatro patrones separados que colaboran.]

> [RESPUESTA: ocultar, permitir ver con un click]
>
> Lo que el diagrama tiene que dejar clarísimo, y suele ser lo que se pregunta oral: **la clase `Ticket` aparece en dos bloques** (es el **producto** del Builder y el **contexto** del State). Eso no es un error de dibujo: es la prueba de que los patrones **colaboran** sin pisarse — el Builder **arma** el ticket, y una vez armado y admitido, el State **lo gobierna**. El Singleton no toca a nadie por asociación directa: es consultado *desde adentro* de los métodos, y por eso mismo **esconde su dependencia** (el defecto que hay que saber señalar).

---

## Después de clase — Batería "¿qué patrón es?"

Enunciados cortos para entrenar el reflejo. Tapá la última slide, respondé todos de corrido y después corregí.

1. *"Quiero envolver un servicio SOAP viejo para que mi código lo use como si fuera mi interfaz `Repositorio`."*
2. *"Un menú tiene ítems y submenús, que a su vez tienen ítems y submenús. Quiero calcular cuántas opciones hay en total."*
3. *"Necesito que exista una sola conexión al broker de mensajes en toda la app."*
4. *"El precio final se calcula distinto según si el cliente es común, VIP o mayorista, y quiero poder cambiar la política sin tocar el resto."*
5. *"Cuando cambia el stock de un producto, se tienen que actualizar solos el carrito, la vitrina y el panel de reportes."*
6. *"Al exportar puedo pedir que vaya comprimido, cifrado, ambos o ninguno; lo elijo en el momento."*
7. *"Un envío pasa por Pendiente → Despachado → En reparto → Entregado, y desde cada etapa solo se puede avanzar a la siguiente."*
8. *"Tengo que crear un botón, un checkbox y un panel, y todos tienen que ser del mismo tema (claro u oscuro), sin mezclarse."*

---

## Después de clase — Batería: respuestas

> [Esta slide es toda respuesta: no se muestra hasta que Tomás resolvió la anterior.]

1. **Adapter** — envolver algo **incompatible** que no controlás y **traducir** a tu interfaz.
2. **Composite** — **árbol** todo-parte (ítems y submenús), misma operación sobre hoja y rama.
3. **Singleton** — *"una sola"* instancia en toda la app.
4. **Strategy** — elegir **cómo** calcular entre algoritmos intercambiables, sin transiciones.
5. **Observer** — un cambio **avisa a varios** suscriptos.
6. **Decorator** — funcionalidades **opcionales y combinables** en runtime, que **se acumulan**.
7. **State** — **ciclo de vida** con **transiciones** (solo avanzar a la siguiente etapa).
8. **Abstract Factory** — una **familia** de productos coherentes que **no se mezclan**.

> Trampas plantadas a propósito: el **1** tienta con Facade (pero acá hay incompatibilidad + una sola clase → traduce, no simplifica); el **6** tienta con CoR (pero acumula, no corta); el **7** tienta con Strategy (pero hay transiciones). Si caíste en alguna, volvé a la ficha de los 13 y a la tabla de desempates.

---

## Después de clase — Lectura recomendada

- **PDF de la cátedra, Unidad 4:** la **comparación entre Chain of Responsibility y Decorator** está explícita ahí y **cae en el parcial**. Leela con la tabla de la slide de CoR al lado.
- **Refactoring Guru — Patrones de diseño** (en español): `refactoring.guru/es/design-patterns`. Fichas de **Facade, Chain of Responsibility, Template Method y State**. La sección **"Relaciones con otros patrones"** de cada ficha es oro puro para los desempates.
- **SOLID (Clase 2) con estos patrones en la mano:** fijate que **CoR, State y Template Method son OCP hecho patrón** (agregás un caso nuevo = una clase nueva, sin tocar lo existente).

> Para el parcial: no estudies "los 13 patrones" — estudiá **los 8 desempates** de la tabla. El que sabe descartar el parecido, aprueba; el que solo memorizó nombres, se traba en la justificación.
