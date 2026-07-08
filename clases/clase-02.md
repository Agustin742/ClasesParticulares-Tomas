# Clase 2 — Principios S.O.L.I.D.

> **Unidad:** 2 (Principios SOLID) · **Lenguaje de ejemplos:** TypeScript

---

## Clase 2 — Principios S.O.L.I.D.

**Programación II · Unidad 2**

> *"Cualquiera puede escribir código que una computadora entienda. Los buenos programadores escriben código que los humanos pueden entender."* — Martin Fowler

En la Clase 1 vimos **cómo** se arma la POO (clases, herencia, interfaces). Hoy vemos **cómo se arma bien**: 5 principios para que el código no se vuelva un plato de fideos.

---

## ¿Qué es SOLID?

Son **5 principios de diseño** para escribir clases más mantenibles. Los enunció **Robert C. Martin ("Uncle Bob")** a principios de los 2000; **Michael Feathers** les dio el acrónimo **SOLID**.

- **S** — Single Responsibility (Responsabilidad única)
- **O** — Open/Closed (Abierto/Cerrado)
- **L** — Liskov Substitution (Sustitución de Liskov)
- **I** — Interface Segregation (Segregación de interfaces)
- **D** — Dependency Inversion (Inversión de dependencias)

> No son reglas mágicas ni obligatorias: son **guías**. La meta de todas es la misma → **bajar el acoplamiento** y que un cambio no rompa media aplicación.

---

## ¿Por qué me deberían importar?

Código que **no** sigue estos principios suele tener tres olores:

- **Rígido** — para un cambio chico tengo que tocar diez lugares.
- **Frágil** — toco acá y se rompe algo allá, sin relación aparente.
- **Inmóvil** — no puedo reutilizar una parte sin arrastrar todo lo demás.

SOLID ataca justamente eso. Los beneficios que busca:

- **Mantenibilidad** — encontrar y arreglar bugs sin efectos colaterales.
- **Escalabilidad** — agregar features sin reescribir lo que ya andaba.
- **Reusabilidad** — piezas chicas e independientes que puedo combinar.

> Regla mental para toda la clase: **una clase debería tener una sola razón para cambiar.**

---

## Repaso relámpago — polimorfismo

**Mismo llamado, distinto comportamiento.** Objetos diferentes responden **al mismo método**, cada uno a su manera. Ya lo viste en la Clase 1, pero hoy es **la herramienta que resuelve casi todo SOLID**, así que lo dejamos bien fresco.

Dos formas de lograrlo en TypeScript:
- **Por herencia** → una subclase **sobreescribe** un método del padre.
- **Por interfaces** → clases de naturaleza distinta implementan el mismo contrato.

La clave: el código que **llama** al método no necesita saber *quién* es el objeto concreto. Le habla a la **abstracción** y cada objeto responde solo.

---

## Polimorfismo — un ejemplo concreto

```typescript
interface Pago {
  pagar(monto: number): void;
}

class Tarjeta implements Pago {
  pagar(monto: number): void { console.log(`Pago $${monto} con tarjeta`); }
}
class Efectivo implements Pago {
  pagar(monto: number): void { console.log(`Pago $${monto} en efectivo`); }
}
class Transferencia implements Pago {
  pagar(monto: number): void { console.log(`Transfiero $${monto}`); }
}

// El cliente le habla a la ABSTRACCIÓN Pago, no a las clases concretas:
function cobrar(medio: Pago, monto: number): void {
  medio.pagar(monto);   // cada objeto responde a su manera
}

cobrar(new Tarjeta(), 1000);   // Pago $1000 con tarjeta
cobrar(new Efectivo(), 500);   // Pago $500 en efectivo
```

Fijate que `cobrar()` **no tiene ni un `if`** sobre el tipo de pago. Agregar `Cripto` es sumar una clase que implemente `Pago` — **sin tocar `cobrar()`**. 👈 acordate de esta sensación.

> [IMAGEN: una caja central "cobrar(medio: Pago)" con una flecha saliente etiquetada "medio.pagar()". Esa flecha llega a una interfaz «interface» "Pago". De la interfaz bajan tres clases conectadas por realización (línea punteada, triángulo vacío): "Tarjeta", "Efectivo" y "Transferencia", cada una con una viñeta de cómo responde ("con tarjeta", "en efectivo", "transferencia"). Un globo de texto sobre cobrar() dice: "no sabe cuál es, solo sabe que es un Pago".]

---

## Por qué es EL hilo de esta clase

El truco de arriba —**hablarle a una abstracción y dejar que cada objeto responda solo**— es lo que vas a ver reaparecer en casi todos los principios:

- **O** (Abierto/Cerrado) → agregar un `Pago` nuevo **sin tocar** el código que ya anda.
- **L** (Liskov) → poder reemplazar `Pago` por **cualquier** implementación sin que se rompa.
- **I** (Segregación) → conviene que esas interfaces sean **chicas y específicas**.
- **D** (Inversión) → `cobrar()` depende de `Pago` (abstracción), **no** de `Tarjeta` (concreto).

> Si entendés bien polimorfismo, SOLID deja de ser 5 reglas sueltas y pasa a ser **5 usos del mismo martillo**. Tené este ejemplo en la cabeza toda la clase.

---

## Antes de arrancar — calentemos

Mirá esta clase (todavía **sin** saber ningún principio):

```typescript
class Usuario {
  guardarEnBaseDeDatos(): void { /* ... */ }
  validarEmail(): boolean { /* ... */ return true; }
  enviarMailBienvenida(): void { /* ... */ }
  renderizarPerfilHTML(): string { /* ... */ return ""; }
}
```

- ¿Te **suena bien** o algo te hace ruido? ¿Por qué?
- Si el equipo de diseño cambia el HTML del perfil, **¿qué clase tocás?** ¿Te gusta que sea *esa*?
- Contá cuántas "tareas distintas" hace `Usuario`.

> *Contestá en voz alta antes de pasar de slide. Todo lo que sientas acá lo vamos a poder nombrar con SOLID.*

---

## S — Responsabilidad Única (SRP)

**Una clase debe tener una sola responsabilidad → una sola razón para cambiar.**

Si una clase hace dos cosas distintas (ej: manejar datos **y** imprimirlos), tiene **dos motivos** para que la vengan a modificar. Y cada motivo es una chance de romper el otro.

**Beneficios:**
- Menos casos de prueba (la clase hace menos).
- Menos dependencias → menos acoplamiento.
- Más fácil de leer y de nombrar (si para nombrarla necesitás un "y", sospechá).

---

## SRP — Antes

La clase `Book` maneja el contenido del libro… **pero también lo imprime**.

```typescript
class Book {
  private name: string;
  private author: string;
  private text: string;

  // constructor, getters y setters

  replaceWordInText(word: string): string {
    return this.text.replace(word, this.text);
  }

  isWordInText(word: string): boolean {
    return this.text.includes(word);
  }

  printTextToConsole(): void {
    // imprime el texto por consola
  }
}
```

**El problema:** `Book` manipula el contenido *y* se ocupa de mostrarlo. ¿Qué pasa si mañana hay que imprimir a un archivo, o cambiar el formato? Toco `Book`, una clase que en teoría es "el libro".

---

## Pará y pensá — ¿dónde cortás?

Antes de ver mi solución, resolvelo vos:

- Listá las **razones de cambio** de `Book`. ¿Son una o más de una?
- Si tuvieras que partirla en dos clases, **¿por qué línea la cortás?** ¿Qué método se va y cuál se queda?
- ¿Cómo llamarías a la clase nueva?

---

## SRP — Después 

Separo las dos responsabilidades en **dos clases**:

```typescript
class Book {
  private name: string;
  private author: string;
  private text: string;

  replaceWordInText(word: string): string {
    return this.text.replace(word, this.text);
  }

  isWordInText(word: string): boolean {
    return this.text.includes(word);
  }
}

class BookPrinter {
  printTextToConsole(text: string): void {
    // imprime por consola
  }

  printTextToAnotherMedium(text: string): void {
    // imprime en otro dispositivo
  }
}
```

Ahora `Book` cambia **solo** si cambia la lógica del libro; `BookPrinter` cambia **solo** si cambia la forma de imprimir.

> [IMAGEN: comparación lado a lado tipo "antes / después". A la izquierda, una sola caja de clase UML titulada "Book" con los atributos privados name, author, text y una lista de métodos que incluye replaceWordInText, isWordInText y printTextToConsole; sobre la caja una etiqueta roja "2 responsabilidades". A la derecha, dos cajas separadas: una "Book" (con name, author, text, replaceWordInText, isWordInText) y otra "BookPrinter" (con printTextToConsole, printTextToAnotherMedium); cada caja con una etiqueta verde "1 responsabilidad". Una flecha grande en el medio apunta de la versión izquierda a la derecha con el texto "separar".]

---

## SRP — Olor a código

Sospechá que se viola SRP cuando…

- Para **describir** la clase usás un "**y**": *"maneja el usuario **y** manda mails"*.
- La clase mezcla niveles distintos: **lógica de negocio** + **persistencia** + **presentación** (UI, logging, formateo).
- Un método toca cosas que no tienen nada que ver entre sí.

**Ejemplos de responsabilidades que conviene separar:**
- Calcular algo ≠ guardarlo en base de datos.
- Un `Empleado` (datos) ≠ un `CalculadorDeSueldo` ≠ un `ReporteDeSueldo`.

> Ojo con exagerar: "una responsabilidad" no es "un solo método". Es **un solo motivo de cambio**.

---

## O — Abierto/Cerrado (OCP)

**Una clase debe estar abierta a la extensión, pero cerrada a la modificación.**

Traducido: para agregar un comportamiento nuevo, **no debería tener que editar código que ya funciona** (y ya está testeado). Agrego código nuevo, no toco el viejo.

> La única excepción sana para modificar código existente es **corregir un bug**.

**La herramienta clave:** interfaces (o clases abstractas) + polimorfismo.

---

## OCP — Antes

Seguimos con el libro: ahora hay que **guardarlo**. Empezamos guardando a un archivo:

```typescript
class BookPersistence {
  saveTextToFile(book: Book): void {
    // guarda el texto del libro en un archivo
  }
}
```

Al tiempo piden **también** guardar en base de datos. La tentación es agregar un método más (o un `if`):

```typescript
class BookPersistence {
  save(book: Book, destino: string): void {
    if (destino === "archivo") { /* ... */ }
    else if (destino === "db") { /* ... */ }   // ← toco la clase que ya andaba
  }
}
```

Cada nuevo destino me obliga a **volver a abrir y modificar** `BookPersistence`. Eso rompe OCP.

---

##  Razonemos — el costo del `if`

Imaginá que ese `if / else if` ya tiene **6 destinos** y entra el séptimo.

- ¿Qué archivo tenés que abrir y editar? ¿Qué podés romper **sin querer** de los otros 6?
- Todo ese código ya estaba **testeado y andando**. ¿Te gusta tener que volver a tocarlo?
- Pista de la Clase 1: cuando distintos objetos tienen que responder distinto **al mismo llamado**… ¿qué herramienta usábamos? 🧩

---

## OCP — Después 

Defino un **contrato** (interfaz) y cada forma de guardar es una implementación aparte:

```typescript
interface BookPersistence {
  save(book: Book): void;
}

class TextFilePersistence implements BookPersistence {
  save(book: Book): void {
    // guarda a un archivo de texto
  }
}

class DbPersistence implements BookPersistence {
  save(book: Book): void {
    // guarda en base de datos
  }
}
```

¿Aparece un nuevo destino (la nube, por ejemplo)? Creo `CloudPersistence implements BookPersistence` y **no toco nada de lo anterior**. El código cliente, que trabaja contra `BookPersistence`, ni se entera.

> [IMAGEN: diagrama de clases UML. Arriba una interfaz "BookPersistence" (con el estereotipo «interface» y el método save(book: Book): void). Debajo, tres cajas de clase — "TextFilePersistence", "DbPersistence" y "CloudPersistence" — cada una conectada a la interfaz con una flecha de realización (línea punteada con triángulo vacío apuntando a la interfaz). La caja "CloudPersistence" está dibujada con borde verde punteado y una etiqueta "nueva: se agrega sin tocar el resto".]

Este es el mismo truco que ya viste en la Clase 1: **programar contra interfaces**. El código se vuelve polimórfico y las implementaciones son intercambiables.

---

## OCP — Olor a código

Sospechá violación de OCP cuando…

- Ves cadenas de **`if / else if`** o **`switch`** sobre un "tipo" y sabés que la lista **va a crecer** (tipos de pago, formatos de export, roles…).
- Cada feature nuevo te obliga a **editar la misma clase** una y otra vez.
- Encontrás comentarios tipo *"si agregás un tipo nuevo, acordate de tocar acá, acá y acá"*.

**La cura casi siempre es la misma:** reemplazar el `switch` por **polimorfismo** (una interfaz + una clase por caso).

---

## L — Sustitución de Liskov (LSP)

**Si B es subclase de A, tengo que poder usar un B en cualquier lado donde se espera un A, sin que la aplicación se rompa.**

Lo formuló **Barbara Liskov** en 1987. En criollo: una subclase **no debe romper las promesas** de su clase padre. Si heredás, tenés que comportarte como lo que heredaste.

> Es la contracara del polimorfismo: solo puedo tratar a los hijos "como el padre" si de verdad **se comportan** como el padre.

---

## LSP — Antes

```typescript
abstract class Animal {
  abstract makeNoise(): void;
}

class Dog extends Animal {
  makeNoise(): void { console.log("guau guau"); }
}

class Cat extends Animal {
  makeNoise(): void { console.log("miau miau"); }
}

class DumbDog extends Dog {
  makeNoise(): void {
    throw new Error("no puedo hacer ruido");   // ← rompe la promesa
  }
}
```

`DumbDog` **es un** `Dog` según el compilador, pero si en el código cliente reemplazo un `Dog` por un `DumbDog`, la app **explota** al llamar `makeNoise()`. Cumple la firma, pero **traiciona el contrato**.

---

## LSP — El caso clásico: Cuadrado / Rectángulo

El ejemplo más famoso de LSP: "un cuadrado **es un** rectángulo", ¿no? En geometría sí; en código, cuidado.

```typescript
class Rectangulo {
  constructor(protected ancho: number, protected alto: number) {}
  setAncho(a: number) { this.ancho = a; }
  setAlto(a: number)  { this.alto = a; }
  area(): number { return this.ancho * this.alto; }
}

class Cuadrado extends Rectangulo {
  setAncho(a: number) { this.ancho = a; this.alto = a; } // fuerza lados iguales
  setAlto(a: number)  { this.ancho = a; this.alto = a; }
}
```

**Predecí antes de mirar la próxima slide.** Un cliente recibe algo declarado como `Rectangulo` y hace:

```typescript
rect.setAncho(5);
rect.setAlto(4);
const a = rect.area();
```

¿Cuánto vale `a`… **si `rect` es un `Rectangulo`**? ¿Y **si por debajo es un `Cuadrado`**? ¿Dan lo mismo? Escribí los dos números antes de seguir.

---

## LSP — El caso clásico: la trampa

- Si el objeto es un `Rectangulo` → `area()` da **20** (5 × 4), justo lo que el cliente espera.
- Si por debajo es un `Cuadrado` → cada `set` iguala los dos lados: queda 4 × 4 = **16**. El cliente pidió ancho 5 y **se lo ignoraron por la espalda**.

El cliente hacía algo **válido para un `Rectangulo`**, y el `Cuadrado` **cambió el comportamiento esperado** → viola LSP.

> Moraleja: "es un" en el lenguaje natural **no** garantiza "es un" en el diseño. Herencia se gana **comportándose igual**, no solo compartiendo nombre.

---

## LSP — Olor a código

Sospechá violación de LSP cuando una subclase…

- **Lanza excepciones** en métodos que en el padre funcionaban.
- **Deja métodos vacíos** o con `// no hace nada` porque "no aplican".
- Necesita que el cliente pregunte **`if (obj instanceof SubclaseX)`** para tratarla distinto.
- **Endurece precondiciones** o **debilita postcondiciones** (pide más / entrega menos que el padre).

> Si te encontrás escribiendo `instanceof` para "arreglar" el comportamiento de un hijo, probablemente la herencia esté mal planteada. A veces la solución es **composición** en vez de herencia.

---

## Checkpoint — vamos por la mitad

Ya vimos **S**, **O** y **L**. explicáme **con tus palabras** (una frase cada uno):

- **S** → una clase, ¿qué…?
- **O** → abierta a… / cerrada a…
- **L** → una subclase no debe…

Y una para pensar: **¿notaste algo que se repite en las soluciones de los tres?** 

> *Spoiler que ya vas viendo venir: casi siempre aparecen **interfaces** y **polimorfismo**. Es el hilo de toda la clase.*

---

## I — Segregación de Interfaces (ISP)

**Ninguna clase debería estar obligada a implementar métodos que no usa.**

Interfaces **grandes** ("gordas", con muchos métodos) → dividirlas en interfaces **chicas y específicas**. Así cada clase implementa **solo** lo que le corresponde.

> Es el SRP de las interfaces: una interfaz también debería tener un propósito acotado.

---

## ISP — Antes

Una sola interfaz que mezcla "imprimir" con "tener papel":

```typescript
interface IPrinter {
  print(text: string): void;
  hasPaper(): boolean;
}

class ConsolePrinter implements IPrinter {
  print(text: string): void { /* imprime por consola */ }

  hasPaper(): boolean {
    throw new Error("una consola no tiene papel");  // ← método que sobra
  }
}
```

`ConsolePrinter` está **forzada** a implementar `hasPaper()`, que no tiene ningún sentido para una consola. Termina tirando un error o devolviendo cualquier cosa.

---

## ISP — Después 

Parto la interfaz gorda en dos contratos chicos:

```typescript
interface IBookPrinter {
  print(text: string): void;
}

interface IPaperBookPrinter {
  hasPaper(): boolean;
}

class ConsolePrinter implements IBookPrinter {
  print(text: string): void { /* imprime por consola */ }
}

class PaperPrinter implements IBookPrinter, IPaperBookPrinter {
  print(text: string): void { /* imprime en papel */ }
  hasPaper(): boolean { return true; }
}
```

La consola implementa **solo** lo que usa. La impresora de papel implementa **las dos** (recordá: en TS una clase puede implementar varias interfaces).

> [IMAGEN: comparación antes/después. A la izquierda una interfaz gorda «interface» "IPrinter" con dos métodos (print y hasPaper) y dos clases "ConsolePrinter" y "PaperPrinter" ambas conectadas con flechas de realización; sobre ConsolePrinter una marca roja de advertencia indicando "obligada a implementar hasPaper() sin sentido". A la derecha, dos interfaces chicas separadas: «interface» "IBookPrinter" (print) e «interface» "IPaperBookPrinter" (hasPaper); "ConsolePrinter" con una sola flecha de realización hacia IBookPrinter, y "PaperPrinter" con dos flechas de realización, una hacia IBookPrinter y otra hacia IPaperBookPrinter.]

---

## ISP — Olor a código

Sospechá violación de ISP cuando…

- Una clase implementa una interfaz pero **deja métodos vacíos**, con `throw new Error("not supported")` o devolviendo `null`.
- Una interfaz tiene **muchísimos métodos** y ninguna clase los usa todos.
- Cambiás un método de una interfaz y **te rompe clases que ni lo usaban**.

> Fijate el parecido con ISP y LSP: los métodos "no soportados" que tiran error aparecen en los dos olores. Muchas veces una interfaz gorda es lo que **empuja** a violar Liskov.

---

## D — Inversión de Dependencias (DIP)

**Las clases deben depender de abstracciones, no de implementaciones concretas.**

Dos enunciados que van juntos:
- Los módulos de **alto nivel** (lógica importante) no deben depender de los de **bajo nivel** (detalles). Ambos dependen de **abstracciones**.
- Las abstracciones no dependen de los detalles; los detalles dependen de las abstracciones.

> La idea: en vez de que mi clase **cree** por dentro lo que necesita (y quede atada a esa clase concreta), que lo **reciba desde afuera** contra una interfaz.

---

## DIP — Antes

`Computer` crea por dentro un teclado y un monitor **concretos**:

```typescript
class Computer {
  private readonly keyboard: StandardKeyboard;
  private readonly monitor: Monitor;

  constructor() {
    this.monitor = new Monitor();          
    this.keyboard = new StandardKeyboard();
  }
}
```

Dos problemas:
1. `Computer` queda **acoplada** a `StandardKeyboard` y `Monitor`.
2. Si quiero un teclado **inalámbrico** o un monitor distinto, **no puedo**: están cableados adentro.

---

## Encontrá la línea culpable

Volvé al código de `Computer` de la slide anterior:

- Señalá **la línea exacta** que ata a `Computer` con un teclado concreto.
- Si mañana querés un `WirelessKeyboard`, ¿podés **sin tocar** `Computer`? ¿Por qué no?
- ¿Y si en vez de **crear** el teclado adentro, `Computer` lo **recibiera** de afuera? ¿Qué cambiaría?

---

## DIP — Después 

Dependo de **interfaces** y las recibo por el constructor:

```typescript
interface Keyboard { /* ... */ }
interface Monitor { /* ... */ }

class Computer {
  private keyboard: Keyboard;
  private monitor: Monitor;

  constructor(keyboard: Keyboard, monitor: Monitor) {
    this.keyboard = keyboard;   // me lo dan de afuera
    this.monitor = monitor;
  }
}

// El que arma la Computer decide QUÉ implementación usar:
const pc = new Computer(new WirelessKeyboard(), new OledMonitor());
```

Ahora `Computer` depende de la **abstracción** `Keyboard`, no de `StandardKeyboard`. Cambiar el teclado es cambiar **quién le paso**, sin tocar `Computer`.

> [IMAGEN: dos diagramas de clases comparados. Arriba ("antes"): una caja "Computer" con dos flechas de dependencia sólidas apuntando directamente a dos cajas concretas "StandardKeyboard" y "Monitor"; etiqueta roja "depende de lo concreto". Abajo ("después"): una caja "Computer" con flechas apuntando a dos interfaces «interface» "Keyboard" y «interface» "Monitor"; de cada interfaz baja una flecha de realización (línea punteada, triángulo vacío) desde clases concretas como "WirelessKeyboard", "StandardKeyboard" y "OledMonitor". Se resalta que las flechas de las clases concretas apuntan HACIA la interfaz: la dependencia quedó "invertida".]

---

## DIP — Olor a código

Sospechá violación de DIP cuando…

- Ves **`new` de clases concretas** dentro de una clase que hace lógica importante (especialmente servicios, repositorios, conexiones).
- Cambiar un detalle de bajo nivel (proveedor de mail, motor de base) te obliga a tocar la lógica de negocio.

> **"Inversión"** = se da vuelta la flecha de dependencia. Sin DIP, `Computer → StandardKeyboard`. Con DIP, `Computer → Keyboard ← StandardKeyboard`: ahora lo concreto depende de la abstracción, no al revés.

---

## Recap — los 5 de un vistazo

- **S** · Responsabilidad única → **una razón para cambiar** por clase.
- **O** · Abierto/Cerrado → **extender sin modificar** (interfaces + polimorfismo).
- **L** · Liskov → **el hijo no rompe las promesas** del padre.
- **I** · Segregación de interfaces → **interfaces chicas**, nada de métodos que sobran.
- **D** · Inversión de dependencias → **depender de abstracciones**, inyectar lo concreto.

> Fijate el hilo conductor: casi todos se resuelven con lo mismo → **interfaces, polimorfismo y bajar el acoplamiento**. SOLID no son 5 ideas sueltas, son 5 caras de "código desacoplado".

---

## 🛠️ Ejercicio en vivo — ¿qué principio se viola?

Para cada caso, decí **qué principio SOLID se rompe** y **cómo lo arreglarías**. Pensalo antes de pasar a la solución.

**Caso 1**

```typescript
class CalculadoraDescuento {
  calcular(tipoCliente: string, monto: number): number {
    if (tipoCliente === "regular") return monto * 0.95;
    if (tipoCliente === "premium") return monto * 0.90;
    if (tipoCliente === "vip")     return monto * 0.80;
    return monto;
  }
}
```

**Caso 2**

```typescript
class Ave {
  volar(): void { console.log("volando"); }
}
class Pinguino extends Ave {
  volar(): void { throw new Error("no vuelo"); }
}
```

**Caso 3**

```typescript
class ReporteVentas {
  calcularTotales(): number { /* lógica */ return 0; }
  exportarPDF(): void { /* arma el PDF */ }
  enviarPorEmail(): void { /* manda el mail */ }
}
```

---

## 🛠️ Ejercicio en vivo — soluciones

**Caso 1 → viola OCP.**
La cadena de `if` crece con cada tipo de cliente nuevo. Definir `interface TipoCliente { descuento(monto): number }` y una clase por tipo (`ClienteRegular`, `ClientePremium`, `ClienteVip`). Agregar un tipo = agregar una clase, sin tocar la calculadora.

**Caso 2 → viola LSP.**
`Pinguino` es un `Ave` pero rompe la promesa de `volar()`. Reordenar la jerarquía: `Ave` con lo común, e `AveVoladora` (o una interfaz `Volador`) solo para las que vuelan. El pingüino **no** hereda `volar()`.

**Caso 3 → viola SRP.**
`ReporteVentas` calcula, exporta *y* envía: tres razones para cambiar. Separar en `ReporteVentas` (cálculo), `ExportadorPDF` y `NotificadorEmail`.

> Observá que arreglar OCP y LSP terminó, otra vez, en **interfaces + polimorfismo**.

---

## Cierre / Recap

Hoy vimos:

- Qué es **SOLID** y por qué existe: pelear contra el código **rígido, frágil e inmóvil**.
- Los **5 principios**, cada uno en formato **antes → después** con su "olor a código".
- Que el hilo común es **bajar el acoplamiento** apoyándose en **interfaces y polimorfismo** (lo que venías viendo desde la Clase 1).

**Próxima clase:** **Patrones de diseño** (Unidad 4) — soluciones ya probadas a problemas que se repiten. SOLID es el *por qué*; los patrones son el *cómo* concreto.

---

## 📚 Después de clase — Ficha resumen

**Guardá esto para tenerlo a mano.** Definición + el olor que delata la violación.

| Principio | En una frase | Olor que lo delata |
|---|---|---|
| **S** — Responsabilidad única | Una clase, una razón para cambiar | Describís la clase con un "y"; mezcla lógica + persistencia + UI |
| **O** — Abierto/Cerrado | Extender sin modificar lo existente | `if/switch` sobre un "tipo" que va a crecer; editás la misma clase por cada feature |
| **L** — Liskov | El hijo no rompe las promesas del padre | Métodos que tiran error o quedan vacíos; el cliente usa `instanceof` |
| **I** — Segregación de interfaces | Interfaces chicas y específicas | Clases con métodos vacíos o `throw "not supported"`; interfaces enormes |
| **D** — Inversión de dependencias | Depender de abstracciones, inyectar lo concreto | `new` de clases concretas adentro; imposible mockear para testear |

> **Cura recurrente:** casi todo se arregla con **una interfaz + polimorfismo + inyección por constructor**.

---

## Después de clase — Ejercicios

Para cada uno: **identificá el principio violado** y **reescribí el código (o el diagrama)** corregido.

1. **`Empleado`** con método `calcularSalario()`, y una subclase **`Voluntario extends Empleado`** cuyo `calcularSalario()` hace `throw new Error("no cobra")`. ¿Qué principio se viola? Reordená la jerarquía.

2. **`NotificadorPedidos`** crea adentro `this.emailSender = new GmailSender()`. Querés poder mandar también por SMS y, en los tests, no mandar nada.

3. Interfaz **`ITrabajador`** con `trabajar()`, `comer()` y `dormir()`. Una clase **`RobotObrero`** la implementa pero no come ni duerme. 

4. **`ProcesadorPago`** tiene un método `procesar(metodo: string)` con un `switch` sobre `"tarjeta"`, `"efectivo"`, `"transferencia"`. Mañana entra `"cripto"`.

5. **`GestorUsuario`** tiene los métodos `guardarEnBaseDeDatos()`, `validarEmail()` y `renderizarPerfilHTML()`. ¿Qué principio rompe y cómo lo partís?

> Traé los 5 resueltos (código o diagrama) para revisarlos al inicio de la próxima clase.

---

## Casos para razonar

Estos no son de "mirar código y listo": son **situaciones contadas en palabras**, como te las va a plantear un enunciado de parcial o un compañero de trabajo. Para cada uno, leé con calma y respondé **por escrito**:

1. ¿Qué principio(s) SOLID se está(n) violando? (puede ser **más de uno**)
2. ¿Cuál es la **frase o el síntoma** exacto del texto que te lo delata?
3. ¿Cómo lo rediseñarías? (nombrá clases/interfaces nuevas)

> No busques "la palabra clave". Reconstruí en tu cabeza **cómo se comportaría ese código cuando cambien los requisitos**. Ahí está la trampa de cada caso.

---

## 🧠 Caso 1 — El sistema de facturación

> En una PyME desarrollaron una clase `Factura`. Además de guardar los datos de la factura (cliente, ítems, total), esa misma clase tiene un método `calcularImpuestos()` que por dentro tiene un `switch` según el país: si es Argentina aplica IVA 21%, si es Uruguay otro porcentaje, si es Chile otro. Cada vez que la empresa empieza a vender en un país nuevo, un programador abre la clase `Factura`, agrega un `case` más al `switch` y vuelve a compilar. La última vez que lo hicieron, sin querer rompieron el cálculo de Argentina y estuvieron dos días facturando mal. Ah, y `Factura` **también** tiene un método `imprimirEnPDF()` y otro `enviarPorEmailAlCliente()`.

Identificá los principios en juego, aqui hay violaciones distintas conviviendo. Encontralas por separado y explicá por qué cada síntoma corresponde a un principio y no al otro.

---

## 🧠 Caso 2 — La jerarquía de cuentas bancarias

> Un banco modela sus productos con una clase base `CuentaBancaria` que tiene los métodos `depositar(monto)` y `extraer(monto)`. De ella heredan `CajaDeAhorro` y `CuentaCorriente`, y todo funciona: en muchas partes del sistema hay código que recibe una `CuentaBancaria` cualquiera y hace `cuenta.extraer(1000)` sin preguntar de qué tipo es. Tiempo después agregan `PlazoFijo`, que **también** hereda de `CuentaBancaria` porque "total, es una cuenta". Pero un plazo fijo no se puede tocar hasta el vencimiento, así que en `PlazoFijo` el método `extraer(monto)` quedó implementado como `throw new Error("no se puede extraer de un plazo fijo")`. Desde entonces, cada tanto, alguna pantalla del banco explota cuando un plazo fijo pasa por ese código que hacía `cuenta.extraer(...)`.

¿Qué principio se viola y por qué? Explicá **qué promesa** de la clase base rompió `PlazoFijo`. Proponé una jerarquía (o interfaces) que arregle el problema sin llenar el código de `if (cuenta instanceof PlazoFijo)`.

---

## 🧠 Caso 3 — El servicio de notificaciones

> El equipo tiene una clase `ProcesadorDePedidos` que, cuando se confirma un pedido, tiene que avisarle al cliente. Adentro del método `confirmar()`, el código hace directamente `const mailer = new SendGridMailer()` y después `mailer.enviarMail(...)`. Todo bien hasta que aparecen tres pedidos nuevos del negocio: (a) además del mail, algunos clientes quieren aviso por WhatsApp; (b) el equipo de testing se queja de que **no puede probar** `ProcesadorDePedidos` sin que se manden mails de verdad a la casilla de SendGrid; (c) marketing quiere cambiar de SendGrid a otro proveedor de mails el mes que viene, y calculan que hay que tocar como 15 clases distintas que hacen `new SendGridMailer()` desperdigadas por todo el sistema.

Nombrá el principio central que se está violando. Después explicá, punto por punto (a, b, c), **cómo el mismo rediseño** resuelve los tres problemas de una. ¿Qué abstracción falta y quién debería decidir qué implementación concreta se usa?

---

## 📖 Después de clase — Lectura recomendada

- **Refactoring Guru — SOLID Principles** (en español): explicación con ejemplos de cada principio.
  `refactoring.guru/es` → sección de principios de diseño.
- **Robert C. Martin — *Clean Code* / *Clean Architecture*** (autor original de SOLID), si querés ir a la fuente.
- Repasá la **Clase 1**: SOLID se apoya fuerte en **interfaces**, **clases abstractas** y **polimorfismo**. Si alguno quedó flojo, es el momento de reforzarlo.

> 💡 Consejo para el parcial: no memorices las siglas sueltas. Aprendé a **oler la violación** en un código o diagrama y a nombrar el principio. Eso es lo que suelen pedir.
