# Clase 6 — Parciales: leer, decidir, justificar

## Cuatro parciales reales

Estos cuatro enunciados son de la cátedra. Uno de ellos —**TravelNow**— es el **final de Mayo 2026, TEMA 2**: el examen textual, no una imitación. Por eso arrancamos por ese.

Están **transcriptos tal cual**: sin negritas, sin resúmenes y con los typos del original. Así los vas a ver en el examen, y **subrayar es trabajo tuyo, no mío**.

El orden es: **TravelNow** → **Heladería** → **Reservas** → **Trenes**. Si no llegamos a los cuatro, no pasa nada; están puestos de mayor a menor rendimiento.

Herramienta única para hoy: **la ficha de los 13 patrones de la Clase 5**. Tenela al lado. No vas a necesitar nada más.

---

## Cómo se lee un parcial

Cuatro pasos, en este orden, **con el lápiz en la mano y antes de dibujar una sola caja**. Hoy los vamos a hacer los cuatro, uno por uno, en cada enunciado.

1. **Subrayá los sustantivos.** Cada sustantivo con datos y comportamiento es candidato a clase.
2. **Marcá los verbos que te piden.** Eso va a ser un método público. Y ojo: **te piden pocos**, no todos los que se te ocurran.
3. **Cazá las frases delatoras.** Los enunciados los escribe alguien que **ya sabe qué patrón quiere**. Siempre se le escapa la frase.
4. **Contá los problemas.** Casi ningún parcial tiene *un* patrón. Tiene dos o tres, **cada uno resolviendo un problema distinto**. Separarlos es la mitad de la nota.

> El paso que casi nadie hace es el **4**, y es el que ordena todo. La primera pregunta **nunca** es *"¿cuál patrón?"* — es **"¿cuántos?"**.

---

## Lo que se corrige

Los cuatro parciales piden **lo mismo**, casi con las mismas palabras:

1. **Diagrama de clases completo.**
2. **Codificar los métodos necesarios** (siempre uno o dos puntuales, no todo el sistema).
3. **Un breve código de ejemplo** mostrando el uso.
4. **Justificar cada clase + SOLID + algún patrón + buen diseño POO.**

El **4** es el que más gente regala y el más barato del examen: son **cuatro renglones** y no hay que dibujar nada.

El **3** es el que más gente saltea. Es el más corto de todos. Y es el que te salva.

---

## Pará y pensá — ¿por qué el ejemplo de uso te salva?

Terminaste el diagrama y el código. Te quedan cinco minutos. ¿Para qué gastarlos en diez líneas de `console.log`?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> Porque **el ejemplo de uso es el único lugar donde aparece la clase que orquesta.**
>
> El diagrama muestra las piezas. El código muestra las piezas. El **ejemplo de uso** muestra **quién las llama y en qué orden** — y eso es lo que separa "aplicó el patrón" de "entendió el patrón".
>
> Y es **tu** punto flaco: en las tres entregas modelaste impecable lo que varía (las subclases, los productos, los estados) y se te escapó siempre **el que las usa**. El ejemplo de uso **te obliga a escribirlo**. Si al escribirlo no sabés quién llama a qué, encontraste tu propio error **antes que el corrector**.

---

# 1. TravelNow — el final real

---

## El enunciado

> **UTN BA – TUP – Programación II — Final Mayo 2026, TEMA 2 — PARTE PRÁCTICA**
>
> La empresa "TravelNow" ofrece paquetes turísticos a sus clientes. Existen distintos tipos de servicios turísticos: Excursiones (con duración y guia asignado) y Estadías (con cantidad de noches y tipo de alojamiento).
>
> Un cliente puede crear "Itinerarios" personalizados que contengan excursiones, estadias o incluso otros itinerarios (anidados). El sistema debe permitir calcular la duración total de cualquier itinerario armado por el usuario.
>
> Además, el sistema debe gestionar diferentes Tipos de Paquetes:
>
> Paquete Económico: Incluye alojamiento estándar y asistencia básica.
> Paquete Premium: Incluye alojamiento de lujo, traslados VIP y asistencia personalizada.
>
> El costo final del viaje varía según el tipo de paquete, y la empresa planea agregar nuevos tipos en el futuro (por ejemplo: Paquete Familiar o Paquete Aventura) sin modificar la lógica principal de cálculo.
>
> **Se pide:**
>
> - Realizar el diagrama de clases completo de la solución.
> - Codificar los métodos necesarios que permitan obtener la duración total de un itinerario.
> - Desarrollar un breve código de ejemplo mostrando el uso de la aplicación.
> - Justificar cada clase creada y explicar por qué la solución cumple principios SOLID, algún patrón de diseño y buenas prácticas de Programación Orientada a Objetos.

---

## Paso 1 — Subrayá los sustantivos

Leelo de nuevo y marcá **todos** los sustantivos que tengan datos o comportamiento. No pienses en patrones todavía. **Solo sustantivos.**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Los que están:** empresa, cliente, **paquete turístico**, **servicio turístico**, **excursión**, **duración**, **guía**, **estadía**, **noches**, **tipo de alojamiento**, **itinerario**, **tipo de paquete**, **costo**, viaje.
>
> **Ahora filtrá. Candidatos a clase de verdad:**
>
> | Sustantivo | ¿Clase? |
> |---|---|
> | **Excursión** | ✅ tiene datos (duración, guía) |
> | **Estadía** | ✅ tiene datos (noches, alojamiento) |
> | **Itinerario** | ✅ contiene cosas y sabe su duración |
> | **Servicio turístico** | ✅ **es lo que excursión y estadía tienen en común** ← el enunciado te lo nombra |
> | **Tipo de paquete** | ✅ Económico y Premium son dos |
> | **Guía** | ✅ pero es un dato, no el problema |
> | Empresa, cliente, usuario | ❌ no hacen nada acá |
>
> **El hallazgo del paso 1:** el enunciado dice *"Existen distintos tipos de **servicios turísticos**: Excursiones y Estadías"*. Te está **regalando la jerarquía**: hay un concepto padre y dos hijos. Eso no lo inventaste vos — está escrito.

---

## Paso 2 — Marcá los verbos que te piden

¿Qué te piden que el sistema **haga**? Ojo: son **dos**, y están en el enunciado y en el "se pide".

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Dos verbos. Nada más.**
>
> 1. *"El sistema debe permitir **calcular la duración total** de cualquier itinerario"* — y el "se pide" lo repite: *"codificar los métodos necesarios que permitan **obtener la duración total** de un itinerario"*. Cuando un verbo aparece **dos veces**, es **el** método.
> 2. *"El **costo final** del viaje varía según el tipo de paquete"* → **calcular el costo final**.
>
> **Lo que NO te piden** (y donde se pierde tiempo): reservar, pagar, listar itinerarios, gestionar clientes. Nada de eso está. **Si no te lo piden, no lo modeles.**
>
> **Y fijate el par:** `duracionTotal()` y `costoFinal()`. Dos verbos → probablemente **dos problemas**. Anotalo para el paso 4.

---

## Paso 3 — Cazá las frases delatoras

Ahora sí. Con la **ficha de los 13** al lado, buscá las frases donde al que escribió el enunciado **se le escapó el patrón**. Hay **dos**.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Frase 1:**
>
> > *"…que contengan excursiones, estadias **o incluso otros itinerarios (anidados)**"*
> > *"…calcular la duración total de **cualquier** itinerario"*
>
> Dos señales en dos renglones: un contenedor que contiene **cosas de su propio tipo**, a profundidad arbitraria (**un árbol**), y **la misma operación** sobre la hoja y sobre la rama (**tratamiento uniforme**).
>
> **Frase 2:**
>
> > *"…la empresa **planea agregar nuevos tipos en el futuro** (…) **sin modificar la lógica principal de cálculo**"*
>
> Esa frase no describe el negocio: **describe un principio de diseño**. "Agregar sin modificar" es **OCP dicho con todas las letras**. Nadie escribe eso salvo que quiera que uses un patrón.
>
> **La pista de que son dos distintas:** la frase 1 habla de **cómo se arma** el itinerario. La frase 2 habla de **cómo se calcula** el costo. **Distinto problema, distinta parte del enunciado, distinto patrón.**

---

## Paso 4 — ¿Cuántos problemas hay?

Con los tres pasos anteriores en la mano: **¿cuántas cosas distintas te están pidiendo?** No digas todavía qué patrón.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Dos, y no se tocan.**
>
> | # | El problema | ¿De qué tipo es? |
> |---|---|---|
> | 1 | Armar el itinerario y **medir su duración** | **Estructurar** |
> | 2 | **Costear** el viaje según el paquete | **Comportamiento** |
>
> Fijate cómo cayó solo: **dos verbos** (paso 2) → **dos frases delatoras** (paso 3) → **dos problemas**. El método se chequea a sí mismo.
>
> Uno es de **estructurar** y el otro de **comportamiento** → van a ser **dos patrones de familias distintas**, que conviven **sin pisarse** porque atacan cosas diferentes.
>
> **Por qué esto vale la mitad de la nota:** el que arranca preguntándose "¿qué patrón es?" se traba, porque **no hay un patrón**. El que cuenta primero, después elige tranquilo — dos veces, por separado.

---

## El enunciado, marcado

Así tendría que haber quedado tu hoja después de los cuatro pasos. Compará con la tuya.

> **negrita** = sustantivo candidato a clase · *cursiva* = verbo que te piden · 🔑 = frase delatora

---

> La empresa "TravelNow" ofrece paquetes turísticos a sus clientes. Existen distintos tipos de **servicios turísticos**: **Excursiones** (con duración y guia asignado) y **Estadías** (con cantidad de noches y tipo de alojamiento).
>
> Un cliente puede crear "**Itinerarios**" personalizados que 🔑 *contengan excursiones, estadias o incluso otros itinerarios (anidados)*. El sistema debe permitir *calcular la duración total* de 🔑 *cualquier itinerario* armado por el usuario.
>
> Además, el sistema debe gestionar diferentes **Tipos de Paquetes**:
>
> **Paquete Económico**: Incluye alojamiento estándar y asistencia básica.
> **Paquete Premium**: Incluye alojamiento de lujo, traslados VIP y asistencia personalizada.
>
> El costo final del viaje *varía* según el tipo de paquete, y la empresa 🔑 *planea agregar nuevos tipos en el futuro* (por ejemplo: Paquete Familiar o Paquete Aventura) 🔑 *sin modificar la lógica principal de cálculo*.

**Mirá lo que NO está marcado:** empresa, cliente, usuario, guía, alojamiento estándar, traslados VIP. **Marcar todo es no marcar nada.** Sobre 150 palabras, lo que decide el diagrama entero son **dos frases**.

---

## Razoná — Problema 1: la estructura del itinerario

> *"…que contengan excursiones, estadias o incluso otros itinerarios (anidados)"* + *"la duración total de cualquier itinerario"*

**¿Qué patrón, y por qué no el parecido?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Composite.** *Estructurar.*
>
> - *"o incluso otros itinerarios (**anidados**)"* → contiene **cosas de su propio tipo**, a **profundidad arbitraria** = **árbol**.
> - *"la duración total de **cualquier** itinerario"* → la **misma operación** sobre la hoja (una excursión) y la rama (un itinerario).
>
> **No es Decorator** —el sospechoso de siempre, porque los dos se componen de su propia interfaz—: un `Itinerario` contiene **muchos** hijos y **no les agrega nada**; los **agrupa**. Decorator envuelve **uno** solo para **sumarle** comportamiento. **Composite arma un árbol; Decorator, una pila.**
>
> El premio: `duracionTotal()` sale **recursiva y sin un solo `if`**. Nunca preguntás si el item es una excursión o un itinerario — le pedís la duración y él sabe.

---

## Razoná — Problema 2: el costo del paquete

> *"El costo final del viaje varía según el tipo de paquete, y la empresa planea agregar nuevos tipos en el futuro sin modificar la lógica principal de cálculo."*

**¿Qué patrón? Cuidado, que acá hay dos candidatos buenos y uno es una trampa.**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Strategy.** *Comportamiento.*
>
> - *"el **costo final** varía **según el tipo**"* → un **cálculo** con varias formas.
> - *"agregar nuevos tipos **sin modificar** la lógica principal"* → **OCP**. `PaqueteFamiliar` mañana = **una clase nueva, cero cambios**.
>
> **La trampa: ¿no será Abstract Factory?** Mirá: *"Económico incluye alojamiento estándar y asistencia básica; Premium incluye alojamiento de lujo, traslados VIP y asistencia personalizada"*. Eso **suena** a una **familia coherente** de productos que no se mezclan (alojamiento + traslado + asistencia), que es la firma de Abstract Factory.
>
> **Lo que desempata es el verbo del paso 2.** Te piden **calcular** el costo final — no **crear** los componentes. El problema es *"¿**cómo** calculo?"* → **Strategy**. Si el enunciado dijera *"crear el alojamiento, el traslado y la asistencia correspondientes, sin mezclar un traslado VIP con asistencia básica"* → ahí sí, **Abstract Factory**.
>
> **La lección, y sirve para los cuatro parciales:** el mismo sustantivo ("paquete") es **Strategy o Abstract Factory según el verbo**. *Calcular* → Strategy. *Crear una familia* → Abstract Factory. **No mires el sustantivo: mirá qué te piden hacer con él.**
>
> Por eso el **paso 2** va antes que el **paso 3**. Sin el verbo, no podés desempatar.

---

## Predecí — ¿cuánto da esta duración?

```typescript
const bariloche = new Itinerario();
bariloche.agregar(new Estadia(5, "Hotel"));        // 5 días
bariloche.agregar(new Excursion(1, guiaAna));      // 1 día

const excursionesDelSur = new Itinerario();        // ← un itinerario…
excursionesDelSur.agregar(new Excursion(2, guiaPablo));
excursionesDelSur.agregar(new Excursion(1, guiaLucia));

bariloche.agregar(excursionesDelSur);              // ← …adentro de otro

console.log(bariloche.duracionEnDias());
```

**¿Qué imprime? Y más importante: ¿cuántos `if` hicieron falta?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Imprime `9`.** → `5 + 1 + (2 + 1)`
>
> **Y hicieron falta CERO `if`.** Ese es todo el punto del Composite.
>
> `bariloche.duracionEnDias()` recorre sus tres items y le pide la duración a cada uno **sin preguntarle qué es**. Los dos primeros son hojas y devuelven su número. El tercero es **otro itinerario**, que hace exactamente lo mismo con sus propios hijos. **La recursión no la escribiste vos: sale sola de la estructura.**
>
> Si en tu solución aparece un `if (item instanceof Itinerario)`, **no hiciste Composite**: hiciste un `if` con pasos extra.

---

## TravelNow — el diagrama

> [IMAGEN: diagrama de clases UML del sistema TravelNow, organizado en DOS bloques rotulados con colores de fondo distintos para que se lea que son dos patrones separados que colaboran.
>
> BLOQUE IZQUIERDO, rotulado «Composite — la estructura del itinerario»: arriba, la interfaz «interface» "ServicioTuristico" con el único método "+ duracionEnDias(): number". Debajo, colgando de ella por realización (línea punteada con triángulo vacío apuntando hacia arriba), tres clases: "Excursion" (con los atributos privados "- dias: number" y "- guia: Guia", y el método "+ duracionEnDias(): number"), "Estadia" (con "- noches: number" y "- alojamiento: TipoAlojamiento", y "+ duracionEnDias(): number") e "Itinerario" (con "- items: ServicioTuristico[]", y los métodos "+ agregar(s: ServicioTuristico)", "+ quitar(s: ServicioTuristico)" y "+ duracionEnDias(): number"). LA RELACIÓN CLAVE: desde "Itinerario" sale además una flecha de AGREGACIÓN (rombo vacío del lado de Itinerario) que sube y apunta a la interfaz "ServicioTuristico", etiquetada con la multiplicidad "items *". Resaltar esa flecha (más gruesa o en color) y ponerle una nota al costado: "acá está el patrón: el contenedor guarda cosas del tipo de la interfaz — por eso un itinerario puede contener otro itinerario". Al costado del bloque, un mini-esquema de árbol ilustrativo: un nodo "Itinerario Bariloche" del que cuelgan "Estadia 5d", "Excursion 1d" y otro nodo "Itinerario Excursiones del Sur", del que a su vez cuelgan "Excursion 2d" y "Excursion 1d"; abajo del árbol, el cálculo "5 + 1 + (2 + 1) = 9".
>
> BLOQUE DERECHO, rotulado «Strategy — el costo según el paquete»: la interfaz «interface» "TipoPaquete" con "+ calcularCosto(itinerario: ServicioTuristico): number", y debajo, por realización, las clases "PaqueteEconomico" y "PaquetePremium", cada una con su "+ calcularCosto(...)". Al costado de las dos, una caja punteada en gris claro rotulada "PaqueteFamiliar / PaqueteAventura (futuros)" conectada con línea punteada a la interfaz, con la nota "OCP: agregarlos es UNA CLASE NUEVA y cero cambios en Viaje".
>
> EN EL CENTRO, uniendo los dos bloques, la clase "Viaje" (EL ORQUESTADOR — dibujarla destacada, con borde más grueso): atributos "- itinerario: ServicioTuristico" y "- tipo: TipoPaquete"; métodos "+ costoFinal(): number", "+ duracionTotal(): number" y "+ cambiarPaquete(t: TipoPaquete)". De "Viaje" salen DOS flechas: una de composición (rombo relleno) hacia la interfaz "ServicioTuristico" del bloque izquierdo, etiquetada "itinerario 1", y otra de composición (rombo relleno) hacia la interfaz "TipoPaquete" del bloque derecho, etiquetada "tipo 1". Poner un cartel sobre "Viaje" que diga: "la clase que USA a las otras dos — sin esta, no se ve el patrón".
>
> Nota al pie del diagrama: "Viaje depende de DOS interfaces, de ninguna clase concreta → DIP".]

---

## TravelNow — el Composite

Lo que el parcial pide codificar: **la duración total del itinerario**.

```typescript
// El COMPONENTE: lo único que hoja y rama tienen en común.
interface ServicioTuristico {
  duracionEnDias(): number;
}

// HOJA 1
class Excursion implements ServicioTuristico {
  constructor(
    private readonly dias: number,
    private readonly guia: Guia,
  ) {}

  duracionEnDias(): number {
    return this.dias;
  }
}

// HOJA 2
class Estadia implements ServicioTuristico {
  constructor(
    private readonly noches: number,
    private readonly alojamiento: TipoAlojamiento,
  ) {}

  duracionEnDias(): number {
    return this.noches;
  }
}

// COMPUESTO: es un ServicioTuristico Y contiene ServicioTuristico.
class Itinerario implements ServicioTuristico {
  private readonly items: ServicioTuristico[] = [];

  agregar(servicio: ServicioTuristico): void {
    this.items.push(servicio);
  }

  quitar(servicio: ServicioTuristico): void {
    const i = this.items.indexOf(servicio);
    if (i >= 0) this.items.splice(i, 1);
  }

  // La recursión sale sola: si el item es otro Itinerario, se repite.
  duracionEnDias(): number {
    return this.items.reduce((total, item) => total + item.duracionEnDias(), 0);
  }
}
```

**Los dos renglones que hacen el patrón:** `class Itinerario implements ServicioTuristico` y `private items: ServicioTuristico[]`. **Es** un servicio y **tiene** servicios. Esa doble relación es la firma — la misma que ya clavaste dos veces con Decorator, pero acá con **muchos** hijos en vez de uno.

---

## TravelNow — el Strategy y el orquestador

```typescript
// La ESTRATEGIA: la familia de algoritmos de costeo.
interface TipoPaquete {
  calcularCosto(itinerario: ServicioTuristico): number;
}

class PaqueteEconomico implements TipoPaquete {
  calcularCosto(itinerario: ServicioTuristico): number {
    return itinerario.duracionEnDias() * 100;      // alojamiento estándar
  }
}

class PaquetePremium implements TipoPaquete {
  calcularCosto(itinerario: ServicioTuristico): number {
    return itinerario.duracionEnDias() * 250 + 5000; // lujo + traslados VIP
  }
}

// EL CONTEXTO: la clase que usa a las otras dos.
class Viaje {
  constructor(
    private readonly itinerario: ServicioTuristico,
    private tipo: TipoPaquete,
  ) {}

  duracionTotal(): number {
    return this.itinerario.duracionEnDias();
  }

  costoFinal(): number {
    return this.tipo.calcularCosto(this.itinerario);   // delega: cero ifs
  }

  cambiarPaquete(tipo: TipoPaquete): void {
    this.tipo = tipo;                                   // intercambiable en runtime
  }
}
```

> **`Viaje` es la clase que en el parcial casi nadie dibuja — y es la que demuestra que entendiste.** Sin ella, el corrector ve una interfaz con dos implementaciones y no ve **quién elige**. `costoFinal()` **delega y no tiene un solo `if`**: eso es lo que hay que poder señalar con el dedo.

---

## TravelNow — el ejemplo de uso

Diez líneas. El punto 3 del "se pide", el que casi nadie entrega.

```typescript
// Armamos el itinerario, anidado.
const bariloche = new Itinerario();
bariloche.agregar(new Estadia(5, TipoAlojamiento.Hotel));
bariloche.agregar(new Excursion(1, new Guia("Ana")));

const excursionesDelSur = new Itinerario();
excursionesDelSur.agregar(new Excursion(2, new Guia("Pablo")));
excursionesDelSur.agregar(new Excursion(1, new Guia("Lucía")));
bariloche.agregar(excursionesDelSur);          // un itinerario dentro de otro

console.log(bariloche.duracionEnDias());       // 9

// Lo costeamos, y cambiamos de paquete sin tocar el itinerario.
const viaje = new Viaje(bariloche, new PaqueteEconomico());
console.log(viaje.costoFinal());               // 900

viaje.cambiarPaquete(new PaquetePremium());
console.log(viaje.costoFinal());               // 7250
```

**Las dos últimas líneas valen oro:** muestran el Strategy **funcionando en runtime** —el mismo itinerario, otro costo, sin tocar una sola clase— que es exactamente lo que el enunciado pedía con *"agregar nuevos tipos sin modificar la lógica principal de cálculo"*.

---

## TravelNow — la justificación (el punto 4)

Cuatro renglones. Lo más barato del parcial:

**Las clases:**
- `ServicioTuristico` — el contrato común: todo lo que entre a un itinerario sabe decir su duración.
- `Excursion` / `Estadia` — las **hojas**: saben su propia duración.
- `Itinerario` — el **compuesto**: agrupa servicios (incluso otros itinerarios) y **suma** las duraciones de sus hijos.
- `TipoPaquete` + `PaqueteEconomico` / `PaquetePremium` — la familia de **algoritmos de costeo**, intercambiables.
- `Viaje` — el **contexto**: une un itinerario con un tipo de paquete y delega.

**Los patrones:**
- **Composite** — para que un itinerario pueda contener otros itinerarios y `duracionEnDias()` funcione igual sobre la hoja y sobre la rama, sin preguntar tipos.
- **Strategy** — para que el costo varíe según el paquete y se puedan agregar tipos nuevos sin tocar `Viaje`.

**SOLID:**
- **OCP** — *el que el enunciado pide con todas las letras.* `PaqueteFamiliar` = una clase nueva, **cero** modificaciones. Ídem un tipo de servicio nuevo.
- **DIP** — `Viaje` depende de las **interfaces** `ServicioTuristico` y `TipoPaquete`, nunca de las concretas.
- **LSP** — una `Excursion` y un `Itinerario` son intercambiables donde se espera un `ServicioTuristico`: por eso la recursión no rompe.
- **SRP** — cada clase una razón de cambio: la excursión sabe su duración, el itinerario sabe sumar, el paquete sabe costear, el viaje sabe coordinar.
- **ISP** — `ServicioTuristico` tiene **un** método. Nadie implementa nada que no use.

**Buen diseño POO:** cero `if` sobre tipos, atributos privados, comportamiento **junto a los datos** que necesita, y polimorfismo en vez de `switch`.

---

# 2. Heladería

---

## El enunciado

> **EJERCICIO HELADERÍA**
>
> Una heladería lanza su propia plataforma para pedir helados, la heladería vende por kilo, en envases de ¼ , ½ , 1k y postre helado.
>
> Si el cliente compra más de 1 artículo se hace un combo y se hace un descuento del 8% al total a pagar, se puede combinar de cualquier manera ej. 2 cuartos, 1 kilo y 1 cuarto, 1 medio y postre helado, etc.
>
> Se solicita un mecanismo que permita gestionar promociones estacionales, que modificarán el precio del combo segun la epoca:
>
> - Noce de helado artesanal: si el combo contiene al menos 1 postre helado se hace un 15% de descuento extra.
> - Vacaciones de invierno: si el combo tiene 4 o más artículos o si el precio total supera los $30.000 se hace un 10% de descuento.
>
> **se pide:**
>
> 1. Diagrama de clases completo de la solución.
> 2. codificar los métodos necesarios.
>     - a) Obtener el precio final de cualquier artículo y/o combo. Tener en cuenta las promos estacionales.
> 3. Desarrolle un breve código de ejemplo, mostrando el uso de la app.
> 4. Justificación de cada clase y de por qué la solución cumple principios solid, algún patrón de diseño y con el buen diseño de POO.

*(Transcripción textual. Sí, dice "Noce" y "segun la epoca". Los enunciados vienen así.)*

---

## Ejercicio en vivo — los cuatro pasos, ahora vos

**Diez minutos, el enunciado y el lápiz.** Los cuatro pasos, en orden, sin saltearte ninguno. Escribí las respuestas antes de dar vuelta la slide.

1. Sustantivos candidatos a clase.
2. Los verbos que te piden.
3. Las frases delatoras.
4. ¿Cuántos problemas?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Paso 1 — Sustantivos:** heladería, **artículo** (envase de ¼, ½, 1k, postre helado), **combo**, **promoción estacional** (Noche de helado artesanal, Vacaciones de invierno), precio, época, cliente.
> Candidatos reales: **Artículo**, **Combo**, **Promoción estacional**. *(Heladería, cliente y época no hacen nada.)*
>
> **Paso 2 — Verbos:** **uno solo**, y está en el punto 2a: *"**obtener el precio final** de cualquier artículo y/o combo, teniendo en cuenta las promos"*. Nada más. No te piden vender, cobrar, ni armar el pedido.
>
> **Paso 3 — Frases delatoras:**
> - *"el precio final de **cualquier artículo y/o combo**"* → **la misma operación** sobre la hoja y sobre la rama.
> - *"un **mecanismo** que permita **gestionar promociones** estacionales, que modificarán el precio **según la época**"* → algo que **varía** y que va a **crecer**.
>
> **Paso 4 — ¿Cuántos problemas?** **Dos:**
> 1. El precio de un artículo suelto y el de un combo, con **una sola operación** → **estructurar**.
> 2. Las promos que modifican ese precio según la época → **comportamiento**.
>
> **Fijate qué pasó:** un verbo, pero **dos frases delatoras** → **dos problemas**. Cuando eso no cierra (un verbo, dos frases), mirá bien: el verbo suele ser **el resultado de los dos problemas juntos**. `precioFinal()` es *el precio del combo* **más** *la promo aplicada*.

---

## El enunciado, marcado

> **negrita** = sustantivo candidato a clase · *cursiva* = verbo que te piden · 🔑 = frase delatora

---

> Una heladería lanza su propia plataforma para pedir helados, la heladería vende por kilo, en **envases** de ¼ , ½ , 1k y **postre helado**.
>
> Si el cliente compra más de 1 **artículo** se hace un **combo** y se hace un descuento del 8% al total a pagar, se puede combinar de cualquier manera ej. 2 cuartos, 1 kilo y 1 cuarto, 1 medio y postre helado, etc.
>
> Se solicita 🔑 *un mecanismo que permita gestionar promociones estacionales, que modificarán el precio del combo segun la epoca*:
>
> - **Noce de helado artesanal**: si el combo contiene *al menos 1* postre helado se hace un 15% de descuento *extra*.
> - **Vacaciones de invierno**: si el combo tiene 4 o más artículos o si el precio total supera los $30.000 se hace un 10% de descuento.
>
> se pide:
>
> 2. codificar los métodos necesarios.
>     - a) *Obtener el precio final* de 🔑 *cualquier artículo y/o combo*. Tener en cuenta las promos estacionales.

**Los dos detalles chicos que cambian todo:** *"al menos 1"* → es un `some`. *"extra"* → es la palabra que abre la discusión Strategy vs Decorator. **Dos palabras sueltas, dos decisiones de diseño.**

---

## Pará y pensá — ¿no te suena de algo?

Antes de elegir los patrones. Volvé al diagrama de TravelNow, mirá tus cuatro pasos de la Heladería, y decime qué ves.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Es el mismo parcial.** Cambiaron los sustantivos.
>
> | TravelNow | Heladería |
> |---|---|
> | `ServicioTuristico` (interfaz) | `ItemPedido` (interfaz) |
> | `Excursion` / `Estadia` (hojas) | `Articulo` (hoja) |
> | `Itinerario` (compuesto) | `Combo` (compuesto) |
> | `duracionEnDias()` recursiva | `precioBase()` recursiva |
> | *"la duración total de **cualquier** itinerario"* | *"el precio final de **cualquier** artículo **y/o** combo"* |
> | `TipoPaquete` (Strategy) | `PromocionEstacional` (Strategy) |
> | *"planea agregar nuevos tipos sin modificar"* | *"un **mecanismo** que permita **gestionar** promociones"* |
> | `Viaje` (contexto) | `Pedido` (contexto) |
>
> **Composite + Strategy + un contexto que los une.** Es un **molde**, y lo vas a ver otra vez, y otra.
>
> **Por qué esto es lo más importante de la clase:** no estudiás cuatro parciales, estudiás **tres o cuatro moldes**. Cuando en el examen leas *"el total de cualquier X"* y *"planea agregar"*, ya sabés cómo termina el diagrama antes de dibujar la primera caja.
>
> **Pero ojo con el atajo:** que sea el mismo molde **no te exime de los cuatro pasos**. Los moldes se reconocen *después* de leer, no en lugar de leer. Los dos que siguen tienen trampas que TravelNow no tenía.

---

## Razoná — la señal está, pero cambió de forma

En TravelNow la señal de Composite era gigante: *"o incluso otros itinerarios (anidados)"*. Acá **no dice eso en ningún lado**. **¿Sigue siendo Composite?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Sí, y la señal es esta:** *"obtener el precio final de **cualquier artículo y/o combo**"*.
>
> Ese **"y/o"** es todo. Te piden **una sola operación** que funcione igual sobre **un artículo suelto** y sobre **un combo**. Eso es **tratamiento uniforme de la hoja y la rama** — la definición de Composite, dicha sin la palabra "anidado".
>
> **El honesto:** acá el enunciado **no pide combos dentro de combos**. Un Composite sin anidación es un Composite "chato". ¿Está mal usarlo? No: te da el tratamiento uniforme que **sí** te piden, y si mañana quieren un "combo familiar" que agrupe combos, **ya funciona**.
>
> **Qué escribir:** *"Uso Composite porque me piden el precio final de cualquier artículo y/o combo con la misma operación. El enunciado no exige combos anidados, pero el patrón los soporta sin cambios."*
>
> **Declarar el supuesto suma:** muestra que leíste fino y **decidiste**, en vez de aplicar el patrón de memoria.

---

## Razoná — Las promos: ¿Strategy o Decorator?

La parte jugosa. El enunciado es **ambiguo**, y las dos lecturas se defienden:

> *"un mecanismo que permita gestionar promociones estacionales, que modificarán el precio del combo segun la epoca"*
> *"…se hace un 15% de descuento extra"*

**¿Cuál elegís, y qué tenés que escribir?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Strategy**, y el argumento es la palabra **"estacionales"**.
>
> "Noche de helado artesanal" y "Vacaciones de invierno" son **épocas distintas**: no son la misma noche. El enunciado dice *"según la **época**"* → **una promo vigente por vez**, que se **reemplaza** cuando cambia la temporada. Eso es Strategy: **intercambiar**, no acumular. Y *"gestionar promociones"* + promos futuras = **OCP**: una promo nueva es **una clase nueva**.
>
> **Por qué Decorator también se defiende:** la palabra *"**extra**"* sugiere que el 15% se apila sobre el 8% del combo. Si asumís que **dos promos pueden estar activas a la vez** (una noche artesanal *durante* las vacaciones de invierno), se **acumulan** → Decorator.
>
> **Lo que el corrector premia no es cuál elegiste: es que declares el supuesto.**
>
> *"Interpreto que las promociones son estacionales y por lo tanto **excluyentes** (una época a la vez), así que uso **Strategy**: la promo vigente se intercambia. Si el negocio necesitara **acumular** varias promos simultáneas, el patrón correcto sería **Decorator**, porque ahí se apilan."*
>
> Esa frase, sola, vale más que el diagrama. **Strategy intercambia; Decorator acumula.** Vos elegís, pero decí por qué.

---

## Razoná — ¿cómo sabe la promo si hay un postre?

`NocheDeHeladoArtesanal` necesita preguntar: *"¿este combo tiene al menos un postre?"*. `VacacionesDeInvierno` necesita: *"¿cuántos artículos tenés?"*.

**¿Dónde ponés esos métodos? Ojo que hay un costo escondido.**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Van en la interfaz `ItemPedido`** — y ese es **el trade-off del Composite**, el mismo que vimos con ISP en la Clase 4.
>
> ```typescript
> interface ItemPedido {
>   precioBase(): number;
>   cantidadDeArticulos(): number;
>   contienePostre(): boolean;
> }
> ```
>
> - `Combo.cantidadDeArticulos()` → **suma** la de sus hijos.
> - `Combo.contienePostre()` → `this.items.some(i => i.contienePostre())` ← **some**, que es la palabra *"al menos 1"* del enunciado.
> - `Articulo.cantidadDeArticulos()` → devuelve **1**.
> - `Articulo.contienePostre()` → devuelve si **él** es un postre.
>
> **El costo, que hay que decir:** `cantidadDeArticulos()` en un **artículo suelto** es medio ridículo — siempre vale 1. La interfaz creció para que el **compuesto** pueda responder, y la **hoja** paga el precio. **Eso tensiona ISP** y es el trade-off clásico de Composite: uniformidad a cambio de una interfaz más gorda.
>
> **La alternativa** sería que la promo pregunte con `instanceof`… y ahí perdés el patrón entero. **Mejor la interfaz un poco más gorda que un `if` sobre tipos.** Decir esta frase es lo que separa "aplicó el patrón" de "entendió el patrón".

---

## Heladería — el diagrama

> [IMAGEN: diagrama de clases UML de la heladería, con la MISMA disposición que el diagrama de TravelNow (dos bloques + un orquestador en el centro), para que se vea de un vistazo que es el mismo molde. Agregar arriba del todo un cartel que diga "mismo esqueleto que TravelNow".
>
> BLOQUE IZQUIERDO, rotulado «Composite — artículo y combo»: la interfaz «interface» "ItemPedido" con tres métodos: "+ precioBase(): number", "+ cantidadDeArticulos(): number" y "+ contienePostre(): boolean". Debajo, por realización (punteada, triángulo vacío), dos clases: "Articulo" (hoja) con "- tipo: TipoEnvase", "- precioUnitario: number" y los tres métodos; y "Combo" (compuesto) con "- items: ItemPedido[]", "+ agregar(i: ItemPedido)" y los tres métodos. Desde "Combo" sale una flecha de AGREGACIÓN (rombo vacío del lado de Combo) hacia la interfaz "ItemPedido", con multiplicidad "items *" — resaltarla igual que en el diagrama de TravelNow. Al costado de "Combo", una nota: "precioBase() = suma de los hijos × 0.92 (el 8% del combo vive acá)". Al costado de la interfaz, una nota de advertencia en otro color: "contienePostre() y cantidadDeArticulos() están acá para que el COMBO pueda responder — el Articulo las paga aunque casi no las use. Ese es el costo del Composite (tensiona ISP)".
>
> BLOQUE DERECHO, rotulado «Strategy — las promos estacionales»: la interfaz «interface» "PromocionEstacional" con "+ aplicar(item: ItemPedido): number"; debajo, por realización, tres clases: "SinPromocion", "NocheDeHeladoArtesanal" (nota: "si contienePostre() → 15%") y "VacacionesDeInvierno" (nota: "si cantidadDeArticulos() >= 4 O precioBase() > 30000 → 10%"). Al costado, una caja punteada gris "promos futuras" con la nota "OCP: una promo nueva = una clase nueva".
>
> EN EL CENTRO, la clase "Pedido" (EL ORQUESTADOR, con borde grueso): "- item: ItemPedido", "- promo: PromocionEstacional", "+ precioFinal(): number", "+ cambiarPromocion(p: PromocionEstacional)". Dos flechas de composición (rombo relleno) desde "Pedido": una a "ItemPedido" y otra a "PromocionEstacional".
>
> ABAJO, una franja comparativa de dos columnas que muestre el paralelo con el parcial anterior: columna izquierda "TravelNow: ServicioTuristico → Itinerario → TipoPaquete → Viaje"; columna derecha "Heladería: ItemPedido → Combo → PromocionEstacional → Pedido"; con flechas de equivalencia entre las filas.]

---

## Heladería — el Composite

```typescript
enum TipoEnvase { Cuarto, Medio, Kilo, PostreHelado }

interface ItemPedido {
  precioBase(): number;
  cantidadDeArticulos(): number;
  contienePostre(): boolean;
}

// HOJA
class Articulo implements ItemPedido {
  constructor(
    private readonly tipo: TipoEnvase,
    private readonly precioUnitario: number,
  ) {}

  precioBase(): number { return this.precioUnitario; }
  cantidadDeArticulos(): number { return 1; }
  contienePostre(): boolean { return this.tipo === TipoEnvase.PostreHelado; }
}

// COMPUESTO
class Combo implements ItemPedido {
  private readonly items: ItemPedido[] = [];

  agregar(item: ItemPedido): void { this.items.push(item); }

  // El 8% del combo vive acá: es la regla del compuesto, no de la promo.
  precioBase(): number {
    const suma = this.items.reduce((t, i) => t + i.precioBase(), 0);
    return suma * 0.92;
  }

  cantidadDeArticulos(): number {
    return this.items.reduce((t, i) => t + i.cantidadDeArticulos(), 0);
  }

  contienePostre(): boolean {
    return this.items.some(i => i.contienePostre());   // "al menos 1" → some
  }
}
```

**Ojo con dónde pusimos el 8%:** vive en `Combo.precioBase()`, **no** en las promos. ¿Por qué? Porque *"si compra más de 1 artículo se hace un combo y se hace un descuento del 8%"* — eso **no es una promoción estacional**, es **lo que significa ser un combo**. Cada regla en la clase que la posee: eso es **SRP**.

---

## Heladería — el Strategy y el orquestador

```typescript
interface PromocionEstacional {
  aplicar(item: ItemPedido): number;
}

// Fuera de temporada: el precio no se toca. Evita el `if (promo != null)`.
class SinPromocion implements PromocionEstacional {
  aplicar(item: ItemPedido): number {
    return item.precioBase();
  }
}

class NocheDeHeladoArtesanal implements PromocionEstacional {
  aplicar(item: ItemPedido): number {
    const base = item.precioBase();
    return item.contienePostre() ? base * 0.85 : base;
  }
}

class VacacionesDeInvierno implements PromocionEstacional {
  aplicar(item: ItemPedido): number {
    const base = item.precioBase();
    const aplica = item.cantidadDeArticulos() >= 4 || base > 30000;
    return aplica ? base * 0.90 : base;
  }
}

// EL CONTEXTO
class Pedido {
  constructor(
    private readonly item: ItemPedido,
    private promo: PromocionEstacional = new SinPromocion(),
  ) {}

  precioFinal(): number {
    return this.promo.aplicar(this.item);
  }

  cambiarPromocion(promo: PromocionEstacional): void {
    this.promo = promo;
  }
}
```

**`SinPromocion` es un detalle que suma:** una estrategia que "no hace nada" te ahorra el `if (this.promo != null)` en `precioFinal()`. **Siempre hay una promo; a veces es la que no descuenta.**

> **Supuesto a declarar:** *"el precio total supera los $30.000"* lo interpreto **después** del 8% del combo (o sea, sobre `precioBase()`). El enunciado no lo aclara — **decilo y seguí**.

---

## Heladería — ejemplo de uso y justificación

```typescript
const combo = new Combo();
combo.agregar(new Articulo(TipoEnvase.Kilo, 18000));
combo.agregar(new Articulo(TipoEnvase.Cuarto, 6000));
combo.agregar(new Articulo(TipoEnvase.PostreHelado, 9000));

console.log(combo.precioBase());        // (18000+6000+9000) * 0.92 = 30360

// Un artículo suelto: MISMA interfaz, sin combo ni descuento.
const suelto: ItemPedido = new Articulo(TipoEnvase.Medio, 10000);
console.log(new Pedido(suelto).precioFinal());   // 10000

// El mismo combo, según la época:
console.log(new Pedido(combo, new NocheDeHeladoArtesanal()).precioFinal());
// tiene postre → 30360 * 0.85 = 25806

console.log(new Pedido(combo, new VacacionesDeInvierno()).precioFinal());
// 3 artículos, pero 30360 > 30000 → 30360 * 0.90 = 27324
```

**La línea del `suelto` vale doble:** demuestra el Composite en acción — **el mismo `Pedido`, la misma interfaz**, funcionando con una hoja en vez de una rama, sin una línea de código distinta. Eso es *"el precio final de cualquier artículo **y/o** combo"*.

**Justificación (los renglones que hay que escribir):**
- **Composite** — precio final de cualquier artículo y/o combo con **la misma operación**, sin `if` sobre tipos.
- **Strategy** — las promos estacionales se **intercambian** según la época; una promo nueva es **una clase nueva** (**OCP**).
- **OCP** — promo nueva ✅, envase nuevo ✅: cero cambios en `Pedido`.
- **DIP** — `Pedido` depende de `ItemPedido` y `PromocionEstacional`, nunca de las concretas.
- **LSP** — `Articulo` y `Combo` son intercambiables donde se espera un `ItemPedido`.
- **SRP** — el 8% es del `Combo` (es lo que significa ser combo); los descuentos de época son de las promos. Cada regla en su clase.
- **ISP** — *"la interfaz creció (`contienePostre`) para que el compuesto pueda responder; es el trade-off aceptado del Composite: prefiero la interfaz un poco más gorda antes que un `instanceof` en las promos."*

---

# 3. Reservas

---

## El enunciado

> **Ejercicio Reservas**
>
> Se desea implementar un sistema para administrar las reservas de habitaciones del hotel Grand Splendid. El hotel cuenta con habitaciones de distinta categoría, a saber:
>
> - Simple: $100 por día.
> - Doble: $250 por día.
> - Suite: $500 por día.
>
> De cada habitación se conoce la cantidad máxima de personas que pueden ocuparla, su precio y los regímenes alimenticios disponibles según la habitación. Al día de hoy, el hotel sólo maneja los regímenes:
>
> - SA: sólo alojamiento. No incrementa el valor de la habitación.
> - MP: media pensión. Es un 25% al valor de la habitación.
> - PC: pensión completa. Es un 75% al valor de la habitación.
>
> Es importante tener en cuenta que el hotel planea agregar nuevos regímenes en un futuro.
>
> Para calcular el precio de una habitación, el hotel diferencia entre temporada alta y temporada baja.
>
> | Temporada Alta | Temporada Baja |
> |---|---|
> | `(precio de la habitación + régimen)* días * 1.20` | `(precio de la habitación + régimen / 2)* días * 1.10` |
>
> La aplicación deberá permitir crear, eliminar y listar reservas.
>
> Una reserva posee fecha de ingreso, fecha de egreso, cliente, habitaciones reservadas y precio.
>
> Al momento de generar una reserva, el sistema deberá verificar que todas las habitaciones seleccionadas estén disponibles.
>
> Una habitación sólo puede estar disponible o reservada.
>
> Si se intenta reservar una habitación y la misma se encuentra reservada, la operación deberá arrojar una excepción informando del error.
>
> El precio de la reserva se obtiene sumando el precio de todas las habitaciones que posee la reserva.
>
> Se asume que un usuario selecciona, de un listado, las habitaciones que quiere reservar y luego intenta realizar la reserva.
>
> **Se pide:**
>
> 1. Elabore un diagrama de clases adecuado para la resolución de este problema.
> 2. Implemente los métodos necesarios para realizar una reserva.
> 3. Implemente un mecanismo que permita a un usuario del sistema cambiar de forma automática de temporada.
> 4. En caso de haber implementado algún patrón de diseño Indique cuál, y qué requerimiento intenta resolver con el mismo.

---

## Ejercicio en vivo — los cuatro pasos

**Diez minutos.** Otra vez los cuatro, en orden. Este enunciado es más largo y tiene más ruido: **filtrar es parte del ejercicio**.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Paso 1 — Sustantivos:** hotel, **habitación** (Simple/Doble/Suite), **régimen** (SA/MP/PC), **temporada** (alta/baja), **reserva**, cliente, precio, días, listado, usuario.
> Candidatos reales: **Habitación**, **Régimen**, **Temporada**, **Reserva**, **Cliente**.
> ⚠️ **Ojo con "Simple / Doble / Suite":** parecen tres clases. Volvemos a esto en dos slides.
>
> **Paso 2 — Verbos:** el "se pide" es explícito:
> - *"los métodos necesarios para **realizar una reserva**"* ← **el** método.
> - *"un mecanismo para **cambiar de forma automática de temporada**"*.
> - Y del cuerpo: *"**crear, eliminar y listar** reservas"*, *"**verificar** que todas estén disponibles"*, *"el precio se obtiene **sumando**"*.
>
> **Paso 3 — Frases delatoras:** hay **tres**, y cada una en un párrafo distinto.
> - *"el hotel **planea agregar nuevos regímenes** en un futuro"*
> - *"un mecanismo que permita **cambiar** de forma automática de temporada"* + dos fórmulas distintas
> - *"una habitación **sólo puede estar disponible o reservada**"* + *"si se intenta reservar una reservada → **excepción**"*
>
> **Paso 4 — ¿Cuántos problemas?** **Tres.** Y el enunciado te lo confirma en el punto 4: *"indique cuál, y **qué requerimiento** intenta resolver"* — o sea, **te está pidiendo el mapeo patrón → requerimiento como entregable**.
>
> **La diferencia con los dos anteriores:** acá los tres problemas **no están juntos**. Están desparramados en párrafos separados, mezclados con ruido (el listado, el usuario, las fechas). Por eso el paso 4 acá vale doble: **contarlos es el ejercicio**.

---

## El enunciado, marcado

> **negrita** = sustantivo candidato a clase · *cursiva* = verbo que te piden · 🔑 = frase delatora

---

> Se desea implementar un sistema para administrar las **reservas** de **habitaciones** del hotel Grand Splendid. El hotel cuenta con habitaciones de distinta categoría, a saber: Simple: $100 por día. Doble: $250 por día. Suite: $500 por día.
>
> De cada habitación se conoce la cantidad máxima de personas que pueden ocuparla, su precio y los **regímenes** alimenticios disponibles según la habitación. (…SA, MP, PC…)
>
> Es importante tener en cuenta que 🔑 *el hotel planea agregar nuevos regímenes en un futuro*.
>
> Para calcular el precio de una habitación, el hotel diferencia entre **temporada alta** y **temporada baja**. (…las dos fórmulas…)
>
> La aplicación deberá permitir *crear, eliminar y listar* reservas.
>
> Una reserva posee fecha de ingreso, fecha de egreso, **cliente**, habitaciones reservadas y precio.
>
> Al momento de *generar una reserva*, el sistema deberá *verificar* que 🔑 *todas* las habitaciones seleccionadas estén disponibles.
>
> 🔑 *Una habitación sólo puede estar disponible o reservada.*
>
> 🔑 *Si se intenta reservar una habitación y la misma se encuentra reservada, la operación deberá arrojar una excepción* informando del error.
>
> El precio de la reserva se obtiene *sumando* el precio de todas las habitaciones que posee la reserva.
>
> Se pide: (…) 3. Implemente un mecanismo que permita a un usuario del sistema 🔑 *cambiar de forma automática de temporada*.

**Dos cosas para ver acá:**

1. **"Simple / Doble / Suite" NO están marcados como clase.** Vienen en la primera línea, parecen las estrellas del enunciado, y no lo son. La slide que sigue explica por qué.
2. **El "todas" está marcado como frase delatora** y no delata ningún patrón — delata un **bug**. No todo lo importante de un enunciado es un patrón.

---

## Razoná — Simple, Doble y Suite: ¿tres clases?

En el paso 1 quedaron marcadas. Casi todos dibujan `HabitacionSimple`, `HabitacionDoble` y `HabitacionSuite` heredando de `Habitacion`. **¿Está bien?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **No hace falta, y decir por qué te suma más que dibujarlas.**
>
> Preguntate: **¿qué hace distinto una Suite de una Simple?** Leé el enunciado otra vez… **nada**. Solo tienen **valores** distintos: otro precio, otra cantidad máxima de personas, otra lista de regímenes. **El comportamiento es idéntico.**
>
> **La regla:** la herencia se justifica cuando cambia el **comportamiento**, no cuando cambian los **datos**. Tres subclases que solo difieren en el valor del constructor son **tres clases vacías** — ruido.
>
> ```typescript
> const simple = new Habitacion(100, 1, [sa, mp]);
> const doble  = new Habitacion(250, 2, [sa, mp, pc]);
> const suite  = new Habitacion(500, 4, [sa, mp, pc]);
> ```
>
> **Cuándo SÍ heredarías:** si la Suite calculara el precio distinto, o tuviera un método que las otras no. Ahí hay **polimorfismo** y la herencia se paga sola. *(Spoiler del parcial 4: en Trenes, los vagones **sí** se heredan. Fijate por qué.)*
>
> **Qué escribir:** *"Modelo la categoría como datos (precio, máximo de personas, regímenes disponibles) y no como subclases, porque las tres categorías **no tienen comportamiento diferenciado**. Si en el futuro una categoría calculara distinto, ahí sí extendería `Habitacion`."*
>
> Eso es **criterio POO**, que es literalmente lo que el punto 4 evalúa.
>
> ⚠️ Si el docente quiere ver herencia igual, dibujarla no te baja la nota. Pero **la frase de arriba sí te la sube**.

---

## Razoná — Temporada: ¿Strategy o Template Method?

Mirá las dos fórmulas del enunciado:

```
Alta:  (precio + régimen)     * días * 1.20
Baja:  (precio + régimen / 2) * días * 1.10
```

**Mismo esqueleto, cambian dos cositas. ¿Eso no es Template Method?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Es Strategy, y lo desempata una sola palabra del paso 2: "cambiar".**
>
> Tenés razón en que la **forma** de las fórmulas es idéntica —esqueleto fijo, dos pasos que varían—, y eso **huele** a Template Method. Pero mirá el verbo que marcaste:
>
> > *"Implemente un mecanismo que permita a un usuario del sistema **cambiar** de forma automática de temporada."*
>
> **Cambiar** = en **runtime**, con el sistema andando. Template Method te obliga a **elegir la subclase** al construir el objeto: para "cambiar de temporada" tendrías que **crear otro objeto**. Strategy te deja hacer `hotel.cambiarTemporada(new TemporadaAlta())` y listo.
>
> **El desempate, en una línea:** **Template Method usa herencia** (decidís eligiendo la subclase, en compilación); **Strategy usa composición** (decidís cambiando el objeto, en runtime). *"¿Necesito cambiarlo con el programa andando?"* → **Strategy**.
>
> **Y lo de "automática":** el enunciado no aclara cómo. Lo más defendible es que la temporada **se deduzca de la fecha**:
>
> ```typescript
> class CalendarioDeTemporadas {
>   temporadaPara(fecha: Date): Temporada {
>     return this.esAlta(fecha) ? new TemporadaAlta() : new TemporadaBaja();
>   }
> }
> ```
>
> Eso es un **Factory Method** chiquito al servicio del Strategy: **decide cuál**, y el Strategy **hace el cálculo**. Declarás el supuesto (*"interpreto 'automática' como que se determina por la fecha de la reserva"*) y quedás impecable.
>
> **Fijate de nuevo:** el desempate salió del **verbo** (paso 2), no del sustantivo. Igual que en TravelNow.

---

## Predecí — el bug que se come medio punto

El enunciado dice: *"verificar que **todas** las habitaciones seleccionadas estén disponibles"* y *"si se intenta reservar una reservada, arrojar una excepción"*. Alguien lo implementa así:

```typescript
confirmar(): void {
  this.habitaciones.forEach(h => {
    if (!h.estaDisponible()) {
      throw new HabitacionNoDisponibleError(h);
    }
    h.reservar();
  });
}
```

**El cliente elige 3 habitaciones. La 1 y la 2 están libres, la 3 está ocupada. ¿Qué pasa?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Quedan reservadas la 1 y la 2, y después explota.** El cliente se va sin reserva… **y con dos habitaciones bloqueadas que nadie más puede alquilar**. Estado inconsistente.
>
> El `forEach` **verifica y reserva en la misma pasada**: para cuando descubre el problema en la habitación 3, ya modificó las dos anteriores, y la excepción se lleva puesto todo sin deshacerlas.
>
> **Y el enunciado te lo avisó:** dice *"verificar que **TODAS** las habitaciones estén disponibles"*. Ese **"todas"** significa **antes de tocar ninguna**. Son **dos pasadas**:
>
> ```typescript
> confirmar(): void {
>   // 1) Verificar TODAS primero. Todavía no tocamos nada.
>   const ocupadas = this.habitaciones.filter(h => !h.estaDisponible());
>   if (ocupadas.length > 0) {
>     throw new HabitacionNoDisponibleError(ocupadas);
>   }
>   // 2) Recién ahora, con la certeza de que están todas libres, reservamos.
>   this.habitaciones.forEach(h => h.reservar());
> }
> ```
>
> **Verificar todo → después mutar todo.** Nunca mezcles las dos pasadas cuando hay una excepción de por medio.
>
> Este detalle separa un 8 de un 10 y **no requiere saber ningún patrón**: requiere leer la palabra "todas". Por eso el **paso 2** dice *marcá los verbos*: `verificar` y `reservar` son **dos**, y el orden importa.

---

## Reservas — el diagrama

> [IMAGEN: diagrama de clases UML del sistema de reservas del hotel, organizado en TRES bloques rotulados con colores de fondo distintos (uno por patrón) más las clases de dominio en el centro.
>
> CENTRO — dominio: la clase "Reserva" con atributos "- ingreso: Date", "- egreso: Date", "- cliente: Cliente", "- habitaciones: Habitacion[]" y métodos "+ confirmar()", "+ precio(temporada: Temporada): number", "+ cantidadDeDias(): number". Una flecha de agregación (rombo vacío del lado de Reserva) hacia la clase "Habitacion" con multiplicidad "*", y una asociación simple hacia "Cliente" con multiplicidad "1". A la derecha de "Reserva", la clase "Habitacion" con atributos "- precioPorDia: number", "- maxPersonas: number", "- regimenesDisponibles: Regimen[]", "- estado: EstadoHabitacion" y métodos "+ reservar()", "+ liberar()", "+ estaDisponible(): boolean", "+ cambiarEstado(e: EstadoHabitacion)" y "+ precioPara(t: Temporada, r: Regimen, dias: number): number". Poner una nota destacada al costado de "Habitacion": "UNA sola clase, NO tres subclases: Simple/Doble/Suite solo cambian los VALORES, no el comportamiento".
>
> BLOQUE 1 «State — disponible o reservada» (abajo a la derecha): la interfaz «interface» "EstadoHabitacion" con "+ reservar(h: Habitacion)", "+ liberar(h: Habitacion)" y "+ estaDisponible(): boolean". Desde "Habitacion" sale una flecha de composición (rombo relleno) hacia "EstadoHabitacion", etiquetada "estado 1". Debajo de la interfaz, por realización (punteada, triángulo vacío), dos clases: "Disponible" y "Reservada". Desde "Disponible" sale una flecha de dependencia punteada que vuelve a "Habitacion" etiquetada "cambiarEstado(new Reservada())"; desde "Reservada", otra flecha punteada roja etiquetada "reservar() → lanza HabitacionNoDisponibleError". Al costado, un mini-diagrama de transiciones: dos círculos "Disponible" y "Reservada" con flechas en ambos sentidos, la de ida etiquetada "reservar()" y la de vuelta "liberar()", y una X roja sobre un bucle en "Reservada" etiquetado "reservar() → EXCEPCIÓN".
>
> BLOQUE 2 «Strategy — el régimen» (abajo a la izquierda): la interfaz «interface» "Regimen" con "+ adicional(precioBase: number): number"; debajo, por realización, tres clases: "SoloAlojamiento" (nota: "0"), "MediaPension" (nota: "25%") y "PensionCompleta" (nota: "75%"). Una caja punteada gris al costado rotulada "regímenes futuros" con la nota "el enunciado dice literal: 'planea agregar nuevos regímenes' → OCP".
>
> BLOQUE 3 «Strategy — la temporada» (arriba a la izquierda): la interfaz «interface» "Temporada" con "+ calcular(precioHab: number, adicionalRegimen: number, dias: number): number"; debajo, por realización, "TemporadaAlta" (nota: "(p + r) * días * 1.20") y "TemporadaBaja" (nota: "(p + r/2) * días * 1.10"). Al costado, la clase "CalendarioDeTemporadas" con "+ temporadaPara(fecha: Date): Temporada" y una flecha de dependencia punteada «create» hacia la interfaz "Temporada", con la nota: "el 'cambio automático': un Factory Method chiquito que ELIGE la estrategia según la fecha".
>
> ARRIBA DEL TODO, la clase "SistemaDeReservas" (el orquestador, borde grueso) con "- reservas: Reserva[]" y los métodos "+ crearReserva(...)", "+ eliminarReserva(r: Reserva)" y "+ listarReservas(): Reserva[]", con una nota: "crear / eliminar / listar — los tres verbos que pide el enunciado". Flecha de agregación hacia "Reserva" con multiplicidad "*".]

---

## Reservas — los tres patrones en código

```typescript
// STRATEGY 1 — el régimen. "Planea agregar nuevos" → OCP.
interface Regimen {
  adicional(precioBase: number): number;
}
class SoloAlojamiento implements Regimen {
  adicional(precioBase: number): number { return 0; }
}
class MediaPension implements Regimen {
  adicional(precioBase: number): number { return precioBase * 0.25; }
}
class PensionCompleta implements Regimen {
  adicional(precioBase: number): number { return precioBase * 0.75; }
}

// STRATEGY 2 — la temporada. Intercambiable en runtime.
interface Temporada {
  calcular(precioHab: number, adicionalRegimen: number, dias: number): number;
}
class TemporadaAlta implements Temporada {
  calcular(p: number, r: number, dias: number): number {
    return (p + r) * dias * 1.20;
  }
}
class TemporadaBaja implements Temporada {
  calcular(p: number, r: number, dias: number): number {
    return (p + r / 2) * dias * 1.10;
  }
}

// STATE — disponible o reservada.
interface EstadoHabitacion {
  reservar(h: Habitacion): void;
  liberar(h: Habitacion): void;
  estaDisponible(): boolean;
}
class Disponible implements EstadoHabitacion {
  reservar(h: Habitacion): void { h.cambiarEstado(new Reservada()); }
  liberar(h: Habitacion): void { /* ya está libre: no hace nada */ }
  estaDisponible(): boolean { return true; }
}
class Reservada implements EstadoHabitacion {
  reservar(h: Habitacion): void {
    throw new HabitacionNoDisponibleError("La habitación ya está reservada");
  }
  liberar(h: Habitacion): void { h.cambiarEstado(new Disponible()); }
  estaDisponible(): boolean { return false; }
}
```

**La regla del enunciado —*"si se intenta reservar una reservada → excepción"*— vive en UNA línea: `Reservada.reservar()`.** No hay un solo `if` preguntando el estado.

---

## Reservas — la habitación y la reserva

```typescript
class Habitacion {
  private estado: EstadoHabitacion = new Disponible();

  constructor(
    private readonly precioPorDia: number,
    private readonly maxPersonas: number,
    private readonly regimenesDisponibles: Regimen[],
  ) {}

  reservar(): void { this.estado.reservar(this); }        // delega
  liberar(): void { this.estado.liberar(this); }          // delega
  estaDisponible(): boolean { return this.estado.estaDisponible(); }
  cambiarEstado(e: EstadoHabitacion): void { this.estado = e; }

  precioPara(temporada: Temporada, regimen: Regimen, dias: number): number {
    return temporada.calcular(this.precioPorDia, regimen.adicional(this.precioPorDia), dias);
  }
}

class Reserva {
  constructor(
    private readonly ingreso: Date,
    private readonly egreso: Date,
    private readonly cliente: Cliente,
    private readonly habitaciones: Habitacion[],
    private readonly regimen: Regimen,
  ) {}

  cantidadDeDias(): number {
    const ms = this.egreso.getTime() - this.ingreso.getTime();
    return Math.ceil(ms / (1000 * 60 * 60 * 24));
  }

  // Verificar TODAS → después reservar TODAS. Dos pasadas.
  confirmar(): void {
    const ocupadas = this.habitaciones.filter(h => !h.estaDisponible());
    if (ocupadas.length > 0) {
      throw new HabitacionNoDisponibleError(`${ocupadas.length} habitación/es ya reservada/s`);
    }
    this.habitaciones.forEach(h => h.reservar());
  }

  precio(temporada: Temporada): number {
    const dias = this.cantidadDeDias();
    return this.habitaciones.reduce(
      (total, h) => total + h.precioPara(temporada, this.regimen, dias), 0,
    );
  }
}
```

`Habitacion.reservar()` **delega y no tiene un `if`**: le pide al estado que decida. Y `Reserva.precio()` es *"el precio se obtiene sumando el precio de todas las habitaciones"* — el enunciado traducido literal a un `reduce`.

---

## Reservas — el sistema y el ejemplo de uso

```typescript
// El "cambio automático de temporada": elige la estrategia según la fecha.
class CalendarioDeTemporadas {
  temporadaPara(fecha: Date): Temporada {
    const mes = fecha.getMonth();
    const esAlta = [11, 0, 1, 6].includes(mes);   // dic, ene, feb, jul
    return esAlta ? new TemporadaAlta() : new TemporadaBaja();
  }
}

// EL ORQUESTADOR: crear, eliminar y listar — los tres verbos del enunciado.
class SistemaDeReservas {
  private readonly reservas: Reserva[] = [];
  constructor(private readonly calendario: CalendarioDeTemporadas) {}

  crearReserva(reserva: Reserva): Reserva {
    reserva.confirmar();               // tira excepción si alguna está ocupada
    this.reservas.push(reserva);
    return reserva;
  }

  eliminarReserva(reserva: Reserva): void {
    const i = this.reservas.indexOf(reserva);
    if (i >= 0) {
      this.reservas.splice(i, 1);
      reserva.liberarHabitaciones();   // devolverlas al estado Disponible
    }
  }

  listarReservas(): Reserva[] { return [...this.reservas]; }
}
```

```typescript
const sistema = new SistemaDeReservas(new CalendarioDeTemporadas());
const suite = new Habitacion(500, 4, [new SoloAlojamiento(), new PensionCompleta()]);

const ingreso = new Date("2026-01-10");
const reserva = new Reserva(ingreso, new Date("2026-01-15"), cliente, [suite], new PensionCompleta());

sistema.crearReserva(reserva);                              // ✅ la reserva
console.log(suite.estaDisponible());                        // false

const temporada = new CalendarioDeTemporadas().temporadaPara(ingreso);  // enero → Alta
console.log(reserva.precio(temporada));    // (500 + 375) * 5 * 1.20 = 5250

// Alguien intenta reservar la misma suite:
const otra = new Reserva(ingreso, new Date("2026-01-12"), otroCliente, [suite], new SoloAlojamiento());
sistema.crearReserva(otra);                // 💥 HabitacionNoDisponibleError
```

**La última línea es el ejemplo de uso más valioso del parcial:** demuestra la excepción que el enunciado pide **explícitamente**, en una sola línea.

---

## Reservas — la justificación

El punto 4 pregunta: *"indique cuál, y **qué requerimiento intenta resolver**"*. Un patrón sin su requerimiento no cuenta. Así se contesta:

| Patrón | Requerimiento **textual** que resuelve |
|---|---|
| **Strategy** (`Regimen`) | *"El hotel **planea agregar nuevos regímenes** en un futuro"* → un régimen nuevo es una clase nueva; nadie más se entera. **OCP puro.** |
| **Strategy** (`Temporada`) | *"un mecanismo que permita **cambiar de forma automática de temporada**"* → dos fórmulas intercambiables **en runtime**. No Template Method, porque hay que **cambiarla con el sistema andando**. |
| **State** (`EstadoHabitacion`) | *"Una habitación **sólo puede estar disponible o reservada**"* + *"si se intenta reservar una reservada → **excepción**"* → la regla vive dentro de `Reservada.reservar()`, sin `if`. |

**SOLID:** **OCP** (régimen nuevo = clase nueva, literal en el enunciado) · **DIP** (`Habitacion` depende de `EstadoHabitacion` y `Temporada`, no de las concretas) · **SRP** (la habitación no sabe de fórmulas de temporada; la temporada no sabe de estados) · **LSP** (cualquier `Regimen` sirve donde se espera un `Regimen`) · **ISP** (interfaces de 1-3 métodos).

> **El trade-off honesto del State, que conviene decir vos antes de que te lo pregunten:** con **dos** estados, un `boolean reservada` alcanzaría y sería más simple. State se paga solo cuando **esperás más estados** (En mantenimiento, Ocupada, Bloqueada) o **transiciones con reglas**. Acá lo elijo porque el enunciado pide una **excepción condicionada al estado** y State la ubica sin un `if`, y porque un hotel real suma estados. **Decir esto muestra criterio; no decirlo, parece que aplicaste el patrón de memoria.**

---

# 4. Trenes y depósitos

> El más largo, y el que **menos patrones** tiene. Lo que entrena es otra cosa: **polimorfismo y colecciones**.

---

## El enunciado

> **Ejercicio – trenes y depósitos**
>
> Una administradora ferroviaria necesita una aplicación que le ayude a manejar las formaciones que tiene disponibles en distintos depósitos.
>
> Una formación es lo que habitualmente llamamos "un tren", tiene una o varias locomotoras, y uno o varios vagones. Hay vagones de pasajeros y vagones de carga.
>
> En cada depósito hay: formaciones ya armadas, y locomotoras sueltas que pueden ser agregadas a una formación.
>
> De cada vagón de pasajeros se conoce el largo en metros, y el ancho útil también en metros. La cantidad de pasajeros que puede transportar un vagón de pasajeros es:
>
> - Si el ancho útil es de hasta 2.5 metros: metros de largo * 8.
> - Si el ancho útil es de más de 2.5 metros: metros de largo * 10.
>
> Por ejemplo, si tenemos dos vagones de pasajeros, los dos de 10 metros de largo, uno de 2 metros de ancho útil, y otro de 3 metros de ancho útil, entonces el primero puede llevar 80 pasajeros, y el segundo puede llevar 100.
>
> Un vagón de pasajeros no puede llevar carga.
>
> De cada vagón de carga se conoce la carga máxima que puede llevar, en kilos. Un vagón de carga no puede llevar ningún pasajero.
>
> No hay vagones mixtos.
>
> El peso máximo de un vagón, medido en kilos, se calcula así:
>
> - Para un vagón de pasajeros: cantidad de pasajeros que puede llevar * 80.
> - Para un vagón de carga: la carga máxima que puede llevar + 160 (en cada vagón de carga van dos guardas).
>
> De cada locomotora se sabe: su peso, el peso máximo que puede arrastrar, y su velocidad máxima. Por ejemplo, puedo tener una locomotora que pesa 1000 kg, puede arrastrar hasta 12000 kg, y su velocidad máxima es de 80 km/h. Obviamente se tiene que arrastrar a ella misma, entonces no le puedo cargar 12000 kg de vagones, solamente 11000; diremos que este es su "arrastre útil".
>
> Una locomotora puede ser agregada a una formación sólo si la formación se encuentra en el depósito detenida o en depósito. Si la formación ya está en movimiento no se debe hacer nada.
>
> Tener en cuenta que:
>
> - El peso útil de la locomotora a agregar debe ser mayor o igual a los kilos de empuje que le faltan a la formación.
> - En caso de que no existan locomotoras disponibles se debe arrojar un error.

---

## El enunciado — lo que hay que resolver

> **Generar el diagrama de clases y código que permita saber:**
>
> 1. El total de pasajeros que puede transportar una formación
> 2. Cuántos vagones livianos tiene una formación; un vagón es liviano si su peso máximo es menor a 2500 kg
> 3. La velocidad máxima de una formación, que es el mínimo entre las velocidades máximas de las locomotoras.
> 4. Si una formación es eficiente; es eficiente si cada una de sus locomotoras arrastra, al menos, 5 veces su peso (el de la locomotora misma).
> 5. Si una formación puede moverse. Una formación puede moverse si el arrastre útil total de las locomotoras es mayor o igual al peso máximo total de los vagones.
> 6. Cuántos kilos de empuje le faltan a una formación para poder moverse, que es: 0 si ya se puede mover, y (peso máximo total de los vagones – arrastre útil total de las locomotoras) en caso contrario.
> 7. Dado un depósito, el conjunto formado por el vagón más pesado de cada formación; se espera un conjunto de vagones.
> 8. Si un depósito necesita un conductor experimentado. Un depósito necesita un conductor experimentado si alguna de sus formaciones es compleja. Una formación es compleja si: tiene más de 20 unidades (sumando locomotoras y vagones), o el peso total (sumando locomotoras y vagones) es de más de 10000 kg.
> 9. Agregar una locomotora a una formación. Evaluar distintos escenarios.

---

## Ejercicio en vivo — los cuatro pasos

**Los mismos cuatro pasos**, aunque el enunciado sea el doble de largo. Acá el paso 2 viene servido: los nueve puntos **son** los verbos.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Paso 1 — Sustantivos:** **formación**, **locomotora**, **vagón** (de pasajeros / de carga), **depósito**, administradora, pasajero, guarda, conductor.
> Candidatos reales: **Formación**, **Locomotora**, **Vagón** + sus dos hijos, **Depósito**.
> El enunciado te vuelve a regalar la jerarquía: *"Hay vagones **de pasajeros** y vagones **de carga**"* + *"**No hay vagones mixtos**"* ← esa última frase te está diciendo **"son dos clases hermanas y ninguna es las dos cosas"**.
>
> **Paso 2 — Verbos:** los **nueve puntos**, tal cual. Ese es todo el trabajo. No inventes nada más.
>
> **Paso 3 — Frases delatoras:** acá pasa algo distinto — **casi no hay**. Solo **una**:
> - *"sólo si la formación se encuentra detenida o en depósito. Si la formación ya está **en movimiento no se debe hacer nada**"* → un comportamiento que **depende de la etapa** + una acción que **no hace nada** en cierto estado.
>
> **Paso 4 — ¿Cuántos problemas?** **Dos, y muy desparejos:**
> 1. **Calcular cosas sobre colecciones** (los puntos 1 a 8) → no es un patrón: es **polimorfismo + operaciones de colección**.
> 2. **Agregar una locomotora** (el punto 9) → **un** patrón, para el estado de la formación.
>
> **La lección de este parcial:** el paso 3 te dio **una sola** frase delatora. **Eso también es información.** Cuando no hay frases, **no hay patrón** — y meter uno a la fuerza es *patternitis*. El 90% de este examen se resuelve con **buen POO** y nada más.

---

## El enunciado, marcado

> **negrita** = sustantivo candidato a clase · *cursiva* = verbo que te piden · 🔑 = frase delatora

---

> Una administradora ferroviaria necesita una aplicación que le ayude a manejar las **formaciones** que tiene disponibles en distintos **depósitos**.
>
> Una formación (…) tiene una o varias **locomotoras**, y uno o varios **vagones**. 🔑 *Hay vagones de pasajeros y vagones de carga.* (…)
>
> De cada **vagón de pasajeros** se conoce el largo en metros, y el ancho útil también en metros. (…)
>
> Un vagón de pasajeros no puede llevar carga. De cada **vagón de carga** se conoce la carga máxima que puede llevar, en kilos. 🔑 *Un vagón de carga no puede llevar ningún pasajero.* 🔑 *No hay vagones mixtos.*
>
> (…) De cada locomotora se sabe: su peso, el peso máximo que puede arrastrar, y su velocidad máxima. (…) diremos que este es su "**arrastre útil**".
>
> Una locomotora puede ser *agregada* a una formación sólo si la formación se encuentra en el depósito **detenida** o **en depósito**. 🔑 *Si la formación ya está en movimiento no se debe hacer nada.*
>
> **Generar el diagrama de clases y código que permita saber:** *(los 9 puntos = los 9 verbos)*
>
> 1. *El total* de pasajeros (…) 2. *Cuántos* vagones livianos (…) 3. *El mínimo* entre las velocidades (…) 4. 🔑 *cada* una de sus locomotoras (…) 7. 🔑 *el conjunto* formado por (…) 8. 🔑 *alguna* de sus formaciones (…)

**Tres lecciones que solo se ven marcando:**

1. *"Hay vagones de pasajeros y de carga"* + *"No hay vagones mixtos"* → el enunciado te **regala la jerarquía** y encima te aclara que **nadie es las dos cosas**. Eso es "dos subclases hermanas" dicho en castellano.
2. **Una sola frase delatora de patrón** en todo el enunciado (*"no se debe hacer nada"*). El resto de las 🔑 no delatan patrones: **delatan operaciones de colección** (`every`, `some`, `Set`).
3. **Los nueve puntos son los nueve verbos.** El paso 2 acá viene resuelto — pero solo si lo mirás así.

---

## Razoná — ¿cuántos pasajeros lleva un vagón de carga?

El enunciado dice: *"Un vagón de carga no puede llevar ningún pasajero"*. Y el punto 1 te pide *"el total de pasajeros que puede transportar una formación"*, que suma **todos** los vagones.

**¿Qué hacés con `VagonCarga.cantidadPasajeros()`?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Devolvés `0`. No tirás excepción, y no preguntás el tipo.**
>
> Las tres opciones que se ven en un parcial:
>
> ```typescript
> // ❌ Rompe LSP: un VagonCarga ya NO es sustituible por un Vagon.
> cantidadPasajeros(): number { throw new Error("no lleva pasajeros"); }
>
> // ❌ Mata el polimorfismo: volvés a preguntar el tipo.
> total = vagones.filter(v => v instanceof VagonPasajeros)
>                .reduce((t, v) => t + v.cantidadPasajeros(), 0);
>
> // ✅ El vagón de carga responde la verdad: lleva cero pasajeros.
> cantidadPasajeros(): number { return 0; }
> ```
>
> **Por qué la primera está mal, que es lo que quieren que digas:** si `VagonCarga` explota donde `VagonPasajeros` responde, entonces **no es sustituible por su padre** → **viola LSP**. Y el síntoma se ve enseguida: el que suma se ve **obligado a preguntar el tipo** antes de llamar, y ahí perdiste el polimorfismo (opción 2).
>
> **"No lleva ningún pasajero" no es un error: es una cantidad. Y esa cantidad es cero.** Con `0`, el `reduce` recorre todos los vagones sin preguntar nada y suma bien.
>
> **Y acá está la respuesta al spoiler de Reservas:** los vagones **sí** se heredan, porque `cantidadPasajeros()` y `pesoMaximo()` se calculan **distinto** en cada uno. Cambia el **comportamiento** → herencia. Las habitaciones del hotel solo cambiaban de **valores** → una sola clase. **Esa es la regla, y se ve mejor comparando los dos parciales.**

---

## La tabla que resuelve 6 de los 9 puntos

Traducción literal del castellano del enunciado al código. **Leelo dos veces:**

| El enunciado dice | La operación |
|---|---|
| *"el **total** de pasajeros"* | `reduce((t, v) => t + v.cantidadPasajeros(), 0)` |
| *"**cuántos** vagones livianos"* | `filter(v => v.esLiviano()).length` |
| *"el **mínimo** entre las velocidades"* | `Math.min(...locomotoras.map(l => l.velocidadMaxima))` |
| *"**cada** una de sus locomotoras…"* | `every(l => …)` |
| *"**alguna** de sus formaciones…"* | `some(f => f.esCompleja())` |
| *"el **conjunto** formado por…"* | `new Set(formaciones.map(f => f.vagonMasPesado()))` |
| *"el vagón **más pesado**"* | `reduce((max, v) => v.pesoMaximo() > max.pesoMaximo() ? v : max)` |

> **"Cada" → `every`. "Alguna" → `some`. "Conjunto" → `Set`.** No es casualidad: el que escribe el enunciado piensa en la operación y **después** la traduce al castellano. Vos hacés el camino inverso.

---

## Trenes — los vagones (polimorfismo puro)

```typescript
abstract class Vagon {
  abstract cantidadPasajeros(): number;
  abstract pesoMaximo(): number;

  // Regla común: vive una sola vez, en el padre.
  esLiviano(): boolean {
    return this.pesoMaximo() < 2500;
  }
}

class VagonPasajeros extends Vagon {
  constructor(
    private readonly largo: number,
    private readonly anchoUtil: number,
  ) { super(); }

  cantidadPasajeros(): number {
    return this.anchoUtil <= 2.5 ? this.largo * 8 : this.largo * 10;
  }
  pesoMaximo(): number {
    return this.cantidadPasajeros() * 80;
  }
}

class VagonCarga extends Vagon {
  constructor(private readonly cargaMaxima: number) { super(); }

  cantidadPasajeros(): number { return 0; }        // no lleva: son CERO
  pesoMaximo(): number { return this.cargaMaxima + 160; }  // +160: dos guardas
}

class Locomotora {
  constructor(
    private readonly peso: number,
    private readonly pesoMaximoArrastre: number,
    readonly velocidadMaxima: number,
  ) {}

  arrastreUtil(): number {
    return this.pesoMaximoArrastre - this.peso;   // se arrastra a sí misma
  }
  esEficiente(): boolean {
    return this.arrastreUtil() >= this.peso * 5;
  }
  pesoPropio(): number { return this.peso; }
}
```

**El único `if` de todo el ejercicio** está adentro de `VagonPasajeros.cantidadPasajeros()`, decidiendo entre `*8` y `*10`. Y ahí está bien: es una **regla del negocio sobre un dato propio**, no una pregunta sobre el tipo de un objeto.

> **Supuesto a declarar:** *"cada locomotora arrastra al menos 5 veces su peso"* lo interpreto sobre el **arrastre útil** (lo que puede arrastrar además de sí misma), que es lo que el enunciado se toma el trabajo de definir.

---

## Trenes — la formación

```typescript
class Formacion {
  private readonly locomotoras: Locomotora[] = [];
  private readonly vagones: Vagon[] = [];
  private estado: EstadoFormacion = new EnDeposito();

  totalPasajeros(): number {                                          // 1
    return this.vagones.reduce((t, v) => t + v.cantidadPasajeros(), 0);
  }

  vagonesLivianos(): number {                                          // 2
    return this.vagones.filter(v => v.esLiviano()).length;
  }

  velocidadMaxima(): number {                                          // 3
    return Math.min(...this.locomotoras.map(l => l.velocidadMaxima));
  }

  esEficiente(): boolean {                                             // 4
    return this.locomotoras.every(l => l.esEficiente());   // "CADA una"
  }

  arrastreUtilTotal(): number {
    return this.locomotoras.reduce((t, l) => t + l.arrastreUtil(), 0);
  }

  pesoMaximoVagones(): number {
    return this.vagones.reduce((t, v) => t + v.pesoMaximo(), 0);
  }

  puedeMoverse(): boolean {                                            // 5
    return this.arrastreUtilTotal() >= this.pesoMaximoVagones();
  }

  kilosDeEmpujeQueFaltan(): number {                                   // 6
    return this.puedeMoverse()
      ? 0
      : this.pesoMaximoVagones() - this.arrastreUtilTotal();
  }

  vagonMasPesado(): Vagon {                                            // 7
    return this.vagones.reduce((max, v) => v.pesoMaximo() > max.pesoMaximo() ? v : max);
  }

  esCompleja(): boolean {                                              // 8
    const unidades = this.locomotoras.length + this.vagones.length;
    const pesoTotal =
      this.pesoMaximoVagones() +
      this.locomotoras.reduce((t, l) => t + l.pesoPropio(), 0);
    return unidades > 20 || pesoTotal > 10000;
  }

  agregarLocomotora(l: Locomotora): void {                             // 9
    this.estado.agregarLocomotora(this, l);      // delega en el estado
  }

  sumarLocomotora(l: Locomotora): void { this.locomotoras.push(l); }
  cambiarEstado(e: EstadoFormacion): void { this.estado = e; }
}
```

**`kilosDeEmpujeQueFaltan()` reusa `puedeMoverse()`, que reusa `arrastreUtilTotal()` y `pesoMaximoVagones()`.** El enunciado te dio los puntos **en el orden en que se construyen**: cada uno usa el anterior. Si escribís el 5 antes que el 6, el 6 son dos líneas.

---

## Predecí — agregar una locomotora a un tren en movimiento

La única frase delatora del enunciado: *"Una locomotora puede ser agregada sólo si la formación se encuentra detenida o en depósito. **Si la formación ya está en movimiento no se debe hacer nada**."*

**¿Cómo se escribe "no se debe hacer nada" sin un `if`?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Un método vacío en la clase del estado.** Exactamente lo mismo que las transiciones prohibidas del reproductor de la Clase 5.
>
> ```typescript
> interface EstadoFormacion {
>   agregarLocomotora(f: Formacion, l: Locomotora): void;
> }
>
> class EnDeposito implements EstadoFormacion {
>   agregarLocomotora(f: Formacion, l: Locomotora): void {
>     if (l.arrastreUtil() >= f.kilosDeEmpujeQueFaltan()) {
>       f.sumarLocomotora(l);
>     }
>   }
> }
>
> class Detenida implements EstadoFormacion {
>   agregarLocomotora(f: Formacion, l: Locomotora): void {
>     if (l.arrastreUtil() >= f.kilosDeEmpujeQueFaltan()) {
>       f.sumarLocomotora(l);
>     }
>   }
> }
>
> class EnMovimiento implements EstadoFormacion {
>   agregarLocomotora(f: Formacion, l: Locomotora): void {
>     // "no se debe hacer nada" ← el enunciado, literal
>   }
> }
> ```
>
> **`Formacion` no tiene un solo `if` preguntando el estado.** Le pide al estado que resuelva, y el estado que no puede… no hace nada. **"No se debe hacer nada" es un método vacío**, y es el enunciado diciéndote "acá va State" en voz alta.
>
> **Los escenarios que el punto 9 te pide evaluar** (decilos, son gratis):
> 1. Formación **en movimiento** → no pasa nada.
> 2. Formación en depósito + locomotora con **arrastre útil suficiente** → se agrega.
> 3. Formación en depósito + locomotora **insuficiente** → no se agrega.
> 4. **No hay locomotoras disponibles** → el **depósito** lanza el error (esa regla es del depósito, que es quien las tiene, no de la formación).

---

## Trenes — el depósito

```typescript
class Deposito {
  private readonly formaciones: Formacion[] = [];
  private readonly locomotorasSueltas: Locomotora[] = [];

  vagonesMasPesados(): Set<Vagon> {                                    // 7
    return new Set(this.formaciones.map(f => f.vagonMasPesado()));
  }

  necesitaConductorExperimentado(): boolean {                          // 8
    return this.formaciones.some(f => f.esCompleja());   // "ALGUNA"
  }

  // "En caso de que no existan locomotoras disponibles se debe arrojar un error."
  asignarLocomotoraA(formacion: Formacion): void {
    if (this.locomotorasSueltas.length === 0) {
      throw new SinLocomotorasDisponiblesError("No hay locomotoras en el depósito");
    }
    const locomotora = this.locomotorasSueltas.shift()!;
    formacion.agregarLocomotora(locomotora);
  }
}
```

**Por qué el error va acá y no en `Formacion`:** *"no existan locomotoras **disponibles**"* — las disponibles las tiene el **depósito**. La formación no sabe ni le importa si el depósito está vacío. **Cada regla en la clase que tiene el dato.** Eso es SRP, y es una de las cosas que el punto 4 de cualquiera de estos parciales quiere leer.

---

## Trenes — la justificación

- **Herencia + polimorfismo** (`Vagon` abstracto): `cantidadPasajeros()` y `pesoMaximo()` se calculan **distinto** según el tipo de vagón — **acá la herencia SÍ se justifica**, porque cambia el **comportamiento** (a diferencia de las habitaciones del hotel, que solo cambiaban de valores).
- **LSP**: `VagonCarga.cantidadPasajeros()` devuelve **0**, no explota. Por eso `totalPasajeros()` suma sin preguntar tipos.
- **State** (`EstadoFormacion`): *"si ya está en movimiento **no se debe hacer nada**"* → método vacío en `EnMovimiento`, cero `if` en `Formacion`.
- **SRP**: el vagón sabe su peso, la locomotora su arrastre, la formación coordina, el depósito administra las locomotoras sueltas y lanza el error cuando no hay.
- **Buen POO**: `esLiviano()` vive **una vez** en `Vagon` (no repetido en las dos subclases); `kilosDeEmpujeQueFaltan()` reusa `puedeMoverse()`; ningún `instanceof`.

> **Y lo que hay que animarse a decir:** *"El resto del ejercicio **no requiere patrones**: se resuelve con herencia, polimorfismo y operaciones sobre colecciones. Meter un patrón acá sería patternitis."* Reconocer **dónde NO va un patrón** es tan evaluable como reconocer dónde sí.

---

## ¿Y las estructuras de datos?

Hay que decirlo derecho, porque es la pregunta que te vas a hacer:

**En estos cuatro parciales no hay pilas, colas ni listas enlazadas.** Ninguno pide "el último en entrar es el primero en salir" ni "el primero en entrar es el primero en salir". Lo que piden es **colecciones** (varios X adentro de un Y) y, en los Composite, **recursión sobre un árbol**.

**Pero la Unidad 3 es evaluable**, y este es el punto de contacto real:

| Donde el enunciado dice… | Podés usar… |
|---|---|
| *"una reserva posee habitaciones reservadas"* | `Habitacion[]` — o `List<Habitacion>` de la cátedra |
| *"**crear, eliminar y listar** reservas"* | ¡Esa **es** la API de `List<T>`! `push` / `delete` / recorrer |
| *"un depósito tiene formaciones y locomotoras sueltas"* | dos colecciones |
| *"el **conjunto** formado por…"* | `Set` — el enunciado usa la palabra a propósito |

**Cuándo aparecería cada una** (la señal delatora, igual que con los patrones):

| Estructura | La frase que la delata | La cátedra la llama |
|---|---|---|
| **Pila** (LIFO) | *"deshacer"*, *"historial"*, *"el último en entrar es el primero en salir"* | `Stack<T>` — `push` / `pop` |
| **Cola** (FIFO) | *"turnos"*, *"se atiende por orden de llegada"*, *"primero en entrar, primero en salir"* | `Queue<T>` — `enqueue` / `dequeue` |
| **Lista enlazada** | *"insertar en cualquier parte"*, *"recorrer en cualquier orden"*, *"mantener ordenado"* | `List<T>` — `insertOrdered` / `search` / `delete` |

> **La decisión práctica para el examen:** si el enunciado **no pide** una política de orden (LIFO/FIFO), un **array alcanza** y nadie te lo baja. Si el docente quiere ver la Unidad 3 explícita, la frase que te salva es: *"uso una colección de `X`; si se requiriera la implementación de la cátedra, sería `List<X>` con `push`/`delete`/`search`, que es exactamente lo que pide 'crear, eliminar y listar'."*
>
> Y ojo con la trampa fácil: **una pila y una cola son de lectura destructiva** — para leer un elemento **hay que sacarlo**. Si un enunciado necesita recorrer sin borrar, **no es ni pila ni cola**.

---

## Lo que se repite en los cuatro

Recién ahora, con los cuatro resueltos, se puede mirar el conjunto:

| | TravelNow | Heladería | Reservas | Trenes |
|---|---|---|---|---|
| **Estructurar** | Composite | Composite | — | Herencia + polimorfismo |
| **Calcular algo que varía** | Strategy | Strategy | Strategy ×2 | — |
| **Estado / reglas** | — | — | State | State |
| **La clase que orquesta** | `Viaje` | `Pedido` | `SistemaDeReservas` | `Deposito` |

**Tres cosas y ya:**

1. **Composite + Strategy + un contexto** es **el molde**. Dos de los cuatro son literalmente eso, y uno de los dos es el final real.
2. **Siempre hay una clase que orquesta**, y es la que todos se olvidan de dibujar: `Viaje`, `Pedido`, `SistemaDeReservas`, `Deposito`. **Es tu punto flaco de las tres entregas.** El **ejemplo de uso** te obliga a escribirla — por eso nunca lo saltees.
3. **La justificación se contesta con citas del enunciado**, no con definiciones.

---

## La tabla de frases delatoras

Ahora sí: esta tabla **no te la di al principio a propósito**. Está armada con las frases que **encontraste vos** en el paso 3 de los cuatro enunciados. Esta es la que va al examen:

| Lo que dice el enunciado | Lo que quiere |
|---|---|
| *"…o incluso otros X (anidados)"* / *"contiene X y otros X"* | **Composite** |
| *"la duración/precio total de **cualquier** X"* / *"de cualquier X **y/o** Y"* | **Composite** (hoja y rama tratadas igual) |
| *"planea agregar nuevos tipos **sin modificar** la lógica"* | **Strategy** + **OCP** (la palabra que quieren oír) |
| *"varía **según el tipo** de…"* | **Strategy** |
| *"un **mecanismo** que permita **cambiar** X"* | **Strategy** (runtime), **no** Template Method |
| *"el procedimiento es **idéntico**, cambia el cómo"* | **Template Method** |
| *"sólo puede estar **disponible o reservada**"* | **State** |
| *"si ya está en movimiento **no se debe hacer nada**"* | **State** (método vacío) |
| *"si no puede, **pasa al siguiente**"* / *"escala"* | **Chain of Responsibility** |
| *"**una sola** instancia en todo el sistema"* | **Singleton** |
| *"muchos campos **opcionales**"* / *"constructor de N parámetros"* | **Builder** |
| *"**cada** una de sus X cumple…"* | `every` |
| *"**alguna** de sus X cumple…"* | `some` |
| *"el **conjunto** formado por…"* | `Set` |
| **No hay ninguna frase delatora** | **No hay patrón.** Buen POO y listo. |

> La última fila es la que más gente ignora, y es la que te salva de la **patternitis**. **Trenes** es la prueba: nueve puntos, **una sola** frase delatora.

---

## Checkpoint — la última pregunta

Estás en el examen. Leés:

> *"Un menú de restaurante tiene secciones, y una sección puede tener platos y otras secciones. Hay que calcular las calorías totales de cualquier sección. Además, el precio final varía según si el cliente es común, socio o VIP, y el restaurante planea agregar nuevas categorías de cliente."*

**Los cuatro pasos, en la cabeza, en dos minutos. ¿Cuántos problemas, qué patrones, y cómo se llama la clase que casi te olvidás?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Es TravelNow con otro disfraz. Otra vez.**
>
> **Paso 1 — Sustantivos:** menú, **sección**, **plato**, calorías, **categoría de cliente**, precio.
> **Paso 2 — Verbos:** *"**calcular** las calorías totales"* y *"el precio final **varía**"*. **Dos.**
> **Paso 3 — Frases:** *"una sección puede tener platos **y otras secciones**"* + *"las calorías totales de **cualquier** sección"*; y *"varía **según** la categoría"* + *"**planea agregar** nuevas categorías"*. **Dos.**
> **Paso 4 — Problemas: dos.** Uno de estructurar, uno de comportamiento.
>
> **Los patrones:**
> 1. **Composite** — `interface ItemMenu { calorias(): number }`, `Plato` (hoja), `Seccion` (compuesto con `items: ItemMenu[]`).
> 2. **Strategy** + **OCP** — `interface CategoriaCliente { calcularPrecio(item: ItemMenu): number }` con `Comun`, `Socio`, `VIP`.
>
> **La clase que casi te olvidás:** el **contexto** — `Cuenta` o `Consumo`, que tiene un `ItemMenu` y una `CategoriaCliente` y **delega**. Sin ella, el corrector no ve **quién elige**.
>
> **Los desempates, gratis:** no es Decorator (la sección **agrupa muchos**, no envuelve **uno** para sumarle). No es Abstract Factory (te piden **calcular**, no **crear** una familia coherente).
>
> Si esto te salió sin dudar: **estás listo.**

---

## Después de clase — el checklist de 5 minutos

Antes de entregar cualquier parcial. Está armado con **tus** errores reales de las tres entregas:

1. **¿Hice los cuatro pasos?** Sustantivos → verbos → frases → **cuántos problemas**. El 4 es el que ordena todo.
2. **¿Está la clase que orquesta?** (`Viaje`, `Pedido`, el contexto del State). Tu error #1 en las tres entregas. Si no está en el diagrama, no está.
3. **¿Escribí el ejemplo de uso?** Diez líneas. Te obliga a encontrar el punto 2 solo.
4. **¿Hay algún `if` sobre tipos** (`instanceof`, `switch (tipo)`)? Si hay, se te escapó un polimorfismo.
5. **¿El producto apunta a su fábrica?** Si sí, está al revés. El producto es tonto.
6. **¿Los métodos devuelven algo?** Nada de `void` donde tiene que volver un número.
7. **¿Las interfaces tienen atributos?** No pueden. Sacalos.
8. **¿Justifiqué con citas del enunciado** y descarté el parecido? Cuatro renglones, la parte más barata.
9. **¿Declaré mis supuestos** donde el enunciado era ambiguo? Suma, no resta.
10. **Multiplicidad `*`** en las agregaciones del Composite. Un caracter. Y **`static` subrayado** si metiste un Singleton.

---

## Después de clase — para practicar

**El de verdad:** rehacé **TravelNow** entero, solo, con el reloj: **50 minutos**, papel, sin mirar nada. Es el final real. Cuando termines, comparalo con las slides.

**Los que no hicimos hoy**, en el mismo formato (los 4 pasos → diagrama → métodos → ejemplo de uso → justificación):
- El que haya quedado de los cuatro.
- El **simulacro integrador de la Clase 5** (el sistema de tickets: Singleton + Builder + CoR + State). Es el más completo de todos y ya tiene solución comentada.

**Y una vuelta corta a lo tuyo:** las devoluciones de las Clases 3, 4 y 5. El **Bloque A** de la devolución de la Clase 5 (el orquestador) es exactamente el punto 2 del checklist, y es lo único que te separa de la nota entera.

---

## Después de clase — Unidad 3 en una slide

La cátedra la toma y nosotros no le dimos una clase entera. Lo mínimo indispensable, todo construido sobre el mismo `Nodo<T>` genérico:

| | Política | Operaciones | Lectura | Se usa para |
|---|---|---|---|---|
| **`Stack<T>`** | **LIFO** — último en entrar, primero en salir | `push` / `pop` / `isEmpty` | **Destructiva** | Deshacer, historial de navegación, invertir un orden |
| **`Queue<T>`** | **FIFO** — primero en entrar, primero en salir | `enqueue` / `dequeue` / `isEmpty` | **Destructiva** | Turnos, cola del cajero, trabajos por orden de llegada |
| **`List<T>`** | **Ninguna** — insertás y recorrés donde quieras | `push` / `insertFirst` / `insertOrdered` / `search` / `delete` / `sort` / `clear` | **No destructiva** | Todo lo demás: colecciones que se recorren y se editan |

**Las tres se construyen sobre el mismo `Nodo<T>`:** un `value: T` y un `next: Nodo<T>`. Lo único que cambia es **por dónde entrás y por dónde salís**:
- **Pila:** entrás y salís por **la cabeza**.
- **Cola:** entrás por **el final**, salís por **el frente** (por eso guarda `first` **y** `last`).
- **Lista:** entrás y salís **por donde quieras** (por eso `insertOrdered` y `search` recorren con un puntero auxiliar).

> **Lo que hay que poder decir:** *"Pila y cola tienen **política de orden** y **lectura destructiva**; la lista **no tiene política** y **se puede leer sin borrar**."* Y el detalle que cae: **la cola necesita dos punteros** (`first` y `last`); la pila, **uno solo** (`head`).

---

## Después de clase — lectura

- **Los cuatro enunciados de hoy**, releídos **con el subrayador**: hacé el **paso 3** en cada uno y compará con la tabla de frases delatoras. Vas a ver que están todas ahí, en la superficie.
- **PDF de la cátedra, Unidad 3** — pilas, colas y listas, con el código TypeScript completo (`MyNode<T>`, `Stack<T>`, `Queue<T>`, `List<T>`). Es la unidad que no dimos y la que menos trabajo cuesta: es **código para leer**, no criterio para construir.
- **Ficha final de los 13 patrones** (Clase 5) — la noche antes, solo esa tabla.

> **Lo único que importa mañana:** vas **17 de 17** eligiendo patrones. Reconocerlos ya lo tenés. Lo que se corrige en un parcial es lo otro: **hacer los cuatro pasos**, **dibujar al que orquesta**, **escribir el ejemplo de uso** y **justificar con la cita del enunciado**. Cuatro cosas, y las cuatro son más baratas que estudiar un patrón nuevo.
