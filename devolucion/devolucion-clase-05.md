# Devolución — Ejercicios de la Clase 5 (patrones)

> **Leé primero la devolución de la Clase 4.** Las dos entregas las mandaste **el mismo día con cuatro horas de diferencia**, así que ninguna de las dos vio la corrección de la otra. Lo que te marco acá se apoya en aquello.

## Titular: 4 de 4 — y por primera vez escribiste un desempate

**Los cuatro patrones correctos** (Chain of Responsibility, Template Method, State, Facade). Con esto vas **17 de 17** en tres entregas.

Pero el titular no es ese. Es esto, que escribiste en el ejercicio C:

> *"State ya que se necesita cambiar lo que hace el reproductor de música dependiendo de su estado. **No es Strategy porque el usuario no es el que hace el cambio**."*

**Eso es exactamente lo que te vengo pidiendo desde la Clase 3, y lo hiciste solo.** Es la primera vez que descartás el parecido por escrito, y encima el desempate está **bien**: la diferencia real entre State y Strategy es justo esa — en Strategy el cliente elige el algoritmo desde afuera; en State **el estado decide el siguiente desde adentro**. No lo dijiste con las palabras del apunte, lo dijiste con las tuyas, que es mejor.

Y no es un accidente: en esta entrega **justificaste 3 de 4** (CoR, State y Facade), contra 2 de 6 en la Clase 4 —que mandaste cuatro horas antes— y sin haber leído una sola corrección en el medio. **Mejoraste solo, en una tarde.** Cuando leas la devolución de la Clase 4 y veas que te insisto con la justificación, tené presente esto: el problema ya se está arreglando.

Dicho eso, hay **un error importante** en el Template Method y **un cruce conceptual** en el State. Vamos.

---

## Ejercicio A — Chat de soporte que escala → **Chain of Responsibility** ✅

Lo que dibujaste: `<<interface>> Validador` con `setSiguiente(s: Validador)` y `procesarSolicitud(...): boolean`; `ValidadorBase` **abstracto** que la implementa **y** la agrega (`- siguiete: Validador`); y `Nivel1`, `Nivel2`, `Supervisor` heredando. **La estructura del patrón está bien**: la clase base abstracta que guarda al siguiente **del tipo de la interfaz** es lo que hace que la cadena se pueda armar en cualquier orden, y eso lo pusiste.

**Y la justificación es buena:** *"pide que cada nivel decide si se resuelve o pasa al siguiente"* — esa es la señal delatora textual del patrón. ✅

**A pulir:**
- **Falta el `Bot`.** El enunciado dice *"primero las atiende un **bot**; si no puede, pasa a un agente de nivel 1…"*: son **cuatro** eslabones (`Bot`, `Nivel1`, `Nivel2`, `Supervisor`) y dibujaste tres. Es el **primero** de la cadena, justo el que arranca todo. Leé el enunciado con el lápiz contando sustantivos.
- **`- numeroGrupo: number` adentro de la interfaz.** Dos problemas: (a) **una interfaz no tiene atributos**, y menos privados — una interfaz es un **contrato de comportamiento**, no tiene estado; (b) `numeroGrupo` es un sobrante del **Composite de la Clase 4** (el `Equipo`). Se te coló copiando el archivo anterior. Aparece **también** en el ejercicio C, así que revisá la plantilla que estás reusando.
- **El parámetro se llama al revés.** `procesarSolicitud(respuesta: string)`: lo que **entra** a la cadena es la **consulta** del cliente; la respuesta es lo que **sale**. `procesarSolicitud(consulta: string): boolean`.
- **El nombre `Validador` no es de este dominio.** Estos no validan nada: **atienden y escalan**. `Soporte`, `Manejador` o `ManejadorConsulta` dice lo que hacen. El nombre de la interfaz es la primera cosa que lee el corrector.
- Typo: `siguiete` → `siguiente`.
- Faltó el desempate: *no es Decorator porque acá **uno solo** termina atendiendo — la cadena **se corta**; en Decorator **todos** los eslabones aportan.* **CoR descarta; Decorator acumula.** Este desempate está explícito en el PDF de la cátedra y **cae en el parcial**.

---

## Ejercicio B — Reportes PDF/Excel/HTML → **Template Method** ✅ (falta *el* método)

Lo que dibujaste: `Reportes` **abstracta** con `titulo`, `peso`, los pasos concretos `traerDatos()` y `agregarPie()`, y los pasos **abstractos** `armarEncabezado()`, `volcarFilas()`, `guardarArchivo()`; `PDF`, `Excel` y `HTML` redefiniendo los tres.

**Lo que está muy bien:** separaste **qué pasos son fijos** (traer datos, agregar el pie) de **cuáles varían** (encabezado, filas, guardado). Esa distinción es el corazón del patrón y la hiciste bien, sin ayuda.

**A pulir (esto es el error importante):**

- **Falta `generar()` — el template method.** No está el método que **define el esqueleto** y llama a los pasos en orden. Y sin él **no hay patrón**: lo que queda es una clase abstracta con métodos abstractos, que es otra cosa. Fijate el detalle: el patrón **se llama así por ese método**, y es el único que falta. Va **concreto en la clase padre**, y no se redefine nunca:

  ```typescript
  abstract class Reporte {
    // ESTE es el template method: el esqueleto fijo.
    generar(): void {
      const datos = this.traerDatos();
      this.armarEncabezado();
      datos.forEach(fila => this.volcarFila(fila));
      this.agregarPie(new Date());
      this.guardarArchivo();
    }

    protected abstract armarEncabezado(): string;
    protected abstract volcarFila(fila: Fila): void;
    protected abstract guardarArchivo(): void;
  }
  ```

  Y acá está la prueba de por qué importa: el enunciado dice que hoy hay **tres clases con el mismo código copiado que ya se desincronizaron**. Ese código duplicado es **el orden de los pasos**. Si `generar()` no existe, el orden **sigue estando copiado tres veces** en PDF, Excel y HTML — y **no arreglaste nada**. El patrón resuelve el problema del enunciado **solo** por ese método.

  > **Ojo con esto, porque no es la primera vez.** En la Clase 3 te marqué lo mismo en los dos Factory Method: faltaba el **método fijo** (`mostrar()`, `lanzarOleada()`) que usa la fábrica. Es **el mismo hueco**: modelás bien **las piezas** que varían, y se te escapa **la clase que las usa en orden**. Mirá el ejercicio C acá abajo y el Builder de la Clase 4: es siempre eso.

- **Los pasos abstractos deberían ser `protected` (`#`), no públicos (`+`).** Son **pasos internos** del algoritmo, no la API de la clase. Lo único público es `generar()`. Si `volcarFilas()` es público, cualquiera puede llamar los pasos sueltos y saltearse el esqueleto — que es justo lo que el patrón quiere impedir.
- `agregarPie(): date` — en TypeScript el tipo es `Date` con mayúscula. Y semánticamente va al revés: el pie **recibe** la fecha, no la devuelve → `agregarPie(fecha: Date): void`.
- `volcarFilas(): void` en el padre pero el enunciado dice *"cómo se vuelca **cada fila**"* → `volcarFila(fila: Fila)`, en singular; el bucle vive en `generar()`.
- Detalles: `Reportes` → **`Reporte`** (una clase es **una** cosa, va en singular); el atributo `peso` no lo usa nadie.
- **Falta la justificación por completo** — escribiste solo "template method". Y es una lástima, porque este enunciado **se justifica solo**: dice *"el procedimiento es **idéntico**"* y *"lo único que cambia es **cómo**"*, que es la definición del patrón palabra por palabra. Copiás esas dos citas y ya está justificado. Agregale el desempate: *no es Strategy porque no se cambia el algoritmo entero en runtime — se rellenan **algunos pasos** de un esqueleto fijo, y se decide **eligiendo la subclase**. Template Method usa **herencia**; Strategy, **composición**.*

---

## Ejercicio C — Reproductor play/pausa/stop → **State** ✅ (con los estados mal nombrados)

**La justificación es la mejor que escribiste hasta ahora** y ya la festejé arriba: patrón + desempate correcto contra Strategy. Eso es exactamente el formato que te piden en el parcial. **Ese párrafo, replicado en los otros tres, es la entrega perfecta.**

Lo que dibujaste: `Reproductor` con `- estadoActual: EstadoReproductor` y `cambiarEstado(e)`, compuesto con `<<interface>> EstadoReproductor`; y `Play`, `Stop`, `Pausa` implementándola. La **composición** al estado y el `cambiarEstado()` en el contexto están **bien** — esa es la maquinaria que hace que el estado pueda empujar la transición.

**A pulir (el cruce conceptual):**

1. **Nombraste los estados con los botones.** `Play`, `Stop` y `Pausa` son **las acciones** que el usuario dispara. Los **estados** son **`Detenido`, `Reproduciendo` y `Pausado`** — las tres situaciones en las que el reproductor **puede estar**. Volvé al enunciado y fijate que lo dice literal: *"si está **detenido**, play arranca; si está **reproduciendo**, play no hace nada; si está **pausado**, play retoma"*. Los estados son **sustantivos de situación**, no verbos de acción.

   Y esto no es un tema de nombres: **te cambia el diagrama**. Con tus nombres, `Play` es a la vez una clase y un método (`Play.arrancarCancion()`), y no hay dónde escribir *"desde detenido no se puede pasar a pausado"*. Con los nombres correctos, esa regla **es una línea**: `Detenido.pausa(r) { /* no hace nada */ }`. El test rápido: **un estado tiene que poder completar la frase "el reproductor está ___"**. "Está pausado" ✅ / "está pausa" ❌.

2. **Los tres métodos de la interfaz tienen que recibir el `Reproductor`.** Tenés `suspenderCancion(r: Reproductor)` y `terminarCancion(r: Reproductor)` ✅, pero `arrancarCancion(): void` **sin el parámetro**. Y ese `r` no es decorativo: es **cómo el estado dispara la transición**. Sin él, `Reproduciendo.arrancar()` no puede hacer `r.cambiarEstado(new Pausado())` — o sea, se rompe justo lo que decís bien en tu justificación (*"el usuario no es el que hace el cambio"*). El que lo hace necesita el `r`.

3. **Al `Reproductor` le falta `play()`.** Tiene `suspenderCancion()` y `terminarCancion()`, pero no el tercer botón. El contexto tiene que exponer **los tres** y **delegar** los tres:

   ```typescript
   class Reproductor {
     private estado: EstadoReproductor = new Detenido();
     play()  { this.estado.play(this); }    // ← delega, sin un solo if
     pausa() { this.estado.pausa(this); }
     stop()  { this.estado.stop(this); }
     cambiarEstado(e: EstadoReproductor) { this.estado = e; }
   }
   ```

   Ahí se ve el premio: el enunciado dice que hoy hay **un `switch (this.situacion)` en cada uno de los tres métodos**, y con State **los tres `switch` desaparecen**. Si el contexto no tiene los tres métodos delegando, no se ve que el patrón resolvió el problema.

4. `+ estadoActual(): void` — un método con el **mismo nombre que el atributo** y que devuelve `void`. Si querés un getter: `getEstado(): EstadoReproductor`. Si no, sacalo.

5. **`- numeroGrupo: number` otra vez adentro de la interfaz.** Mismo sobrante del Composite que en el ejercicio A. Limpiá la plantilla.

---

## Ejercicio D — Backup en cuatro pasos → **Facade** ✅

Lo que dibujaste: `Backup` con `+ hacerBackup(datos): void` y las cuatro flechas hacia `Compresor.zip()`, `Cifrado.encriptar()`, `Nube.subir()` y `Registro.log()`. **Correcto**, y la justificación también: *"pide un solo método de entrada por el que llegar a las diferentes clases"* ✅ — punto de entrada único, que es la señal de Facade.

**A pulir:**
- **Falta el cliente, que es donde se ve el valor del patrón.** El enunciado dice que ese bloque *"está pegado en **tres comandos distintos**"*. Dibujá los tres `Comando` con **una sola flecha cada uno hacia `Backup`** y **ninguna** hacia `Compresor`/`Cifrado`/`Nube`/`Registro`. **Eso es el patrón**: la foto de que el subsistema quedó **del otro lado de la puerta**. Sin el cliente, tu diagrama muestra una clase que llama a otras cuatro — y eso no tiene nada de particular.
- **El orden no se ve, y el orden es el punto.** Un diagrama de clases no puede mostrar secuencia (es estructura, no comportamiento), así que no es un error del dibujo — pero **decilo en palabras**: *"`hacerBackup()` llama en orden fijo a `zip()` → `encriptar()` → `subir()` → `log()`"*. Ese orden fijo, encapsulado **una sola vez**, es lo que Facade te compra.
- Faltó el desempate, y este es **el que más cae**: *no es **Adapter** porque no hay ninguna interfaz preexistente que respetar ni incompatibilidad que traducir — la interfaz `hacerBackup` **la inventás vos**, cómoda, sobre un subsistema que ya funciona.* **Facade simplifica; Adapter traduce.**
- Detalles: `Cifrado` → **`Cifrador`** (el enunciado lo nombra así, y además es **quien cifra**: sustantivo de agente, no de resultado); `log(): Void` → `void` en minúscula; y `hacerBackup(datos)` sin tipo en el parámetro.

---

## Para llevarte

1. **17 de 17.** Reconocer el patrón ya es tuyo. Confiá.
2. **Escribiste tu primer desempate y salió bien** (State vs Strategy, ej. C). Ese párrafo es el modelo: **patrón + la cita del enunciado + por qué no el parecido**. Replicalo en los cuatro y la entrega es perfecta. En esta entrega justificaste 3 de 4, contra 2 de 6 cuatro horas antes: **está mejorando solo**.
3. **El hueco real, y es uno solo: te falta siempre la clase que USA a las otras.** Mirá el patrón del patrón:
   - **Clase 3, Factory Method:** faltaba el **método fijo** (`mostrar()`, `lanzarOleada()`) del creador.
   - **Clase 4, Builder:** el **producto** apuntaba al builder, y el **cliente** no estaba.
   - **Clase 5, Template Method:** falta **`generar()`**, el esqueleto.
   - **Clase 5, State:** al **contexto** le faltan los métodos que delegan.
   - **Clase 5, Facade:** faltan los **tres comandos** que llaman a la puerta.

   Modelás muy bien **las piezas que varían** (las subclases, los productos, los estados: eso siempre está impecable) y se te escapa **el que las orquesta**. Y no es un detalle de prolijidad: **el orquestador es donde vive el beneficio del patrón**. Los tres `switch` que desaparecen, el código duplicado que se unifica, los tres comandos que se simplifican — todo eso pasa **en la clase que no dibujás**.

   > **Chequeo de 10 segundos antes de entregar cualquier patrón:** *"¿quién llama a esto, y está en mi diagrama?"* Si la respuesta es "se entiende", no está.

4. **Higiene** (rápido y gratis): `numeroGrupo` colado en **dos** interfaces —limpiá la plantilla que copiás—; **las interfaces no llevan atributos**; nombres en singular (`Reporte`, no `Reportes`) y del dominio (`Soporte`, no `Validador`; `Cifrador`, no `Cifrado`); `Void`/`date` → `void`/`Date`; y repasá los typos ("siguiete", "qeu", "deendiedo") — en un parcial escrito a mano eso resta.

---

# Ejercicios de refuerzo

Soluciones al final. Los Bloques A y B son **el mismo tema**: la clase que orquesta.

## Bloque A — El orquestador

> En los tres: dibujá el UML **completo** y marcá con una flecha **quién llama a quién**. No entregues ninguno sin la clase que usa a las otras.

**A1.** *(El ej. B, ahora completo.)* Rehacé el Template Method de los reportes **con `generar()`**. Quiero ver: `generar()` **concreto y público** en `Reporte`, los pasos variables **`protected abstract`**, y una nota al costado que diga **en qué orden** los llama. Después contestá en una línea: *si `generar()` no existiera, ¿qué problema del enunciado seguiría sin resolverse?*

**A2.** *(El ej. D, ahora completo.)* Rehacé el Facade **con los tres comandos**. Regla: de cada comando tiene que salir **exactamente una** flecha, y ninguna puede tocar `Compresor`, `Cifrador`, `Nube` ni `Registro`. Escribí al lado el cuerpo de `hacerBackup()` en 4 líneas.

**A3.** Un cajero automático procesa una extracción. El procedimiento es **siempre el mismo**: validar la tarjeta, verificar el saldo, entregar el efectivo, imprimir el comprobante. Pero para una cuenta en dólares la verificación de saldo aplica cotización, y para una cuenta empresa el comprobante lleva datos fiscales. ¿Qué patrón? **¿Dónde vive el procedimiento fijo y cómo se llama ese método?**

## Bloque B — El contexto de un State

**B1.** *(El ej. C, ahora bien.)* Rehacé el reproductor con los estados **`Detenido`, `Reproduciendo`, `Pausado`**. Quiero ver: los **tres** métodos en `Reproductor` **delegando** (`play`, `pausa`, `stop`), los **tres** métodos de la interfaz recibiendo `r: Reproductor`, y **escrito aparte**: qué hace `Detenido.pausa(r)` (la transición prohibida) y qué hace `Reproduciendo.play(r)` (la acción que no hace nada). Contestá también: *¿cuántos `if` quedaron en `Reproductor`?*

**B2.** Un pedido de e-commerce va **Carrito → Pagado → Enviado → Entregado**. Solo se puede pagar desde *Carrito*, no se puede enviar sin pagar, y desde *Entregado* no se vuelve a ningún lado. Además, `cancelar()` se puede desde *Carrito* y *Pagado*, pero **no** desde *Enviado* ni *Entregado*. Modelalo con State. ¿Dónde vive la regla *"no se puede cancelar un pedido enviado"*?

## Bloque C — Los cuatro desempates de esta clase

Sin dibujar. Una oración cada uno, con **tu** formato del ejercicio C (*"Es X porque [cita del enunciado]. No es Y porque…"*):

1. **CoR vs Decorator** (el del ej. A — este está en el PDF de la cátedra y cae).
2. **Template Method vs Strategy** (el del ej. B).
3. **State vs Strategy** (el del ej. C — **ya lo escribiste bien; copialo de tu propia entrega** y guardalo).
4. **Facade vs Adapter** (el del ej. D).

---

# Soluciones de los ejercicios de refuerzo

> No mires hasta haberlos hecho.

**A1 → Template Method** *(el ej. B completo)*.
- `abstract class Reporte` con el **template method** `generar(): void` **público y concreto** (traer datos → armar encabezado → volcar cada fila → agregar pie → guardar), que **no se redefine nunca**.
- Pasos **fijos** en el padre: `traerDatos()`, `agregarPie(fecha: Date)`.
- Pasos **variables**, `protected abstract`: `armarEncabezado(): string`, `volcarFila(fila: Fila): void`, `guardarArchivo(): void`.
- `ReportePDF`, `ReporteExcel`, `ReporteHTML` redefinen **solo** esos tres.
- **La respuesta a la pregunta:** sin `generar()`, **el orden de los pasos sigue duplicado en las tres subclases** — que es *literalmente* el problema del enunciado ("tres clases con el mismo código copiado y ya se desincronizaron"). El patrón lo arregla **solo** porque el orden vive **una vez** en el padre.

**A2 → Facade** *(el ej. D completo)*.
- `Comando1`, `Comando2`, `Comando3` → **una flecha cada uno** a `BackupFacade`. Ninguna al subsistema.
- `BackupFacade.hacerBackup(datos: Datos): void` → **cuatro** flechas al subsistema (`Compresor`, `Cifrador`, `Nube`, `Registro`).
- El cuerpo, que es todo el patrón:
  ```typescript
  hacerBackup(datos: Datos): void {
    const zip = this.compresor.zip(datos);
    const cifrado = this.cifrador.encriptar(zip);
    this.nube.subir(cifrado);
    this.registro.log("backup ok");
  }
  ```
- **La foto que importa:** cuatro clases de un lado, una puerta, tres clientes que solo la tocan a ella. Si mañana el backup suma un paso, **los comandos no se enteran**.

**A3 → Template Method.**
- **El procedimiento fijo vive en la clase abstracta `Extraccion`**, en el template method **`procesar()`**: validar tarjeta → verificar saldo → entregar efectivo → imprimir comprobante.
- Pasos **variables** (`protected abstract`): `verificarSaldo()` e `imprimirComprobante()`. Los otros dos son concretos en el padre.
- Subclases: `ExtraccionPesos`, `ExtraccionDolares` (cotización en `verificarSaldo()`), `ExtraccionEmpresa` (datos fiscales en `imprimirComprobante()`).
- *No es Strategy:* el esqueleto es **fijo** y solo se rellenan pasos, eligiendo la **subclase** — herencia, no composición.
> El truco del Bloque A: **si el enunciado dice "el procedimiento es siempre el mismo", ese procedimiento es un método concreto en el padre y tiene nombre.** Nombralo y dibujalo. Es el que justifica todo.

**B1 → State** *(el ej. C bien modelado)*.
- **Contexto:** `Reproductor` con `- estado: EstadoReproductor`, `+ play()`, `+ pausa()`, `+ stop()` (los tres **delegan**: `this.estado.play(this)`) y `+ cambiarEstado(e)`.
- **Interfaz:** `EstadoReproductor` con `play(r: Reproductor)`, `pausa(r: Reproductor)`, `stop(r: Reproductor)` — **los tres con `r`**.
- **Estados:** `Detenido`, `Reproduciendo`, `Pausado`.
- Las dos que te pedí escritas:
  ```typescript
  class Detenido implements EstadoReproductor {
    play(r: Reproductor)  { r.cambiarEstado(new Reproduciendo()); }
    pausa(r: Reproductor) { /* transición prohibida: no hace nada */ }
    stop(r: Reproductor)  { /* ya está detenido */ }
  }
  class Reproduciendo implements EstadoReproductor {
    play(r: Reproductor)  { /* ya suena: no hace nada */ }
    pausa(r: Reproductor) { r.cambiarEstado(new Pausado()); }
    stop(r: Reproductor)  { r.cambiarEstado(new Detenido()); }
  }
  ```
- **`Reproductor` tiene CERO `if`.** Los tres `switch` del enunciado desaparecieron: cada rama del switch se volvió **una clase**, y las transiciones prohibidas son **métodos vacíos**. Eso es OCP hecho patrón — un estado nuevo es una clase nueva, sin tocar nada.

**B2 → State.**
- **Contexto:** `Pedido` con `- estado: EstadoPedido`, y `pagar()`, `enviar()`, `entregar()`, `cancelar()` delegando.
- **Interfaz:** `EstadoPedido` con los cuatro métodos recibiendo `p: Pedido`. **Estados:** `Carrito`, `Pagado`, `Enviado`, `Entregado`.
- **Dónde vive la regla:** *"no se puede cancelar un pedido enviado"* vive **adentro de `Enviado.cancelar(p)`**, que no hace nada (o tira una excepción). No hay ningún `if` en `Pedido` preguntando el estado: **cada clase de estado sabe lo suyo**. Esa es la respuesta que buscan: en State las reglas de transición **están distribuidas en los estados**, no centralizadas en el contexto.
> El truco del Bloque B: los estados son **sustantivos de situación** ("está pausado", "está enviado"), nunca las acciones. Y toda regla del tipo *"desde X no se puede Y"* es **un método vacío en la clase X** — no un `if`.
