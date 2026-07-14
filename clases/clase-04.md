# Clase 4 — Los creacionales que faltan + Composite

> **Unidad:** 4 (Patrones de diseño) · **Lenguaje de ejemplos:** TypeScript

---

## Clase 4 — Los creacionales que faltan + Composite

**Programación II · Unidad 4**

> *"El patrón no se elige por bonito: se elige porque el problema ya te lo estaba pidiendo."*

En la Clase 3 instalamos el **método**: leer el enunciado, preguntarse *¿el problema es de **crear**, **estructurar** o **comunicar**?* y recién ahí buscar el patrón. Vimos 5 anclas. Hoy vamos por **4 más**, con el mismo molde de siempre: cerramos la familia **creacional** entera y sumamos el estructural más lindo de todos, **Composite**.

---

## Dónde estamos parados

La cátedra evalúa **13 patrones**. No son los 22 del catálogo completo: son estos, y con estos alcanza.

| Familia | Patrones |
|---|---|
| **Creacionales (4)** | Singleton · Factory Method · Abstract Factory · Builder |
| **Estructurales (4)** | Adapter · Composite · Decorator · Facade |
| **Comportamiento (5)** | Strategy · Observer · Chain of Responsibility · Template Method · State |

En la **Clase 3** viste 5: **Factory Method, Adapter, Decorator, Strategy, Observer**.
Hoy van **4**: **Singleton, Abstract Factory, Builder** (con esto la familia creacional queda **completa**) y **Composite**.

> Quedan **4 para la Clase 5**: **Facade, Chain of Responsibility, Template Method y State**. Ahí cerramos el catálogo y arranca la práctica de reconocimiento.

---

## Arrancamos con un repaso rápido

Antes de meter patrones nuevos, quiero escucharte los 5 de la clase pasada. **Una línea cada uno**, sin mirar apuntes: no la definición de manual, sino **la señal delatora** (qué frase de un enunciado te hace pensar en él).

1. **Factory Method** → ?
2. **Adapter** → ?
3. **Decorator** → ?
4. **Strategy** → ?
5. **Observer** → ?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **1)** un `switch`/`new` que decide **qué clase crear** · **2)** dos interfaces que **no encajan** y una no la controlás · **3)** **sumar funcionalidades combinables** sin tocar la clase ni explotar en subclases · **4)** elegir **cómo hacer algo** entre varios algoritmos, cambiable en runtime · **5)** un cambio tiene que **avisarle a varios** que se suscriben y se van.

---

## El método, otra vez (no lo sueltes)

Todo lo de hoy se apoya en el mismo reflejo de la Clase 3. Frente a un enunciado: **¿de qué se queja el problema?**

- *"No sé qué instanciar / el `new` está desparramado / quiero controlar la creación"* → **Creacional**.
- *"Tengo que hacer encajar / envolver / esconder / representar un todo-y-sus-partes"* → **Estructural**.
- *"Tengo que elegir un comportamiento / avisar / coordinar quién hace qué"* → **Comportamiento**.

> Y el hilo que ya conocés: casi todos los patrones se resuelven con las **mismas dos herramientas** —**programar contra una interfaz** y **composición**—. Si en algún momento te perdés, buscá esas dos y el patrón aparece solo.

---

## Creacionales — Singleton

**Familia:** creacional.

**Intención:** garantizar que una clase tenga **una única instancia** en todo el sistema y proveer un **punto de acceso global** a ella.

Dos cosas hace, no una: (1) **impide** que se cree más de un objeto, y (2) **ofrece la manera** de conseguir ese objeto desde cualquier lado.

> Analogía: el **gobierno** de un país. Puede cambiar quién lo integra, pero "el gobierno de Argentina" es **uno solo** y todos saben cómo llegar a él. No podés fundar un segundo gobierno paralelo porque te haga falta.

---

## Singleton — señal delatora

Sospechá Singleton cuando…

- Necesitás que **exista exactamente un** objeto de algo: la **configuración** del sistema, un **pool de conexiones**, un **logger**, la caché.
- Crear una segunda instancia sería **un error o un desperdicio** (dos conexiones a la base donde debería haber una).
- El enunciado dice: *"una única instancia"*, *"accesible desde todo el sistema"*, *"cargada una sola vez en memoria"*.

Y el mecanismo, siempre el mismo: **constructor privado** + un **método estático** que devuelve la instancia.

---

## Razoná — ¿y por qué no una variable global?

Si lo único que quiero es "un objeto único al que todos accedan"… **una variable global hace eso**. O una clase con todos los métodos `static`.

Antes de la próxima slide, pensá y decime:

- ¿Qué le podés hacer a una **clase Singleton** que a una **clase estática** no? (pista: pensá en `extends` y en `implements`)
- Si la configuración tarda 2 segundos en cargar de disco, ¿cuándo querés que se cargue: **al arrancar el programa** o **la primera vez que alguien la pide**? ¿Cuál de las dos formas te deja elegir?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> La respuesta de la cátedra: el Singleton *"es más plástico que una clase del tipo `final` con funciones estáticas, ya que **puede ser extendida y sus métodos sobrescritos**"*. O sea: **es un objeto**, entonces puede implementar interfaces, heredarse y crearse **en el momento en que se lo necesita** (*lazy*). Una variable global es solo un dato colgado; un Singleton es un objeto con reglas.

---

## Singleton — el código

```typescript
class Configuracion {
  // 1. la única instancia, guardada en la propia clase
  private static instancia: Configuracion | null = null;

  private valores = new Map<string, string>();

  // 2. constructor PRIVADO: nadie de afuera puede hacer new Configuracion()
  private constructor() {
    this.valores.set("moneda", "ARS");   // simulá acá una carga costosa (disco, red...)
  }

  // 3. único punto de acceso
  static getInstancia(): Configuracion {
    if (Configuracion.instancia === null) {
      Configuracion.instancia = new Configuracion();   // se crea recién en el primer pedido (lazy)
    }
    return Configuracion.instancia;
  }

  get(clave: string): string | undefined { return this.valores.get(clave); }
  set(clave: string, valor: string): void { this.valores.set(clave, valor); }
}

const a = Configuracion.getInstancia();
const b = Configuracion.getInstancia();

console.log(a === b);          // true → es literalmente el mismo objeto

a.set("moneda", "USD");
console.log(b.get("moneda"));  // "USD" → lo que cambia uno, lo ve el otro
```

Las tres piezas que **siempre** están: **atributo estático privado**, **constructor privado**, **método estático de acceso**.

---

## Singleton — UML

> [IMAGEN: diagrama de clases UML del patrón Singleton. Una sola caja de clase llamada "Configuracion", dividida en tres compartimentos. En el compartimento de atributos: "- instancia: Configuracion" con la palabra "static" indicada (subrayado, que es como UML marca lo estático) y "- valores: Map". En el compartimento de métodos: "- Configuracion()" marcado como constructor privado (el signo menos es clave, resaltarlo), "+ getInstancia(): Configuracion" subrayado por ser estático, "+ get(clave)" y "+ set(clave, valor)". A la izquierda, una caja "Client" con una flecha punteada de dependencia hacia Configuracion, etiquetada "getInstancia()". Una flecha curva sale de Configuracion y vuelve a entrar a sí misma (autorreferencia) sobre el atributo "instancia", con una nota que dice: "la clase se guarda a sí misma". Otra nota, apuntando al constructor privado, dice: "acá está el truco: si el constructor es privado, NADIE puede hacer new desde afuera".]

**Conexión SOLID:** ninguna, y ahí está el problema…

---

## Pará y pensá — la dependencia que no se ve

Singleton es el patrón que **más se enseña y más se critica**. Antes de que te dé la lista, buscá vos el problema. Mirá estas dos versiones de la **misma** clase:

```typescript
// Versión A — se consigue la config por adentro
class ServicioDePagos {
  cobrar(monto: number) {
    const moneda = Configuracion.getInstancia().get("moneda");   //
    // ...
  }
}

// Versión B — la config se la pasan por constructor
class ServicioDePagos {
  constructor(private config: Configuracion) {}                  //
  cobrar(monto: number) {
    const moneda = this.config.get("moneda");
    // ...
  }
}
```

- Imaginate que abrís el archivo y **solo leés la primera línea** de cada clase. En la **B**, ¿de qué depende `ServicioDePagos`? ¿Y en la **A**?
- En la A la dependencia **existe igual** (el acoplamiento está), pero **no aparece en ningún lado visible**. ¿Eso a qué principio de la Clase 2 te suena que le falta el respeto?
- Última: contá **cuántas responsabilidades** tiene la clase `Configuracion`. No una: mirá bien qué hace además de guardar valores.

---

## Singleton — la letra chica (cuándo NO usarlo)

La cátedra lo dice sin vueltas: el Singleton **tiene ventajas y desventajas**, y las desventajas son serias.

**Ventajas**
- Certeza de que hay **una sola instancia**, y un punto de acceso claro.
- **Inicialización perezosa**: el objeto se crea recién cuando alguien lo pide por primera vez.

**Desventajas**
- **Viola SRP**: la clase hace *su trabajo* (guardar la configuración) **y además** se encarga de controlar su propia instanciación. Son **dos motivos de cambio** en una sola clase.
- **Viola DIP y esconde las dependencias**: quien lo usa lo consigue por adentro (`getInstancia()`), así que la dependencia **no aparece en la firma**. El acoplamiento existe igual, pero **invisible**: para saber de qué depende una clase, no te alcanza con leerla, tenés que leer todo su cuerpo.
- **Es estado global disfrazado**: cualquier parte del sistema puede cambiarle un valor, y **todas** las demás lo ven. Cuando algo anda mal, *"¿quién me cambió la moneda a USD?"* puede ser cualquiera de las 200 clases.
- En entornos **multihilo**, dos hilos pueden entrar juntos al `if` y crear **dos** instancias (en TS/JS de un solo hilo no te pasa, pero es *el* clásico bug del patrón en Java o C#).

> Regla práctica: **antes de meter un Singleton, preguntate si no alcanza con pasar el objeto por constructor** (inyección de dependencias). Casi siempre alcanza. Singleton es el patrón que más se usa **mal**: si en un parcial el enunciado no dice explícitamente *"una única instancia"*, probablemente no sea Singleton.

---

## Creacionales — Abstract Factory

**Familia:** creacional.

**Intención:** crear **familias de objetos relacionados** sin especificar sus clases concretas, y **garantizar que los productos de una misma familia se usen juntos**.

La palabra clave es **familia**. No es "un producto", son **varios productos que tienen que ser coherentes entre sí**.

> Analogía: comprar **muebles de un mismo juego**. Si elegís el estilo "moderno", querés la silla moderna, la mesa moderna y el sillón moderno. Lo que **no** querés es terminar con una silla moderna y una mesa victoriana. La fábrica te asegura que **todo lo que salga de ella sea del mismo estilo**.

---

## Abstract Factory — señal delatora

Sospechá Abstract Factory cuando…

- Tenés **varios productos** que **varían juntos** por "estilo", "plataforma", "proveedor" o "tema": botón + checkbox + menú; o conexión + comando + lector de una base de datos.
- El enunciado te pide **que no se mezclen**: *"si el sistema corre en Windows, toda la interfaz debe ser de Windows"*.
- Palabras que lo delatan: **familia de productos**, **conjunto coherente**, **variante completa**, **skin/tema/plataforma/proveedor**.

```typescript
// Productos abstractos: el cliente solo conoce estas interfaces
interface Boton    { render(): string; }
interface CheckBox { render(): string; }
```

---

## Abstract Factory — el código

```typescript
// --- Familia 1: Windows
class BotonWindows    implements Boton    { render() { return "[ Botón cuadrado Windows ]"; } }
class CheckBoxWindows implements CheckBox { render() { return "[x] CheckBox Windows"; } }

// --- Familia 2: macOS
class BotonMac    implements Boton    { render() { return "( Botón redondeado macOS )"; } }
class CheckBoxMac implements CheckBox { render() { return "(x) CheckBox macOS"; } }

// La fábrica abstracta: sabe crear la FAMILIA COMPLETA, no un producto suelto
interface FabricaUI {
  crearBoton(): Boton;
  crearCheckBox(): CheckBox;
}

class FabricaWindows implements FabricaUI {
  crearBoton(): Boton       { return new BotonWindows(); }
  crearCheckBox(): CheckBox { return new CheckBoxWindows(); }
}
class FabricaMac implements FabricaUI {
  crearBoton(): Boton       { return new BotonMac(); }
  crearCheckBox(): CheckBox { return new CheckBoxMac(); }
}

// El cliente recibe UNA fábrica y ya no puede mezclar estilos aunque quiera
class Formulario {
  constructor(private ui: FabricaUI) {}          // ← no sabe si es Windows o Mac
  dibujar(): string {
    return `${this.ui.crearBoton().render()}  ${this.ui.crearCheckBox().render()}`;
  }
}

const so = "mac";                                 // esto se decide UNA sola vez, al arrancar
const fabrica: FabricaUI = so === "mac" ? new FabricaMac() : new FabricaWindows();

console.log(new Formulario(fabrica).dibujar());
// ( Botón redondeado macOS )  (x) CheckBox macOS
```

Fijate: **la coherencia está garantizada por construcción**. `Formulario` **no puede** producir un botón Mac con un checkbox de Windows aunque se lo proponga.

---

## Razoná — Factory Method vs Abstract Factory

Los dos "fabrican". Los dos evitan el `new` cableado. Este par **cae seguro en el parcial**. Pensalo con estos dos enunciados:

**Enunciado 1.** *"Según el nivel del juego, la lógica de oleadas tiene que crear un enemigo distinto: lobo, murciélago o caballero."*

**Enunciado 2.** *"El juego tiene modo Bosque y modo Cueva. En cada modo cambian el enemigo, el sonido ambiente Y la textura del piso, y nunca se pueden mezclar: no puede aparecer un lobo con el sonido de la cueva."*

- ¿Cuántos **tipos distintos de producto** hay en cada enunciado? Contalos.
- ¿En cuál el problema es *"¿qué clase concreta instancio?"* y en cuál es *"¿cómo garantizo que todo lo que creo sea del mismo juego?"*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Enunciado 1 → Factory Method** (UN producto, varias variantes; una subclase decide cuál). **Enunciado 2 → Abstract Factory** (VARIOS productos que varían **en bloque** y no pueden mezclarse).
>
> La regla corta: **Factory Method = un producto. Abstract Factory = una familia de productos.** De hecho, una Abstract Factory suele estar hecha **por dentro** de varios Factory Methods (mirá: `crearBoton()` y `crearCheckBox()` son, cada uno, un método fábrica).

---

## Abstract Factory — UML

> [IMAGEN: diagrama de clases UML del patrón Abstract Factory, en tres bloques. BLOQUE IZQUIERDO (las fábricas): arriba una interfaz «interface» "FabricaUI" con los métodos crearBoton(): Boton y crearCheckBox(): CheckBox; debajo, dos clases "FabricaWindows" y "FabricaMac" conectadas a ella por realización (línea punteada con triángulo vacío). BLOQUE DERECHO (los productos), dividido en dos columnas: la columna de arriba muestra la interfaz «interface» "Boton" con render(), y debajo de ella, por realización, "BotonWindows" y "BotonMac"; la columna de abajo muestra la interfaz «interface» "CheckBox" con render(), y debajo, por realización, "CheckBoxWindows" y "CheckBoxMac". BLOQUE INFERIOR IZQUIERDO: una clase "Formulario" (el cliente) con una flecha de asociación hacia la interfaz "FabricaUI" (etiquetada "ui") y flechas de dependencia punteadas hacia las interfaces "Boton" y "CheckBox". Desde "FabricaMac" salen dos flechas de dependencia punteadas etiquetadas «create» hacia "BotonMac" y "CheckBoxMac"; lo mismo desde "FabricaWindows" hacia sus productos. IMPORTANTE: sombrear con un color de fondo los productos de la familia Mac (BotonMac + CheckBoxMac) y con otro color los de la familia Windows, para que se vea a simple vista que son DOS FAMILIAS que no se mezclan. Una nota apuntando a "Formulario" dice: "el cliente solo conoce las INTERFACES: nunca nombra un producto concreto".]

**Conexión SOLID:** OCP (agregar la familia "Linux" = una fábrica nueva, sin tocar `Formulario`), DIP (el cliente depende de abstracciones) y SRP (la creación vive en un solo lugar).

---

## Creacionales — Builder

**Familia:** creacional.

**Intención:** separar la **construcción** de un objeto complejo de su **representación**, para poder armarlo **paso a paso** y que el mismo proceso de construcción produzca **objetos distintos**.

Se usa cuando el objeto tiene **muchas partes opcionales** y el `new` se vuelve un monstruo.

> Analogía: pedir una pizza. No hay un único "constructor de pizza" con 12 parámetros: elegís la masa, después la salsa, después vas agregando ingredientes. **Paso a paso**, y podés parar donde quieras.

---

## Pará y pensá — el constructor telescópico

Estás revisando el código de un compañero y te encontrás con esta línea. **Solo con esta línea**, decime qué pizza pidió:

```typescript
const p = new Pizza("a la piedra", "tomate", ["muzzarella"], "grande", true, false, false);
```

La masa, la salsa, los ingredientes y el tamaño los leés sin problema. **Pero el `true` y los dos `false` no te dicen nada**: para saber qué son tenés que ir a abrir la clase `Pizza`. Ese viaje de ida y vuelta, en cada línea que leas, es el primer costo.

Ahora sí, la definición:

```typescript
constructor(masa, salsa, ingredientes, tamanio,
            bordeRelleno: boolean, extraQueso: boolean, sinTACC: boolean) {}
```

- Ahora que sabés qué es cada uno: si yo **me equivoco** y escribo `false, true, false` en vez de `true, false, false`, el borde deja de estar relleno y aparece extra queso. **¿El compilador me avisa?** Los tres son `boolean`…
- Si querés una pizza **solo con masa integral** y nada más, ¿qué escribís en los otros 6 lugares? ¿Podés **saltear** los que no te importan?
- Mañana agregan la opción "sin sal". ¿Qué les pasa a **todas** las llamadas a `new Pizza(...)` que ya existen desparramadas por el sistema?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> Esto tiene nombre: **constructor telescópico**. Cada opción nueva le suma un parámetro; el orden se vuelve imposible de recordar, el compilador no te protege (todos son del mismo tipo) y no podés omitir lo que no usás. Guardá esa incomodidad: **ese** es el olor que cura Builder.

---

## Builder — señal delatora

Sospechá Builder cuando…

- El objeto tiene **muchos atributos opcionales** y verías un constructor con un montón de parámetros (o cinco constructores sobrecargados).
- La **construcción tiene pasos** y querés controlarlos: *"armar el objeto paso a paso"*.
- Querés **el mismo proceso** de armado produciendo **representaciones distintas** (una pizza napolitana vs. una fugazzeta salen del mismo builder).
- Palabras que lo delatan: **paso a paso**, **opcional**, **configurable**, **con o sin**, **armar por partes**.

---

## Builder — el código

```typescript
class Pizza {
  masa = "clásica";
  salsa = "tomate";
  tamanio = "mediana";
  bordeRelleno = false;
  ingredientes: string[] = [];

  descripcion(): string {
    const borde = this.bordeRelleno ? ", con borde relleno" : "";
    return `Pizza ${this.tamanio} de masa ${this.masa}${borde} — ${this.ingredientes.join(", ")}`;
  }
}

class PizzaBuilder {
  private pizza = new Pizza();   // la pizza a medio armar vive acá adentro

  // Cada paso configura UNA cosa y devuelve el propio builder (return this).
  // Devolver el builder es lo que después nos va a dejar encadenar las llamadas.
  conMasa(m: string): PizzaBuilder    { this.pizza.masa = m;             return this; }
  conSalsa(s: string): PizzaBuilder   { this.pizza.salsa = s;            return this; }
  tamanio(t: string): PizzaBuilder    { this.pizza.tamanio = t;          return this; }
  conBordeRelleno(): PizzaBuilder     { this.pizza.bordeRelleno = true;  return this; }
  agregar(i: string): PizzaBuilder    { this.pizza.ingredientes.push(i); return this; }

  construir(): Pizza { return this.pizza; }   // ← el único que NO devuelve el builder:
}                                            //   devuelve la pizza ya terminada
```

**Y ahora lo uso.** Voy pidiendo lo que quiero, un método por vez:

```typescript
const b = new PizzaBuilder();
b.tamanio("grande");
b.conMasa("a la piedra");
b.conBordeRelleno();
b.agregar("muzzarella");
const p = b.construir();

console.log(p.descripcion());
// Pizza grande de masa a la piedra, con borde relleno — muzzarella
```

Comparalo con `new Pizza("a la piedra", "tomate", ["muzzarella"], "grande", true, false, false)`: acá **cada valor viene con el nombre pegado** (`conBordeRelleno()` no se confunde con nada), **el orden no importa** y **lo que no te interesa, no lo escribís**.

---

## Builder — por qué cada paso hace `return this`

¿Para qué devuelve el builder cada método, si el objeto `b` ya lo tenía? **Para poder encadenar.** Como `b.tamanio("grande")` **devuelve el mismo `b`**, podés escribirle un punto y seguir llamando métodos sobre el resultado:

```typescript
const p = new PizzaBuilder()
  .tamanio("grande")        // devuelve el builder...
  .conMasa("a la piedra")   // ...entonces le puedo pedir otra cosa acá
  .conBordeRelleno()        // ...y otra
  .agregar("muzzarella")    // ...y otra
  .construir();             // ← este corta la cadena: devuelve la Pizza
```

Es **exactamente lo mismo** que las 5 líneas de la slide anterior, pero sin repetir `b.` cada vez. Si los métodos devolvieran `void`, la cadena se cortaría en el primer punto.

> Esto se llama **interfaz fluida** (*fluent interface*) y lo vas a ver por todos lados. Ojo: es una **comodidad de lectura**, no es el patrón. Un Builder cuyos métodos devuelvan `void` **sigue siendo un Builder** — lo que lo define es *construir el objeto por partes y entregarlo terminado al final*, no el encadenamiento.

---

## Builder — el Director (opcional)

La GoF define además un **Director**: una clase que conoce **recetas fijas** y le dicta los pasos al builder. Sirve cuando **el mismo armado se repite** en muchos lados.

```typescript
class Pizzeria {                                   // ← el Director
  napolitana(b: PizzaBuilder): Pizza {
    return b.tamanio("grande").conMasa("clásica")
            .agregar("muzzarella").agregar("tomate").agregar("ajo")
            .construir();
  }
}

const napo = new Pizzeria().napolitana(new PizzaBuilder());
```

El **Director** sabe *en qué orden* llamar a los pasos; el **Builder** sabe *cómo se hace* cada paso. Si tu receta es única y no se repite, el Director es opcional: no lo metas por metelo.

**Trade-off (cátedra):** Builder **aumenta la cantidad de clases** del sistema. Si tu objeto tiene 3 atributos y ninguno opcional, **no uses Builder**: usá el constructor y listo.

---

## Builder — UML

> [IMAGEN: diagrama de clases UML del patrón Builder. A la derecha, la clase producto "Pizza" con los atributos masa, salsa, tamanio, bordeRelleno, ingredientes y el método descripcion(). En el centro, la clase "PizzaBuilder" con un atributo privado "- pizza: Pizza" y los métodos encadenables conMasa(m), conSalsa(s), tamanio(t), conBordeRelleno(), agregar(i) y construir(): Pizza. De "PizzaBuilder" sale una flecha de composición (rombo relleno) hacia "Pizza", y también una flecha de dependencia punteada etiquetada «create». A la izquierda, la clase "Pizzeria" (el Director, dibujada con línea de trazos o marcada con la etiqueta "opcional") con una flecha de asociación hacia "PizzaBuilder", etiquetada "le dicta los pasos". Una nota sobre los métodos del builder dice: "todos devuelven el propio builder → por eso se encadenan". Otra nota sobre construir() dice: "el único método que devuelve el producto terminado".]

**Conexión SOLID:** SRP (el código de construcción, que puede ser complejo, sale de la clase del producto y vive en el builder).

---

## Checkpoint — los 4 creacionales

Ya los tenés todos. Antes de pasar a estructurales, **decilo vos**: ¿cuál elegís en cada caso y por qué **no** el parecido?

1. *"Necesitamos una única conexión al servidor de la cátedra, compartida por todo el sistema."*
2. *"Según el tipo de archivo, hay que crear un objeto Documento distinto; el flujo de apertura es siempre igual."*
3. *"Un usuario se registra con nombre y mail obligatorios, y opcionalmente foto, bio, teléfono, fecha de nacimiento y preferencias de notificación."*
4. *"La app tiene tema claro y tema oscuro. Cada tema define su propio botón, su propio panel y su propio ícono, y no se pueden mezclar."*

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **1) Singleton** (una única instancia) · **2) Factory Method** (UN producto, varias variantes concretas) · **3) Builder** (muchos atributos **opcionales** → constructor telescópico) · **4) Abstract Factory** (una **familia** de productos que varían en bloque).
>
> El desempate creacional en una línea: **¿uno solo?** → Singleton. **¿un producto, varias variantes?** → Factory Method. **¿varios productos coherentes entre sí?** → Abstract Factory. **¿un producto con muchas partes opcionales?** → Builder.

---

## Estructurales — Composite

**Familia:** estructural.

**Intención:** componer objetos en **estructuras de árbol** para representar jerarquías **todo-parte**, y lograr que el cliente trate **de la misma manera** a un objeto individual (**hoja**) y a un grupo de objetos (**rama**).

La magia está ahí: **el cliente no pregunta si es hoja o rama**. Le habla igual a los dos.

> Analogía: una **caja de mudanza**. Adentro puede haber cosas sueltas (un libro, una taza) **o más cajas**, que adentro tienen más cosas o más cajas. Si te preguntan *"¿cuánto pesa esa caja?"*, la respuesta es la misma operación sin importar la profundidad: **sumar el peso de lo que hay adentro**.

---

## Razoná — el árbol y el `if`

Un sistema de archivos: una carpeta contiene archivos **y otras carpetas**. Querés calcular el **tamaño total** de una carpeta. La solución "a mano":

```typescript
function tamanio(nodo: any): number {
  if (nodo instanceof Archivo) return nodo.kb;
  if (nodo instanceof Carpeta) {
    let total = 0;
    for (const hijo of nodo.hijos) total += tamanio(hijo);   // ← ojo acá
    return total;
  }
  return 0;
}
```

- ¿Qué pasa si mañana aparece un tercer tipo de nodo (un **acceso directo**)? ¿Cuántos `if` como este hay en el sistema (uno para el tamaño, otro para listar, otro para buscar…)?
- Ese `instanceof`, ¿qué principio de la Clase 2 te está gritando que estás violando?
- Y ahora la pregunta importante: **¿y si `tamanio()` fuera un método que tienen tanto el archivo como la carpeta?** ¿Qué haría la versión de la carpeta?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> La cura: que hoja y rama **compartan la interfaz**. La rama implementa la operación **delegando en sus hijos** —y como los hijos también son de la misma interfaz, la recursión sale sola—. El `if` desaparece: **el polimorfismo lo reemplaza**.

---

## Composite — señal delatora

Sospechá Composite cuando…

- Aparece una estructura **de árbol** o **jerárquica**: menú con submenús, carpetas con carpetas, comentarios con respuestas, organigrama, categorías con subcategorías.
- El enunciado usa palabras como **"contiene"**, **"agrupa"**, **"subgrupo"**, **"a cualquier nivel de profundidad"**, **"un ítem o un conjunto de ítems"**.
- Querés hacer **la misma operación sobre uno o sobre muchos** sin distinguirlos.

> La cátedra marca un detalle fino: Composite tiene sentido **cuando el comportamiento de la rama y el de la hoja son distintos** (la rama tiene que recorrer a sus hijos). Si fueran iguales, alcanzaría con una clase que tenga una lista adentro — no hace falta el patrón.

---

## Composite — el código

```typescript
// Component: la interfaz común. La hoja y la rama la implementan IGUAL.
interface NodoFS {
  nombre(): string;
  tamanio(): number;
}

// Leaf (hoja): no tiene hijos. Sabe su tamaño y listo.
class Archivo implements NodoFS {
  constructor(private _nombre: string, private kb: number) {}
  nombre(): string  { return this._nombre; }
  tamanio(): number { return this.kb; }
}

// Composite (rama): tiene hijos, y resuelve la operación DELEGANDO en ellos.
class Carpeta implements NodoFS {
  private hijos: NodoFS[] = [];
  constructor(private _nombre: string) {}

  agregar(n: NodoFS): void { this.hijos.push(n); }
  quitar(n: NodoFS): void  { this.hijos = this.hijos.filter(h => h !== n); }

  nombre(): string { return this._nombre; }

  tamanio(): number {
    // mismo método que la hoja, pero se apoya en los hijos → recursión natural
    return this.hijos.reduce((total, hijo) => total + hijo.tamanio(), 0);
  }
}

const raiz = new Carpeta("proyecto");
const src  = new Carpeta("src");
src.agregar(new Archivo("main.ts", 12));
src.agregar(new Archivo("utils.ts", 8));
raiz.agregar(src);
raiz.agregar(new Archivo("README.md", 3));

// El cliente NO pregunta si es archivo o carpeta. Le habla igual a los dos:
const cosas: NodoFS[] = [raiz, new Archivo("suelto.txt", 1)];
cosas.forEach(c => console.log(c.nombre(), c.tamanio()));
// proyecto 23      ← 12 + 8 + 3, recursivo, sin un solo if
// suelto.txt 1
```

---

## Composite — UML y su trade-off

> [IMAGEN: diagrama de clases UML del patrón Composite. Arriba, centrada, una interfaz «interface» "NodoFS" con los métodos nombre() y tamanio(). Debajo, dos clases conectadas a ella por realización (línea punteada con triángulo vacío): a la izquierda "Archivo" (la HOJA) con el atributo privado -kb y los métodos nombre() y tamanio(); a la derecha "Carpeta" (la RAMA) con un atributo -hijos y los métodos agregar(n), quitar(n), nombre() y tamanio(). LA RELACIÓN CLAVE: desde "Carpeta" sale una flecha de agregación (rombo VACÍO) que sube y vuelve a apuntar a la interfaz "NodoFS", etiquetada "hijos *" — es una relación recursiva, y hay que dibujarla bien visible porque es el corazón del patrón. Una nota junto a esa flecha dice: "una Carpeta contiene NodoFS: pueden ser Archivos O más Carpetas, a cualquier profundidad". A la izquierda, una caja "Client" con una flecha de dependencia punteada hacia la interfaz "NodoFS" y una nota que dice: "el cliente le habla a la interfaz: nunca pregunta si es hoja o rama".]

**Conexión SOLID:** OCP (sumás un tipo de nodo sin tocar al cliente) y LSP (hoja y rama son sustituibles entre sí).

> **El trade-off que marca la cátedra:** si ponés `agregar()` / `quitar()` **en la interfaz común**, las **hojas quedan obligadas a implementar métodos que no tienen sentido** para ellas (¿qué le agregás a un archivo?). Eso **viola ISP**. La alternativa —dejarlos solo en `Carpeta`, como hicimos arriba— salva ISP, pero obliga al cliente a hacer un `instanceof` si quiere agregar hijos. **No hay salida gratis**: es una decisión de diseño, y saber explicar el intercambio vale más que memorizar una versión.

---

## Recap — los 4 de hoy

| Patrón | Familia | Señal delatora (una línea) |
|---|---|---|
| **Singleton** | Creacional | *"Una **única** instancia"*, accesible desde todo el sistema |
| **Abstract Factory** | Creacional | Una **familia** de productos que varían **juntos** y no se mezclan |
| **Builder** | Creacional | Objeto con **muchas partes opcionales**; armarlo **paso a paso** |
| **Composite** | Estructural | **Árbol** todo-parte; tratar **igual** a la hoja y a la rama |

> Con los tres de arriba, la familia **creacional** te queda **completa**: Singleton, Factory Method, Abstract Factory y Builder. Y el par que **cae seguro** en el parcial ya lo trabajaste: **Factory Method vs Abstract Factory** (uno vs. familia).

---

## Ejercicio en vivo — cómo lo jugamos

Un caso, **sin escribir nada**: ni código ni diagrama. Todo hablado. Te pido tres respuestas en voz alta:

1. **¿De qué se queja el problema?** ¿Crear, estructurar o comunicar? → la **familia**.
2. **¿Qué patrón?** Y **qué palabra exacta** del enunciado te lo delató.
3. **¿Por qué no el parecido?** Te digo yo cuál es el parecido. Esta es la respuesta que vale.

> Si el 3 no te sale, el 2 no cuenta. En el parcial no alcanza con acertar el nombre: hay que saber **descartar** el que se le parece.

---

## Ejercicio en vivo — La inmobiliaria

**El caso.** El portal se despliega para dos marcas: *Premium* y *Económica*. En Premium, la ficha de propiedad se muestra con galería de fotos grande, el mapa es interactivo y el contacto es por teléfono directo. En Económica, la ficha es una lista simple, el mapa es una imagen estática y el contacto es un formulario. La regla del negocio: **nunca** puede aparecer una ficha Premium con un mapa estático. Cada marca es **un paquete completo o nada**.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **El parecido a descartar: Factory Method.** Los dos evitan el `new` cableado y fabrican objetos…

---

## Ejercicio en vivo — La solución

**→ Abstract Factory.** *Crear.* Hay **tres productos** (ficha, mapa, contacto) que varían **en bloque** según la marca, y el enunciado **exige coherencia**: *"nunca una ficha Premium con un mapa estático"*, *"un paquete completo o nada"*. Esa imposibilidad de mezclar es **la firma** del patrón.

> **¿Por qué no Factory Method?** Porque Factory Method fabrica **un** producto con variantes. Acá son **varios productos distintos** que tienen que ser **consistentes entre sí**. Contá los tipos de producto en el enunciado: si es **uno**, es Factory Method; si son **varios que viajan juntos**, es Abstract Factory. (De hecho, la Abstract Factory está hecha **por dentro** de varios Factory Methods.)

---

## Cierre

Hoy sumaste **4**: **Singleton, Abstract Factory, Builder** y **Composite**. Con eso vas **9 de 13**, y la familia **creacional entera**.

- Confirmamos que todos salen de las mismas dos herramientas: **programar contra una interfaz** y **composición**. Composite lo lleva al extremo: se compone **a sí mismo**, y de ahí sale la recursión.
- Y viste que ningún patrón es gratis: **Singleton viola SRP y DIP** y **Composite tensiona ISP**. Saber explicar el **trade-off** vale más que recitar la definición.

> Lo que **no** hay que llevarse: la idea de que hay que meter patrones. La cátedra abre la unidad advirtiéndolo: *"su mera existencia no implica su uso"*. Un `if` simple bien puesto le gana a un patrón forzado.

**Próxima clase:** los **4 que faltan** —**Facade, Chain of Responsibility, Template Method y State**— y con eso cerramos el catálogo. Después, **práctica en vivo**: leer enunciados tipo parcial, reconocer y justificar.

---

## Después de clase — ¿Cuál elijo? (los 9 que ya viste)

La tabla para el parcial. Lo que importa no es la definición: es **la pregunta que delata a cada uno**.

| Patrón | Familia | La pregunta que lo delata |
|---|---|---|
| **Singleton** | Creacional | ¿Tiene que haber **una sola** instancia en todo el sistema? |
| **Factory Method** | Creacional | ¿El problema es **qué clase concreta instanciar** (un producto)? |
| **Abstract Factory** | Creacional | ¿Hay **varios productos** que varían **en bloque** y no deben mezclarse? |
| **Builder** | Creacional | ¿El objeto tiene **muchas partes opcionales** y hay que armarlo **paso a paso**? |
| **Adapter** | Estructural | ¿Dos interfaces **no encajan** y una no la controlo? |
| **Decorator** | Estructural | ¿Quiero **sumar comportamientos combinables** sin tocar la clase? |
| **Composite** | Estructural | ¿Hay un **árbol** todo-parte y quiero tratar igual a la hoja y a la rama? |
| **Strategy** | Comportamiento | ¿Quiero elegir **cómo hago algo**, intercambiable en runtime? |
| **Observer** | Comportamiento | ¿Un cambio debe **avisarle a varios** que se suscriben y se van? |

**Los desempates que ya podés hacer:**
- **Factory Method vs Abstract Factory** → **un** producto vs. una **familia** de productos.
- **Abstract Factory vs Builder** → **varios productos** que tienen que ser coherentes entre sí vs. **un** producto con muchas **partes opcionales**.
- **Composite vs Decorator** → los dos se componen **de su propia interfaz**, pero Composite arma un **árbol** (muchos hijos, para tratar igual al todo y a la parte) y Decorator arma una **pila** (un solo envuelto, para sumarle comportamiento).
- **Adapter vs Decorator** → Adapter **cambia** la interfaz (traduce) y no agrega nada; Decorator la **mantiene** y suma.

> Los **4 patrones que faltan** —y sus desempates— llegan la clase que viene.

---

## Después de clase — Ejercicios

Para cada uno: **1)** ¿el problema es de *crear*, *estructurar* o *comunicar*? **2)** ¿qué patrón, y **por qué no el parecido**? **3)** bosquejá el UML y un esqueleto en TypeScript.

Ojo: **hay dos enunciados cuya respuesta es un patrón de la Clase 3.** No todo lo de hoy es lo de hoy.

**1.** El sistema de logs de la empresa escribe en un archivo. Si dos partes del programa crean cada una su propio objeto logger, el archivo se corrompe y se pierden líneas. Necesitás que **exista un solo logger** en toda la aplicación y que cualquier módulo pueda conseguirlo sin recibirlo por parámetro desde diez niveles arriba.

**2.** Una app de e-commerce se despliega para dos clientes: "Retail" y "Mayorista". En Retail, el cálculo de precio muestra IVA incluido, el carrito permite hasta 10 unidades y el mail de confirmación es informal. En Mayorista, el precio va sin IVA, el carrito no tiene tope y el mail es formal con datos fiscales. **Nunca** puede salir un mail informal con precios sin IVA: la configuración es **coherente o nada**.

**3.** El formulario de alta de una propiedad en un portal inmobiliario tiene: dirección y precio (obligatorios) y, opcionales, metros cubiertos, metros totales, ambientes, cocheras, si acepta mascotas, si tiene amenities, fotos y descripción. Hoy hay un constructor con 10 parámetros y nadie se acuerda del orden.

**4.** Un sistema de gestión de una consultora tiene empleados y equipos. Un **equipo** puede tener empleados **y otros equipos** adentro (subequipos), sin límite de profundidad. Hay que poder calcular el **costo salarial total** de cualquier cosa: de un empleado suelto, de un equipo, o de la empresa entera.

**5.** Tu módulo de reportes consume datos con `FuenteDatos.obtener(): Registro[]`. Un cliente nuevo expone su información con un servicio SOAP que tiene el método `fetchAll()` y devuelve XML con otros nombres de campo. No podés modificar el servicio del cliente ni querés reescribir el módulo de reportes.

**6.** Una app de notas permite exportar una nota. La exportación puede ir **cifrada**, **comprimida**, **con marca de agua**, o **cualquier combinación de esas tres**, y el usuario elige la combinación **en el momento de exportar** con unos checkboxes. La clase `Nota` ya existe y funciona; no la querés tocar.

---

## Después de clase — Soluciones

**1 → Singleton.** *Crear:* el enunciado dice literalmente **"un solo logger en toda la aplicación"** y **"conseguirlo sin recibirlo por parámetro"** — las dos mitades del patrón (instancia única + punto de acceso global). Constructor privado, `private static instancia`, `static getInstancia()`.
> Pero decilo también en el parcial: esto **viola SRP** (la clase guarda logs *y* administra su propia instancia) y **DIP** (quien lo usa no declara que depende de él). Si el logger pudiera **pasarse por constructor**, sería mejor diseño. Singleton se justifica acá **solo** porque el enunciado exige explícitamente el acceso global desde cualquier módulo.

**2 → Abstract Factory.** *Crear:* hay **tres productos** (cálculo de precio, política de carrito, generador de mail) que varían **en bloque** según el cliente, y el enunciado exige **coherencia** (*"nunca un mail informal con precios sin IVA"*). Eso es exactamente lo que garantiza una fábrica de familias.
> *No es Factory Method:* ahí habría **un solo** producto variando. Acá son varios que **no se pueden mezclar** — y esa imposibilidad de mezclar es la firma de Abstract Factory.

**3 → Builder.** *Crear:* **constructor telescópico** de manual (2 obligatorios + 8 opcionales). `new PropiedadBuilder().enDireccion(...).conPrecio(...).conAmbientes(3).aceptaMascotas().construir()`. Se lee, y lo que no aplica no se escribe.
> *No es Abstract Factory:* acá hay **un solo** producto (la propiedad) con muchas **partes opcionales**, no varios productos que tengan que ser coherentes entre sí.

**4 → Composite.** *Estructurar:* jerarquía **todo-parte** (un equipo contiene empleados **y equipos**), **profundidad arbitraria**, y la misma operación —`costoSalarial()`— sobre la hoja y sobre la rama. `interface Costeable { costoSalarial(): number }`; `Empleado` (hoja) devuelve su sueldo; `Equipo` (rama) tiene `miembros: Costeable[]` y **suma los de sus hijos** → la recursión sale sola, sin un solo `if`.
> *No es Decorator:* los dos se componen de su propia interfaz, pero Decorator envuelve **un** objeto para **sumarle comportamiento**; acá `Equipo` contiene **muchos** hijos y no le agrega nada a ninguno: los **agrupa** para poder tratar igual al todo y a la parte. Composite arma un **árbol**; Decorator, una **pila**.

**5 → Adapter** *(¡de la Clase 3!)*. *Estructurar:* dos interfaces **incompatibles** (`obtener()` vs `fetchAll()`, `Registro[]` vs XML) y una que **no controlás**. `class SoapAdapter implements FuenteDatos` compone el servicio y traduce `obtener()` → `fetchAll()` + parsea el XML al formato propio.
> *No es Decorator:* Decorator **mantiene** la interfaz del objeto que envuelve y le **suma** comportamiento. Acá no sumás nada: la interfaz del SOAP **no encaja** con la que tu módulo ya habla, y tu único trabajo es **traducirla**. Adapter **cambia** la interfaz; Decorator la **conserva**.

**6 → Decorator** *(¡de la Clase 3!)*. *Estructurar:* funcionalidades **opcionales y combinables** (2³ = 8 combinaciones), elegidas **en runtime**, sobre una clase que **no querés tocar**. `interface Exportable`, `abstract class ExportDecorator implements Exportable` que **contiene** un `Exportable`; `Cifrado`, `Comprimido`, `ConMarcaDeAgua` se apilan: `new Cifrado(new Comprimido(new ExportadorBase(nota)))`.
> *No es Strategy:* en Strategy hay **un solo** algoritmo activo por vez y el cliente lo **reemplaza**. Acá las tres funcionalidades pueden estar activas **a la vez**, apiladas una sobre otra. Strategy **intercambia**; Decorator **acumula**.

---

## Después de clase — Lectura recomendada

- **Refactoring Guru — Patrones de diseño** (en español): `refactoring.guru/es/design-patterns`. Leé las fichas de **los 4 de hoy**: Singleton, Abstract Factory, Builder y Composite. La sección **"Relaciones con otros patrones"** de cada ficha es oro puro para los desempates.
- **PDF de la cátedra, Unidad 4.** Prestale atención al **cuadro de ventajas y desventajas del Singleton**: está explícito ahí y cae en el parcial.
- Volvé sobre la **Clase 2 (SOLID)** con los patrones de hoy en la mano: fijate cómo **Singleton viola SRP y DIP**, cómo **Composite tensiona ISP**, y cómo **Abstract Factory es OCP y DIP hechos patrón**.

> Para la próxima: llegá pudiendo decir **los 9 que ya viste, en una línea cada uno** (intención + señal delatora). Arrancamos con los **4 que faltan** y después nos vamos derecho a **reconocer enunciados**: te voy a dar casos y vas a tener que decidir vos.
