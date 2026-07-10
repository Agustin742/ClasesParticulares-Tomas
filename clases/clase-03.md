# Clase 3 — Patrones de diseño

> **Unidad:** 4 (Patrones de diseño) · **Lenguaje de ejemplos:** TypeScript

---

## Clase 3 — Patrones de diseño

**Programación II · Unidad 4**

> *"Los patrones de diseño identifican los problemas de diseño recurrentes y sus soluciones."* — Erich Gamma

En la Clase 2 vimos SOLID: el **por qué** del buen diseño (bajar el acoplamiento, extender sin romper). Hoy vemos el **cómo concreto**: soluciones **ya probadas** a problemas que aparecen una y otra vez. Un patrón es "la jugada que ya alguien resolvió bien, con nombre propio".

---

## ¿Qué es un patrón de diseño?

Un **patrón** es una **solución general y reutilizable** a un problema de diseño que se repite. **No es código para copiar y pegar**: es una **plantilla**, una forma de organizar clases e interfaces que vos adaptás a tu caso.

- Lo popularizó la **"Gang of Four" (GoF)** —Gamma, Helm, Johnson, Vlissides— en el libro *Design Patterns* (**1994**).
- Estudiaron cómo los buenos programadores resolvían los mismos problemas y les pusieron **nombre**.

> Analogía: en ajedrez, "enroque" o "gambito de dama" son patrones. No te dicen la partida entera; te dan una **jugada conocida** para una situación conocida. Si sabés el nombre, tu compañero te entiende en una palabra.

---

## ¿Por qué me deberían importar?

Dos razones bien concretas:

- **Vocabulario compartido.** Decir *"acá meté un Observer"* comunica en dos palabras una estructura entera. Sin el nombre, tenés que explicar cinco clases.
- **No reinventar la rueda (mal).** Son soluciones **maduras**, ya probadas, que respetan buenas prácticas de POO: **programar contra interfaces** y **preferir composición sobre herencia**.

> ⚠️ Advertencia del material de cátedra: *que un patrón exista no obliga a usarlo.* Meter patrones donde no hacen falta (**"patternitis"**) es tan malo como no usarlos. Primero el problema; después, si encaja, el patrón.

---

## El mapa: 3 familias

La GoF ordenó los patrones en **tres categorías**, según **qué tipo de problema** resuelven. Este mapa es **la herramienta más importante de hoy**: si sabés a qué familia pertenece un problema, ya descartaste dos tercios del catálogo.

- **Creacionales** → problemas de **cómo se crean** los objetos.
- **Estructurales** → problemas de **cómo se arman/relacionan** los objetos entre sí.
- **De comportamiento** → problemas de **cómo se comunican** y reparten responsabilidades los objetos.

> [IMAGEN: un diagrama tipo "mapa mental" con un nodo central que dice "Patrones de diseño (GoF)". Del centro salen tres ramas grandes hacia tres cajas de color distinto: una caja "CREACIONALES — crear objetos" (con etiquetas Singleton, Factory Method, Abstract Factory, Builder), otra caja "ESTRUCTURALES — armar/relacionar objetos" (con etiquetas Adapter, Composite, Decorator, Facade), y una tercera caja "COMPORTAMIENTO — comunicar objetos" (con etiquetas Strategy, Observer, Chain of Responsibility, Template Method). Cada caja tiene un ícono simple: una fábrica para creacionales, unos bloques encastrados para estructurales, y dos globos de diálogo para comportamiento.]

---

## Familia 1 — Creacionales

Resuelven **cómo se crean los objetos**. El problema típico: tu código hace `new ClaseConcreta()` por todos lados y queda **casado** con esa clase; el día que necesitás otra, tenés que tocar diez lugares. Estos patrones **encapsulan la creación** detrás de una abstracción, así el resto del código pide "dame un X" sin saber —ni importarle— cuál concreto le toca.

- **Analogía:** un restaurante. Vos pedís "una pizza"; no entrás a la cocina a armarla. La cocina (la fábrica) decide cómo se construye y con qué.
- **Qué comparten:** separan *qué se usa* de *cómo se construye*. Ganás flexibilidad para cambiar la clase concreta sin romper al cliente.
- **Viven acá:** Factory Method, Abstract Factory, Builder, Prototype, Singleton.

---

## Familia 2 — Estructurales

Resuelven **cómo se componen y relacionan** los objetos para armar estructuras más grandes sin que todo quede rígido. El problema típico: tenés piezas que **no encajan**, o querés **sumar / esconder / organizar** comportamiento sin reescribir lo que ya funciona.

- **Analogía:** una remodelación de la casa. No la tirás abajo: ponés un adaptador de enchufe, una pared que esconde los caños, un mueble que suma una función. Cambiás la estructura **envolviendo o reacomodando**, no rompiendo.
- **Qué comparten:** casi todos se apoyan en **composición** (un objeto contiene a otro) para reorganizar sin depender de la herencia.
- **Viven acá:** Adapter, Decorator, Composite, Facade, Bridge, Proxy, Flyweight.

---

## Familia 3 — De comportamiento

Resuelven **cómo interactúan y se reparten el trabajo** los objetos: quién le habla a quién, quién decide qué, cómo fluye una petición. El problema típico: la lógica de "quién hace qué y cuándo" termina enredada en `if` gigantes, o en objetos que **se conocen demasiado** entre sí.

- **Analogía:** una oficina. No importa tanto *quién es* cada empleado, sino **el protocolo**: cómo circula un trámite, a quién se le avisa cuando algo cambia, quién delega en quién.
- **Qué comparten:** desacoplan al que **pide** algo del que lo **hace**, casi siempre metiendo una interfaz en el medio.
- **Viven acá:** Strategy, Observer, Chain of Responsibility, Template Method, State, Command, Iterator, Mediator, Memento, Visitor.
> Es la familia **más numerosa** (10 patrones): tiene sentido, "cómo se comunican los objetos" es donde más variantes de problema aparecen.

---

## Cómo descartar entre familias (el método)

Frente a un enunciado, hacete **una sola pregunta primero**: *¿de qué se queja el problema?*

- *"No sé qué clase concreta instanciar / quiero controlar la creación / el `new` está desparramado"* → **Creacional**.
- *"Tengo que hacer encajar cosas incompatibles / envolver / simplificar una estructura / representar un todo-y-sus-partes"* → **Estructural**.
- *"Tengo que elegir un comportamiento / avisarle a varios cuando algo pasa / pasar una petición por una cadena"* → **Comportamiento**.

> Palabras que delatan la familia en un enunciado:
> - **Creacional:** *crear, instanciar, `new`, única instancia, familia de productos, construir paso a paso*.
> - **Estructural:** *adaptar, envolver, compatibilizar, árbol, jerarquía, esconder complejidad, agregar sin tocar*.
> - **Comportamiento:** *avisar, notificar, suscribir, intercambiar algoritmo, en tiempo de ejecución, cadena, pasos fijos*.

---

## 🧠 Probá el método — ¿qué familia?

Todavía sin pensar en el patrón puntual, solo la **familia**. Leé cada uno y decí en voz alta: *¿el problema es de crear, estructurar o comunicar?* y **qué palabra** te lo dice.

1. "Cada vez que entra un nuevo tipo de usuario hay que armar el objeto de otra manera, y el `new` está repetido en medio código."
2. "Cuando cambia el stock, tienen que enterarse el carrito, el mail de aviso y el panel de admin."
3. "Tenemos una librería de PDF con métodos de nombres rarísimos y queremos usarla como si fuera nuestra clase `Documento`."

> Chequealo: **1)** crear → *creacional* · **2)** avisar a varios → *comportamiento* · **3)** hacer encajar algo ajeno → *estructural*. Fijate que **no hizo falta** saber el patrón exacto para descartar dos tercios del catálogo.

---

## Un hilo que ya conocés

Antes de entrar a los patrones "ancla", fijate que **casi todos usan las mismas dos herramientas** que venís viendo desde la Clase 1:

- **Programar contra una interfaz** (no contra la clase concreta).
- **Composición** (un objeto guarda a otro) en lugar de herencia.

> Si SOLID te enseñó a *oler* el problema (un `switch` que crece, un `new` cableado adentro, una subclase que rompe la promesa del padre), los patrones son el **catálogo de curas** con nombre. Muchos son, literalmente, "OCP hecho patrón".

---

## Los 5 patrones ancla de hoy

No vamos a ver los 22. Vamos a ver **uno o dos por familia**, bien elegidos, y para cada uno lo que de verdad importa en un parcial: **la señal delatora**.

- **Creacional** → **Factory Method**
- **Estructurales** → **Adapter** y **Decorator**
- **Comportamiento** → **Strategy** y **Observer**

> Para cada patrón vamos con el mismo molde: **intención** (una frase) → **señal delatora** (cómo lo reconocés) → **código chico** → **UML** → **con qué principio SOLID se conecta**. Aprendete el molde y el resto del catálogo lo leés solo.

---

## Factory Method — intención

**Familia:** creacional.

**Intención:** proporcionar una interfaz para **crear objetos**, dejando que las **subclases decidan qué clase concreta** instanciar. El código cliente trabaja contra la **abstracción del producto**, no contra las clases concretas.

En criollo: en vez de escribir `new ProductoConcreto()` desparramado por todos lados, centralizás la creación en un **método fábrica** que cada subclase implementa a su manera.

---

## Factory Method — señal delatora

Sospechá Factory Method (o algún creacional) cuando…

- Tenés `new ClaseConcreta()` **repetido** en muchos lugares, y sabés que **la clase concreta va a cambiar** o crecer.
- Un `if/switch` decide **qué objeto crear** según un tipo (`if tipo === "pdf" return new PdfExporter()`…). 👈 el mismo olor de OCP, pero en la **creación**.
- Querés que **quien usa** el objeto **no dependa** de cuál concreto es.

```typescript
// Producto: la abstracción con la que trabaja el cliente
interface Transporte {
  entregar(): string;
}

class Camion implements Transporte {
  entregar(): string { return "Entrega por tierra en caja"; }
}
class Barco implements Transporte {
  entregar(): string { return "Entrega por mar en contenedor"; }
}
```

---

## 🤔 Pará y pensá — el `if` que crece

Antes de ver mi solución: imaginá que resolvés el transporte con un `switch`.

```typescript
function crearTransporte(tipo: string): Transporte {
  if (tipo === "camion") return new Camion();
  if (tipo === "barco")  return new Barco();
  // ...
}
```

- En un sistema real ese mismo `tipo` se repite en **otros** `switch` (uno para el costo, otro para el ícono, otro para el seguimiento…). Si agregás `Avion`, ¿en **cuántos lugares** te tenés que acordar de sumar el caso? ¿Qué pasa si te olvidás de uno?
- Pista de la Clase 2: esto es el olor exacto de **OCP**. ¿Con qué lo curábamos? 🧩


---

## Factory Method — el creador

El **creador** define el método fábrica **abstracto**; cada subclase decide qué producto fabricar. La lógica que **usa** el transporte vive en el creador y **no sabe** cuál concreto le toca.

```typescript
abstract class Logistica {
  // las subclases deciden QUÉ transporte crear
  protected abstract crearTransporte(): Transporte;

  // lógica que NO cambia y no conoce la clase concreta
  planificarEntrega(): string {
    const t = this.crearTransporte();   // ← método fábrica
    return `Planificando: ${t.entregar()}`;
  }
}

class LogisticaTerrestre extends Logistica {
  protected crearTransporte(): Transporte { return new Camion(); }
}
class LogisticaMaritima extends Logistica {
  protected crearTransporte(): Transporte { return new Barco(); }
}

const l: Logistica = new LogisticaMaritima();
console.log(l.planificarEntrega());  // Planificando: Entrega por mar en contenedor
```

¿Aparece un `Avion`? Nueva subclase `LogisticaAerea`, y **no tocás** `planificarEntrega()`. **OCP puro.**

---

## Factory Method — UML

> [IMAGEN: diagrama de clases UML del patrón Factory Method. Arriba a la izquierda una interfaz «interface» "Transporte" con el método entregar(). Debajo, dos clases "Camion" y "Barco" conectadas a la interfaz con flechas de realización (línea punteada, triángulo vacío). Arriba a la derecha una clase abstracta "Logistica" (nombre en itálica) con dos métodos: crearTransporte() en itálica y marcado como abstracto, y planificarEntrega(). Debajo, dos clases "LogisticaTerrestre" y "LogisticaMaritima" conectadas a Logistica por herencia (línea con triángulo vacío), cada una redefiniendo crearTransporte(). Una flecha de dependencia punteada va desde LogisticaMaritima hacia "Barco" con la etiqueta «create», y otra desde LogisticaTerrestre hacia "Camion". Un globo sobre planificarEntrega() dice: "usa un Transporte, no sabe cuál".]

**Conexión SOLID:** OCP (agregar productos sin tocar el cliente) y SRP (el código de creación queda en un solo lugar).

---

## Adapter — intención

**Familia:** estructural.

**Intención:** hacer que **dos interfaces incompatibles trabajen juntas**. El Adapter **envuelve** una clase existente (que **no podés o no querés modificar**) y le pone por delante la interfaz que tu código espera.

> Analogía física perfecta: el **adaptador de enchufe** cuando viajás. El tomacorriente de la pared (interfaz A) y tu cargador (interfaz B) no encajan. No cambiás ni la pared ni el cargador: metés un **adaptador** en el medio.

---

## Adapter — señal delatora

Sospechá Adapter cuando…

- Querés usar una **librería / clase vieja / API externa** cuyos métodos **no encajan** con lo que el resto de tu código espera.
- No podés (o no conviene) **tocar** esa clase (código de terceros, sistema legacy).
- Ves que traducís "a mano" de un formato a otro en muchos lugares.

```typescript
// Lo que MI código espera usar en todos lados:
interface ProveedorClima {
  temperatura(ciudad: string): number;
}

// Clase de una librería externa, con OTRA interfaz (no la puedo tocar):
class WeatherApiExterna {
  fetchWeatherFahrenheit(city: string): number {
    return 68; // supongamos que devuelve °F
  }
}
```

---

## Adapter — el adaptador

El adaptador **implementa la interfaz que espero** y **por dentro** se compone de la clase incompatible, traduciendo la llamada.

```typescript
class ClimaAdapter implements ProveedorClima {
  private api = new WeatherApiExterna();   // ← composición: envuelve al "adaptee"

  temperatura(ciudad: string): number {
    const f = this.api.fetchWeatherFahrenheit(ciudad);
    return Math.round((f - 32) * 5 / 9);   // traduce °F → °C
  }
}

// El cliente habla SOLO con ProveedorClima; ni se entera de la API externa:
function mostrarClima(prov: ProveedorClima) {
  console.log(`${prov.temperatura("Rosario")} °C`);
}
mostrarClima(new ClimaAdapter());   // 20 °C
```

---

## Adapter — UML

> [IMAGEN: diagrama de clases UML del patrón Adapter. A la izquierda una caja "Client". El Client apunta con una flecha de dependencia hacia una interfaz «interface» "ProveedorClima" con el método temperatura(ciudad). Debajo de la interfaz, una clase "ClimaAdapter" conectada por realización (línea punteada, triángulo vacío) que implementa temperatura(). Desde "ClimaAdapter" sale una flecha de composición (rombo relleno) o de asociación hacia la derecha, hacia una caja "WeatherApiExterna" (el adaptee) que tiene el método fetchWeatherFahrenheit(city). Una nota sobre ClimaAdapter dice: "traduce °F → °C por dentro". Un rótulo aclara que WeatherApiExterna es código externo que no se puede modificar.]

**Conexión SOLID:** DIP (el cliente depende de la abstracción `ProveedorClima`, no de la API concreta) y OCP (podés cambiar de proveedor con otro adapter sin tocar al cliente).

---

## Decorator — intención

**Familia:** estructural.

**Intención:** **agregar responsabilidades a un objeto de forma dinámica**, envolviéndolo, **sin tocar su clase** y sin usar herencia. Es una alternativa flexible a subclasear para todo.

La clave: el decorador **tiene la misma interfaz** que el objeto que envuelve, así que quien lo usa **no nota la diferencia**… pero por dentro le suma comportamiento.

> Analogía: un **café**. Empezás con un espresso. Le agregás leche (lo envolvés), le agregás caramelo (lo volvés a envolver). Sigue siendo "una bebida que cuesta $X y se describe", pero cada capa **suma precio y descripción**.

---

## 🤔 Razoná — ¿por qué no con herencia?

Un café puede llevar leche, caramelo y/o crema, en **cualquier combinación**. Si lo resolvieras creando una subclase por combinación (`CafeConLeche`, `CafeConLecheYCaramelo`, `CafeConCremaYCaramelo`…):

- Contá **cuántas clases** necesitás para cubrir esas 3 opciones en toda combinación posible. ¿Y si mañana entra una cuarta?
- ¿Qué hacés si querés decidir los agregados **en runtime**, según lo que pide el cliente en el momento?

> Con 3 opcionales ya son **2³ = 8** clases; con 4, dieciséis. La herencia **explota**. Guardá esa sensación: es exactamente lo que Decorator viene a resolver.

---

## Decorator — señal delatora

Sospechá Decorator cuando…

- Querés **combinar funcionalidades opcionales** en cualquier orden, y por herencia te explotarían las subclases (`CafeConLecheYCaramelo`, `CafeConCaramelo`, `CafeConLecheYCarameloYCrema`… 💥 combinatoria).
- Necesitás **agregar comportamiento en tiempo de ejecución**, no en tiempo de compilación.
- No querés (o no podés) modificar la clase original.

```typescript
interface Cafe {
  costo(): number;
  descripcion(): string;
}

class Espresso implements Cafe {
  costo(): number { return 1500; }
  descripcion(): string { return "espresso"; }
}
```

---

## Decorator — las capas

El decorador **implementa `Cafe`** y **guarda un `Cafe` adentro** (composición). Delega en el envuelto y **le suma lo suyo**.

```typescript
// Decorador base: mismo tipo que el envuelto, y lo compone
abstract class CafeDecorator implements Cafe {
  constructor(protected base: Cafe) {}
  abstract costo(): number;
  abstract descripcion(): string;
}

class ConLeche extends CafeDecorator {
  costo(): number { return this.base.costo() + 300; }
  descripcion(): string { return this.base.descripcion() + " + leche"; }
}
class ConCaramelo extends CafeDecorator {
  costo(): number { return this.base.costo() + 500; }
  descripcion(): string { return this.base.descripcion() + " + caramelo"; }
}

// Envuelvo en capas, en el orden que quiera:
let bebida: Cafe = new ConCaramelo(new ConLeche(new Espresso()));
console.log(bebida.descripcion(), bebida.costo()); // espresso + leche + caramelo 2300
```

---

## Decorator — UML

> [IMAGEN: diagrama de clases UML del patrón Decorator. Arriba una interfaz «interface» "Cafe" con los métodos costo() y descripcion(). De ella bajan por realización (línea punteada, triángulo vacío) dos elementos: a la izquierda la clase concreta "Espresso"; a la derecha una clase abstracta "CafeDecorator" (nombre en itálica). Además de heredar/implementar la interfaz, "CafeDecorator" tiene una relación de composición (rombo relleno) hacia la interfaz "Cafe", etiquetada "base" — indicando que un decorador CONTIENE otro Cafe. Debajo de CafeDecorator, dos clases "ConLeche" y "ConCaramelo" conectadas por herencia. Una nota dice: "un decorador ES un Cafe y a la vez TIENE un Cafe: por eso se pueden apilar en capas".]

**Conexión SOLID:** OCP (extendés funcionalidad sin tocar `Espresso`) y SRP (cada decorador agrega **una** cosa).

> ⚠️ No confundas **Decorator** con **Adapter**: los dos envuelven un objeto, pero el Adapter **cambia la interfaz** (para que encaje), y el Decorator **mantiene la interfaz** (para sumar comportamiento).

---

## Strategy — intención

**Familia:** comportamiento.

**Intención:** definir una **familia de algoritmos**, encapsular cada uno en su clase y hacerlos **intercambiables**. El objeto que los usa (el **contexto**) puede **cambiar de algoritmo** —incluso en **tiempo de ejecución**— sin cambiar su propio código.

> El material lo llama *"el patrón más representativo del polimorfismo"*: es composición + interfaz en su forma más pura. Si entendiste el ejemplo de `Pago` de la Clase 2, ya entendiste Strategy.

---

## Strategy — señal delatora

Sospechá Strategy cuando…

- Tenés un `if/switch` que elige **cómo hacer algo** (cómo ordenar, cómo calcular el envío, cómo comprimir) y esos "cómo" van a crecer.
- Querés poder **cambiar el algoritmo en runtime** (el usuario elige el método de pago, el tipo de envío…).
- Varias clases hacen **lo mismo con pequeñas variantes** y querés no duplicar.

```typescript
interface EstrategiaEnvio {
  costo(pesoKg: number): number;
}

class EnvioEstandar implements EstrategiaEnvio {
  costo(pesoKg: number): number { return pesoKg * 100; }
}
class EnvioExpress implements EstrategiaEnvio {
  costo(pesoKg: number): number { return pesoKg * 250 + 500; }
}
```

---

## 🤔 Predecí — cambiar de algoritmo sin tocar al cliente

Tenés `EnvioEstandar` y `EnvioExpress`. El `Carrito` tiene que poder calcular el total con uno u otro, e incluso **cambiar** de tipo de envío después de creado.

- Sin mirar la próxima slide: ¿qué tendría que **guardar** `Carrito` para no quedar atado a un envío concreto?


---

## Strategy — el contexto

El **contexto** guarda una **estrategia** (composición contra la interfaz) y **delega** en ella. Puede recibirla o cambiarla cuando quiera.

```typescript
class Carrito {
  constructor(private envio: EstrategiaEnvio) {}

  setEnvio(e: EstrategiaEnvio) { this.envio = e; }   // ← cambio en runtime

  total(pesoKg: number, subtotal: number): number {
    return subtotal + this.envio.costo(pesoKg);      // delega, no sabe cuál es
  }
}

const c = new Carrito(new EnvioEstandar());
console.log(c.total(2, 5000));      // 5200
c.setEnvio(new EnvioExpress());     // cambio la estrategia sin tocar Carrito
console.log(c.total(2, 5000));      // 6000
```

Agregar `EnvioInternacional` = una clase nueva. `Carrito` **ni se entera**.

---

## Strategy — UML

> [IMAGEN: diagrama de clases UML del patrón Strategy. A la izquierda una caja "Carrito" (el contexto). Desde "Carrito" sale una flecha de composición (rombo relleno) hacia una interfaz «interface» "EstrategiaEnvio" con el método costo(pesoKg), etiquetada "envio". Debajo de la interfaz, dos clases "EnvioEstandar" y "EnvioExpress" conectadas por realización (línea punteada, triángulo vacío), cada una con su propia versión de costo(). Un globo sobre Carrito dice: "delega el cálculo a la estrategia; puede cambiarla en runtime".]

**Conexión SOLID:** OCP (nuevas estrategias sin tocar el contexto) y DIP (el contexto depende de la interfaz).

> Ojo: **Strategy** y **Factory Method** se parecen en el UML (una interfaz + varias implementaciones). La diferencia es la **intención**: Factory Method resuelve **qué crear**; Strategy resuelve **cómo hacer algo** con un objeto que ya tenés.

---

## Observer — intención

**Familia:** comportamiento.

**Intención:** definir una dependencia **uno-a-muchos** entre objetos, de modo que cuando **uno** (el **sujeto/observado**) cambia de estado, **todos** los que dependen de él (los **observadores**) se enteran y reaccionan **automáticamente**.

> Analogía: el **canal de YouTube**. El canal (sujeto) sube un video; **todos los suscriptores** (observadores) reciben la notificación, sin que el canal sepa quién es cada uno. Te suscribís (`attach`) y te desuscribís (`detach`) cuando querés.

---

## Observer — señal delatora

Sospechá Observer cuando…

- Un cambio en un objeto tiene que **disparar reacciones en varios otros**, y no querés que el primero **conozca** a cada uno.
- Escuchás palabras como **notificar, avisar, suscribir, "cuando pase X, que se actualicen Y y Z"**, eventos.
- El conjunto de "interesados" **cambia con el tiempo** (se suman, se van).

```typescript
interface Observer {
  update(estado: string): void;
}
interface Subject {
  attach(o: Observer): void;
  detach(o: Observer): void;
  notify(): void;
}
```

---

## 🤔 Pará y pensá — el costo de conocer a todos

Imaginá que `Canal` avisa a los suscriptores llamándolos **por nombre**, hardcodeado:

```typescript
subirVideo(t: string) {
  ana.avisar(t);
  beto.avisar(t);   // ...
}
```

- Cuando se suscribe **Caro**, ¿qué tenés que tocar? ¿Y si **Ana se va**?
- ¿`Canal` debería **conocer** a cada suscriptor concreto para hacer su trabajo?

> Cada alta o baja te obliga a editar `Canal`. La cura: que `Canal` hable con una **lista de una interfaz** (`Observer`) y no sepa quién hay del otro lado. Eso es Observer.

---

## Observer — sujeto y observadores

El **sujeto** guarda la lista de observadores y, al cambiar, llama a `update()` de cada uno.

```typescript
class Canal implements Subject {
  private observers: Observer[] = [];
  private ultimoVideo = "";

  attach(o: Observer) { this.observers.push(o); }
  detach(o: Observer) { this.observers = this.observers.filter(x => x !== o); }
  notify() { this.observers.forEach(o => o.update(this.ultimoVideo)); }

  subirVideo(titulo: string) {
    this.ultimoVideo = titulo;
    this.notify();                 // ← avisa a TODOS los suscriptos
  }
}

class Suscriptor implements Observer {
  constructor(private nombre: string) {}
  update(video: string) { console.log(`${this.nombre} → nuevo video: ${video}`); }
}

const canal = new Canal();
canal.attach(new Suscriptor("Ana"));
canal.attach(new Suscriptor("Beto"));
canal.subirVideo("Patrones de diseño en 10 min");
// Ana → nuevo video: ...   |   Beto → nuevo video: ...
```

---

## Observer — UML

> [IMAGEN: diagrama de clases UML del patrón Observer. A la izquierda, una interfaz «interface» "Subject" con los métodos attach(o), detach(o) y notify(); debajo la clase "Canal" que la implementa (realización, línea punteada con triángulo vacío) y tiene un atributo -ultimoVideo. A la derecha, una interfaz «interface» "Observer" con el método update(estado); debajo la clase "Suscriptor" que la implementa. Desde "Subject" (o "Canal") sale una flecha de agregación (rombo vacío) hacia "Observer" con la etiqueta "observers *" indicando que el sujeto mantiene una lista de observadores. Una flecha punteada de dependencia va del sujeto al observador etiquetada "notify() → update()". Nota: "el sujeto conoce la INTERFAZ Observer, no a los suscriptores concretos".]

**Conexión SOLID:** OCP (sumás observadores sin tocar el sujeto) y bajo acoplamiento (sujeto y observadores se hablan por interfaz).

---

## 🧠 Checkpoint — antes del recap

- **Adapter vs Decorator:** los dos envuelven un objeto. ¿En qué se diferencian?
- **Strategy vs Factory Method:** ¿cuál decide *qué crear* y cuál *cómo hacer algo* con algo que ya tenés?
- De los 5 de hoy, ¿cuáles atacan un problema de **comunicar** entre objetos?

> Defini con tus palabras los 5 Patrones de Diseño

**Factory Method**
**Adapter**
**Decorator**
**Strategy**
**Observer**

---

## Recap — los 5 anclas

| Patrón | Familia | Señal delatora (una línea) |
|---|---|---|
| **Factory Method** | Creacional | `switch`/`new` que decide **qué clase crear**; querés no atarte al concreto |
| **Adapter** | Estructural | Dos interfaces **incompatibles** que hay que hacer encajar (librería/legacy) |
| **Decorator** | Estructural | **Sumar funcionalidades opcionales** apilables, sin herencia ni tocar la clase |
| **Strategy** | Comportamiento | Elegir **cómo hacer algo** entre varios algoritmos, intercambiable en runtime |
| **Observer** | Comportamiento | Un cambio debe **avisar a varios** que se suscriben/desuscriben |

> El hilo de siempre: **interfaz + composición**. Todos "invierten" la dependencia hacia una abstracción. Patrones = SOLID con nombre y apellido.

---

## 🛠️ Ejercicio en vivo — ¿qué patrón conviene?

No te digo la familia ni el patrón. Leé cada caso, decidí **cuál de los 5 de hoy** aplica y justificá en una frase. Recién después pasá a la solución.

**Caso A.** En un juego online, cuando un jugador sube de nivel tienen que reaccionar el marcador, el chat y el sistema de sonido. El equipo ya adelantó que van a sumar más reacciones (misiones, ranking). El jugador no debería tener que conocer a ninguna de esas partes.

**Caso B.** Un editor abre archivos de distinto tipo (texto, planilla, presentación). El procedimiento de "abrir" es siempre el mismo, pero el objeto documento que termina creándose depende de la extensión del archivo.

**Caso C.** Tu sistema de cobros usa en todos lados una interfaz propia `Pago.pagar(monto)`. Integrás MercadoPago, cuyo SDK ofrece `createPayment({ amount, currency })`, con otros nombres de campos y otro formato de respuesta. El resto del sistema tiene que seguir cobrando igual que siempre.

---

## 🛠️ Ejercicio en vivo — soluciones

**Caso A → Observer.** Es un problema de *comunicar*: un objeto cambia de estado y varios interesados —que van a crecer— deben reaccionar sin que el emisor los conozca. El jugador es el `Subject`; marcador, chat y sonido son `Observer`. Sumar una reacción = un observador nuevo.

**Caso B → Factory Method.** Es un problema de *crear*: el flujo de "abrir" no cambia, pero **qué objeto se instancia** sí. Una clase base define `abrir()` y un método fábrica abstracto `crearDocumento()`; cada subclase decide el tipo concreto.

**Caso C → Adapter.** Es un problema de *estructura*: dos interfaces que no encajan y una (el SDK de MercadoPago) que no controlás. `class MercadoPagoAdapter implements Pago` compone el SDK y traduce `pagar(monto)` → `createPayment({ amount: monto, currency: "ARS" })`. El resto del sistema le sigue hablando a `Pago` y ni se entera.

> El método: primero **¿el problema es de crear, estructurar o comunicar?** → familia. Recién ahí, el patrón puntual.

---

## Cierre / Recap

Hoy vimos:

- Qué es un patrón: una **solución probada con nombre** a un problema recurrente (GoF, 1994).
- El **mapa de 3 familias** —creacional / estructural / comportamiento— y el **método** para descartar: *¿el problema es de crear, de estructurar o de comunicar?*
- **5 patrones ancla** (Factory Method, Adapter, Decorator, Strategy, Observer), cada uno por su **señal delatora**, su código y su UML.
- Que todos se apoyan en lo mismo de siempre: **interfaces + composición** → o sea, **SOLID en acción**.

**Próxima clase:** seguimos con patrones, **profundizando** en más del catálogo y en *cuándo conviene* cada uno con ejemplos y diagramas.

---

## 📊 Después de clase — ¿Cuál elijo? (los 5 de hoy)

Guardá esta tabla: la **pregunta que separa** a cada patrón es lo que te salva en el parcial.

| Patrón | La pregunta que lo delata | En una frase |
|---|---|---|
| **Factory Method** | ¿El problema es **qué objeto crear** sin atarme al concreto? | Subclases deciden la clase concreta |
| **Adapter** | ¿Tengo dos interfaces que **no encajan** y una no la controlo? | Traduzco una interfaz a la que espero |
| **Decorator** | ¿Quiero **sumar comportamientos combinables** sin tocar la clase? | Envuelvo manteniendo la misma interfaz |
| **Strategy** | ¿Quiero elegir **cómo hago algo** entre varias formas, en runtime? | Algoritmo intercambiable por composición |
| **Observer** | ¿Un cambio debe **avisar a varios** que aparecen y desaparecen? | Uno-a-muchos: el sujeto notifica |

**Los dos pares que más se confunden:**
- **Adapter vs Decorator:** los dos envuelven un objeto. Adapter **cambia** la interfaz (para que encaje); Decorator **mantiene** la interfaz (para sumar comportamiento).
- **Strategy vs Factory Method:** el UML es casi igual (interfaz + implementaciones). Factory Method decide **qué crear**; Strategy decide **cómo hacer algo** con algo que ya tenés.

---

## ✍️ Después de clase — Ejercicios

Para cada uno:

1. Decidí **qué patrón** de los 5 aplica.
2. **Justificá:** ¿el problema es de *crear*, *estructurar* o *comunicar*? ¿Por qué ese y no el parecido?
3. Bosquejá el **diagrama UML** y un **esqueleto de clases** en TypeScript.

Las soluciones están al final;

**1.** Una app de préstamos calcula los intereses con varias fórmulas: tasa fija, tasa variable y una promocional por tiempo limitado. El producto va agregando fórmulas nuevas seguido, y un mismo préstamo puede pasar de una fórmula a otra sin recrearse. Querés que el objeto préstamo no tenga que reescribirse cada vez que aparece una fórmula.

**2.** Un tablero de una app de trading muestra, para cada acción, un gráfico, un registro de alertas y el total de la cartera. Cuando el precio de una acción se actualiza, esas tres vistas tienen que quedar al día. Además, el usuario puede agregar o quitar vistas mientras la app corre.

**3.** Tenés una clase `Mensaje` con `contenido()` que ya funciona y preferís no tocar. Según el caso, un mensaje puede ir cifrado, comprimido, firmado, o varias de esas cosas a la vez, y quien arma el mensaje decide la combinación en el momento.

**4.** Un framework de UI define el procedimiento para mostrar una ventana modal (crearla, posicionarla, animar la aparición). Ese procedimiento es idéntico para todas, pero cada tipo de pantalla —login, alerta, formulario— necesita crear su propio contenido interno de ventana.

**5.** Tu módulo de reportes consume datos vía `FuenteDatos.obtener(): Registro[]`. Entra un cliente que expone su información por un servicio con métodos `fetchRecords()` que devuelven otro formato y otros nombres de campo. Tenés que alimentar ese origen a tu flujo de reportes sin reescribirlo.

**6.** Una app de navegación ya tiene cargados el mapa y el destino. El usuario puede pedir la ruta **más rápida**, la **más corta** o una que **evite peajes**, y puede cambiar de criterio sobre la misma búsqueda —sin arrancar de cero— las veces que quiera.

**7.** En un videojuego, cada nivel genera oleadas de enemigos. El procedimiento para lanzar una oleada es siempre el mismo (elegir la posición de aparición, hacerlos aparecer y activarles la IA), pero el tipo de enemigo depende del nivel: en el bosque salen lobos, en la cueva murciélagos, en el castillo caballeros. Querés que sumar un nivel nuevo no te obligue a tocar la lógica de las oleadas. 

---

## ✍️ Después de clase — Soluciones

**1 → Strategy.** *Comunicar/comportamiento:* no es crear ni estructurar, es **elegir cómo se calcula** algo y poder cambiarlo en caliente. `interface CalculoInteres { calcular(capital, plazo): number }` con `TasaFija`, `TasaVariable`, `TasaPromo`; `Prestamo` compone una `CalculoInteres` y delega. Fórmula nueva = clase nueva.
UML: `Prestamo` --◆--> «interface» `CalculoInteres`, y `TasaFija/TasaVariable/TasaPromo` la realizan.

**2 → Observer.** *Comunicar:* un cambio (el precio) dispara reacciones en varias vistas que aparecen y desaparecen. `Accion` es `Subject` (`attach`/`detach`/`notify`); gráfico, alertas y cartera son `Observer` con `update(precio)`.
UML: `Subject` mantiene una lista `*` de `Observer`; al cambiar, llama `update()` de cada uno.

**3 → Decorator.** *Estructura:* sumar responsabilidades combinables sin tocar `Mensaje` ni explotar en subclases (`CifradoYComprimido`, `CifradoYFirmado`…). `MensajeDecorator implements Mensaje` y **contiene** un `Mensaje`; `Cifrado`, `Comprimido`, `Firmado` extienden y envuelven → `new Firmado(new Cifrado(base))`. No es Adapter: la interfaz **se mantiene**, no se traduce.

**4 → Factory Method.** *Crear:* el procedimiento de mostrar la ventana no cambia; cambia **qué contenido se crea**. Clase base `Ventana` con `mostrar()` (flujo fijo) que llama al método fábrica abstracto `crearContenido()`; `VentanaLogin`, `VentanaAlerta`, `VentanaForm` lo redefinen.
UML: Creator abstracto <|-- ConcreteCreators; el método fábrica devuelve un `Contenido` (Product).

**5 → Adapter.** *Estructura:* una interfaz que no encaja y que no controlás. `class ServicioAdapter implements FuenteDatos` compone ese servicio y traduce `obtener()` → `fetchRecords()` + mapea el formato. El flujo de reportes queda intacto.

**6 → Strategy.** El detalle que lo decide: los datos ya están (mapa + destino) y lo que cambia es **cómo se calcula**, pudiendo **cambiar el criterio en runtime** sobre la misma búsqueda. Eso es un algoritmo intercambiable, no la creación de un producto. `interface CalculadorRuta { calcular(mapa, destino): Ruta }` con `MasRapida`, `MasCorta`, `EvitaPeajes`; la app compone uno y lo reemplaza cuando el usuario cambia de criterio. *No es Factory Method: ahí el problema sería "según un tipo, qué objeto instanciar"; acá no fabricás productos distintos, variás el cálculo sobre datos que ya tenés.*

**7 → Factory Method.** *Crear:* la lógica de la oleada no cambia; lo que cambia es **qué enemigo se instancia**. Clase base `Nivel` con `lanzarOleada()` (flujo fijo: posición → aparecer → activar IA) que llama al método fábrica abstracto `crearEnemigo(): Enemigo`; `NivelBosque`, `NivelCueva`, `NivelCastillo` lo redefinen devolviendo `Lobo`, `Murcielago`, `Caballero`. Nivel nuevo = subclase nueva, sin tocar `lanzarOleada()`.
UML: `Nivel` (abstracto, con `lanzarOleada()` + `crearEnemigo()` abstracto) <|-- `NivelBosque`/`NivelCueva`/`NivelCastillo`; `crearEnemigo()` --> «interface» `Enemigo`.

---

## 📖 Después de clase — Lectura recomendada

- **Refactoring Guru — Patrones de diseño** (en español): `refactoring.guru/es/design-patterns`. Repasá **las fichas de los 5 de hoy** (Factory Method, Adapter, Decorator, Strategy, Observer): cada una trae analogía, código y UML. Es la fuente principal de la unidad.
- **Gang of Four — *Design Patterns* (1994)**: el libro original, si querés ir a la fuente (denso, pero es *el* clásico).
- Repasá la **Clase 2 (SOLID)**: cada patrón es, en el fondo, uno o más principios SOLID resueltos. Si ves un patrón y no reconocés qué principio respeta, volvé a esa clase.

> 💡 Consejo para el parcial: te van a dar un **enunciado con un síntoma**, no el nombre del patrón. Entrená el reflejo en dos pasos: **1)** ¿la queja es de *crear*, *estructurar* o *comunicar*? → familia. **2)** ¿cuál es la señal que lo distingue del patrón parecido? → patrón.
