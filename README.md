# Home Challenge — Pedidos a la cabina

**De:** Nova (Hangar 3, Estación Umbral)
**Para:** Cadete de turno

Ahora que Mixtape suena, a la cabina de música le llueven pedidos. La gente del turno de noche grita canciones por el pasillo y nadie se acuerda de quién pidió qué. Necesitamos un formulario de pedidos en la app, que no deje pasar pedidos incompletos.

Hoy no creas un proyecto nuevo: **sigues trabajando en el Mixtape de la clase**. Los estilos ya vienen en `estilos-l3.css`; solo falta el JSX. La próxima clase lo usamos otra vez, así que lo que dejes hecho hoy es lo que tendrás mañana.

**Tiempo estimado:** menos de una hora.

> ¿No terminaste los tickets de la clase? Descarga la solución del ejercicio de clase del repo del curso (`REACT/M9/L3`) y arranca desde ahí.

---

## Antes de empezar

1. Abre en VS Code la carpeta de tu Mixtape.
2. En la terminal, arranca el servidor:

```bash
pnpm dev
```

3. Abre la dirección que muestra la terminal (normalmente `http://localhost:5173`) y déjala abierta mientras trabajas.
4. Deja la consola del navegador abierta (F12). Si algo falla, ahí aparece el archivo y la línea.

---

## Tickets

### Ticket 1 — El formulario

**Archivos:** `src/components/PedidoCancion.jsx` (nuevo) y `src/App.jsx`

Crea el componente `PedidoCancion`. Por ahora devuelve este formulario:

```jsx
<form className="pedido">
    <h2 className="seccion-titulo">Pide una canción</h2>

    <label>Canción</label>
    <input type="text" placeholder="¿Qué quieres escuchar?" />

    <label>Tu nombre</label>
    <input type="text" placeholder="¿Quién la pide?" />

    <button type="submit">Enviar pedido</button>
</form>
```

Después, convierte los dos inputs en **inputs controlados**:

1. Crea dos estados de texto que arranquen en `""`: `cancion` y `nombre`.
2. A cada input ponle `value` con su estado y `onChange` con su `set`, como en la teoría.

En `App.jsx`, impórtalo y ponlo debajo de la sección de la playlist, antes de `<Reproductor />`.

**Listo cuando:** el formulario aparece debajo de las tarjetas y puedes escribir en los dos campos. Si un campo no deja escribir, revisa que tenga su `onChange`.

---

### Ticket 2 — Validar antes de enviar

**Archivo:** `src/components/PedidoCancion.jsx`

1. Crea un estado `error` que arranque en `""`.
2. Crea una función `manejarEnvio(e)` y conéctala al `<form>` con `onSubmit`. Adentro:
   - primero `e.preventDefault()`, para que la página no se recargue
   - si `cancion.trim()` tiene menos de 2 letras, guarda en `error` el texto `"El nombre de la canción necesita al menos 2 letras."` y corta con `return`
   - si `nombre.trim()` está vacío, guarda en `error` el texto `"Escribe quién pide la canción."` y corta con `return`
   - si pasó las dos revisiones, limpia el error con `setError("")`
3. Debajo del botón, muestra el error **solo si hay uno** (es "esto o nada"):

```jsx
<p className="pedido-error">{error}</p>
```

4. Deja el botón apagado mientras alguno de los dos campos esté vacío, con `disabled`. Pista: `cancion === ""` da `true` o `false`; necesitas que se apague si **cualquiera** de los dos está vacío.

**Listo cuando:**
- con un campo vacío, el botón se ve gris y no se puede presionar
- con la canción "a" y un nombre, al enviar aparece el error de las 2 letras
- con la canción y el nombre escritos solo con espacios, aparece un error (el botón se activa, pero `trim()` los atrapa)

---

### Ticket 3 — Pedido recibido

**Archivo:** `src/components/PedidoCancion.jsx`

Cuando el pedido pasa la validación, la cabina confirma y el formulario queda limpio para el siguiente.

1. Crea un estado `confirmacion` que arranque en `""`.
2. En `manejarEnvio`, justo después de `e.preventDefault()`, borra la confirmación anterior con `setConfirmacion("")`. Así, si el siguiente pedido tiene un error, no queda el mensaje viejo.
3. Al final de `manejarEnvio`, cuando todo salió bien:
   - guarda en `confirmacion` un texto con los datos del pedido. Usa template literals:

```jsx
setConfirmacion(`Pedido recibido: ${cancion}, para ${nombre}`);
```

   - después, vacía los dos campos con sus `set`
4. Debajo del error, muestra la confirmación solo si hay una:

```jsx
<p className="pedido-exito">{confirmacion}</p>
```

Prueba algo: pon el `setCancion("")` **antes** del `setConfirmacion(...)`. El mensaje sale igual, con la canción completa. Es lo que vimos en la teoría: el `set` no cambia la variable en ese instante, así que durante toda la función `cancion` conserva lo que tenía cuando presionaste el botón.

**Listo cuando:** envías un pedido válido, aparece "Pedido recibido: ..." con la canción y el nombre que escribiste, y los dos campos quedan vacíos.

---

### Bonus — Contador de letras

**Archivo:** `src/components/PedidoCancion.jsx`

La pantalla de la cabina solo muestra títulos de hasta 40 letras.

1. Justo debajo del input de la canción, muestra cuántas letras lleva y el máximo:

```jsx
<span className="contador-letras">12/40</span>
```

El 12 no va escrito a mano: sale de `cancion.length`.

2. Si pasa de 40, el contador se pone rojo. La clase cambia con un ternario: `"contador-letras contador-letras-rojo"` si pasa de 40, y `"contador-letras"` si no.
3. Si pasa de 40, el botón también se apaga. Agrega esa condición a tu `disabled`.

**Listo cuando:** el número sube mientras escribes, y a partir de la letra 41 se pone rojo y el botón se apaga.

---

## Si algo falla

- **`useState is not defined`:** falta `import { useState } from "react";` arriba de `PedidoCancion.jsx`.
- **No puedo escribir en un input:** tiene `value` pero no `onChange`. Van siempre juntos.
- **Al enviar, la página se recarga y todo se borra:** falta `e.preventDefault()`, o la función no recibe la `e`.
- **El error no aparece nunca:** revisa que `onSubmit` esté en el `<form>` y no en el botón.
- **El error sigue en pantalla después de un pedido bueno:** falta el `setError("")` cuando el pedido pasa la validación.
- **`Assignment to constant variable` al enviar:** intentaste vaciar un campo con `cancion = ""`. Un estado solo cambia con su `set`: `setCancion("")`.

---

## Antes de terminar

- [ ] La app corre con `pnpm dev` sin pantalla de error.
- [ ] La consola del navegador (F12) no muestra avisos en rojo.
- [ ] El botón se apaga con un campo vacío.
- [ ] Los dos errores aparecen cuando toca.
- [ ] Un pedido válido muestra la confirmación y deja el formulario limpio.
- [ ] El reproductor de la clase sigue funcionando.
- [ ] Guardaste el proyecto (o lo subiste a tu GitHub): la próxima clase seguimos con él.
