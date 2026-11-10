# Plan Estratégico Anual · guía de instalación

Programa web para el plan estratégico anual con metodología Scaling Up. La página se publica gratis en **GitHub Pages** y los datos se guardan en **Supabase**, una base de datos en la nube con inicio de sesión.

Tus datos se quedan guardados siempre, hasta que tú los borres, y se protegen así:

- **Guardado automático en la nube** menos de un segundo después de cada cambio.
- **Cambios en vivo:** si dos personas trabajan a la vez, cada una ve lo que escribe la otra.
- **Si se cae el internet**, el programa guarda una copia en tu computadora y la sube sola al reconectar. Si intentas cerrar la ventana con cambios pendientes, el navegador te avisa.
- **Historial automático:** se guarda una versión cada 15 minutos mientras trabajas, y siempre antes de borrar un año. Puedes restaurar cualquiera desde el programa.
- **Respaldos descargables:** un archivo con todos los años, para guardar en tu computadora o en Drive.

Tiempo estimado de instalación: 30 a 40 minutos. No necesitas programar.

---

## Qué trae esta carpeta

| Archivo | Para qué sirve |
|---|---|
| `index.html` | El programa completo |
| `config.js` | Donde pegas los datos de conexión de tu proyecto de Supabase |
| `supabase/schema.sql` | El script que crea la base de datos, la seguridad y el historial |
| `ejemplos/respaldo-ejemplo-marca-ciclismo.json` | Datos de ejemplo (2025 y 2026) para ver cómo funciona; es opcional |
| `.nojekyll` | Archivo técnico para GitHub Pages; súbelo también |

---

## Parte 1 · Crear la base de datos en Supabase

1. Entra a **supabase.com** y crea una cuenta. Puedes entrar con tu cuenta de GitHub.
2. Pulsa **New project**:
   - **Name:** `plan-estrategico`
   - **Database password:** crea una contraseña segura y guárdala. No la vas a necesitar en el programa, pero es la llave maestra de tu base de datos.
   - **Region:** elige la más cercana, por ejemplo *East US*.
   - Pulsa **Create new project** y espera a que termine de prepararse (1 a 2 minutos).
3. **Crea las tablas:**
   - En el menú de la izquierda, abre **SQL Editor** y pulsa **New query**.
   - Abre el archivo `supabase/schema.sql`, copia **todo** y pégalo.
   - Al final del script, cambia `tu-correo@empresa.com` y `Tu nombre` por tu correo y tu nombre. Escribe el correo **en minúsculas**.
   - Pulsa **Run**. Debe decir *Success*.
4. **Cierra el registro público,** para que nadie pueda crearse una cuenta por su cuenta:
   - Ve a **Authentication → Sign In / Providers** (en algunas versiones: *Authentication → Settings*).
   - Desactiva **Allow new users to sign up** y guarda.
   - Deja activado el proveedor **Email**.
5. **Crea tu usuario:**
   - Ve a **Authentication → Users → Add user → Create new user**.
   - Escribe el mismo correo del paso 3 y una contraseña.
   - Marca **Auto Confirm User** y pulsa **Create user**.
6. **Copia los datos de conexión:**
   - Ve a **Project Settings → API** (o **API Keys**).
   - Copia la **Project URL** (algo como `https://abcdefgh.supabase.co`).
   - Copia la clave **anon public**. Si tu panel muestra una **Publishable key** (empieza con `sb_publishable_`), esa también sirve.
   - ⚠️ **Nunca** uses la clave `service_role` ni la `secret`.
7. Abre `config.js` con el Bloc de notas (o TextEdit en Mac) y pega los dos valores entre las comillas:

   ```js
   window.PLAN_CONFIG = {
     supabaseUrl: "https://abcdefgh.supabase.co",
     supabaseAnonKey: "eyJhbGciOi...tu-clave..."
   };
   ```

   Guarda el archivo.

> **¿Es seguro publicar la clave "anon"?** Sí. Está hecha para usarse en páginas web. Sin un usuario y contraseña válidos, y sin estar en la tabla `miembros`, la base de datos no deja leer ni escribir nada.

---

## Parte 2 · Publicar la página en GitHub Pages

1. Entra a **github.com** y crea una cuenta si no tienes una.
2. Pulsa **New repository**:
   - **Repository name:** `plan-estrategico`
   - Elige **Public**. En el plan gratuito, GitHub Pages solo funciona con repositorios públicos. El código queda visible, pero **tus datos no**, porque viven en Supabase protegidos por contraseña.
   - Pulsa **Create repository**.
3. En el repositorio nuevo, pulsa **uploading an existing file** (o **Add file → Upload files**).
4. Arrastra **todo el contenido** de esta carpeta: `index.html`, `config.js`, `.nojekyll` y las carpetas `supabase` y `ejemplos`.
   - Si no ves `.nojekyll` porque está oculto, puedes omitirlo; normalmente funciona sin él.
5. Abajo, pulsa **Commit changes**.
6. Ve a **Settings → Pages**:
   - En **Source**, elige **Deploy from a branch**.
   - En **Branch**, elige `main` y la carpeta `/ (root)`, y pulsa **Save**.
7. Espera 1 o 2 minutos y recarga la página. Arriba aparece tu dirección, por ejemplo:
   `https://tuusuario.github.io/plan-estrategico/`

---

## Parte 3 · Conectar la recuperación de contraseña

1. En Supabase, ve a **Authentication → URL Configuration**.
2. En **Site URL**, pega tu dirección de GitHub Pages (`https://tuusuario.github.io/plan-estrategico/`).
3. En **Redirect URLs**, agrega la misma dirección y guarda.

Así funciona el enlace **"Olvidé mi contraseña"** de la pantalla de entrada.

---

## Parte 4 · Primer uso

1. Abre tu dirección de GitHub Pages y entra con tu correo y contraseña.
2. El programa abre el año en curso en blanco. Empieza por la pestaña **Estrategia**. El año se guarda en cuanto escribes algo.
3. Si quieres ver primero cómo se ve con datos, ve a **Años y respaldos → Importar respaldo** y elige `ejemplos/respaldo-ejemplo-marca-ciclismo.json`. Para quitarlos después, usa el botón **Borrar ejemplos y empezar mi plan** de la pestaña **Tablero**.
   - ⚠️ Si importas los ejemplos cuando ya tienes datos en 2025 o 2026, esos años se reemplazan por los de ejemplo. La versión anterior queda en el historial, pero lo más sencillo es importar los ejemplos solo antes de empezar.

---

## Agregar a tu equipo

Cada persona necesita **dos cosas**:

1. **Un usuario en Supabase:** **Authentication → Users → Add user**, con su correo y una contraseña temporal, y la casilla **Auto Confirm User** marcada.
2. **Estar en la lista de miembros:** en **SQL Editor**, ejecuta:

   ```sql
   insert into public.miembros (email, nombre, rol)
   values ('correo@empresa.com', 'Nombre Apellido', 'editor');
   ```

   - `editor`: puede escribir, crear años, importar y restaurar versiones.
   - `lector`: solo puede ver.

   También puedes hacerlo sin SQL: **Table Editor → miembros → Insert row**.

Para quitarle el acceso a alguien, borra su fila de `miembros` y su usuario en **Authentication → Users**.

---

## Recuperar información

- **Una versión anterior de un año:** **Años y respaldos → Versiones anteriores → Restaurar**.
- **Un año que borraste:** **Años y respaldos → Años eliminados → Recuperar**.
- **Desde un archivo de respaldo:** **Años y respaldos → Importar respaldo**.

**Recomendación:** descarga un respaldo una vez al mes (**Años y respaldos → Descargar respaldo**) y guárdalo en tu Drive.

---

## Notas del plan gratuito

- **Supabase** gratuito alcanza de sobra para este uso. Si el proyecto pasa una semana sin que nadie lo use, Supabase lo **pausa**. Tus datos **no se borran**: entras al panel de Supabase y pulsas **Restore project**. Usarlo cada semana evita la pausa.
- **GitHub Pages** es gratuito y no tiene límites relevantes para este programa.

---

## Problemas comunes

| Lo que ves | Qué hacer |
|---|---|
| "Falta conectar el programa con Supabase" | Revisa que `config.js` tenga la URL y la clave entre comillas, y vuelve a subirlo a GitHub |
| "Correo o contraseña incorrectos" | Revisa el usuario en **Authentication → Users**; debe estar confirmado (*Auto Confirm*) |
| "Tu correo no está en la lista de miembros" | Agrega el correo, **en minúsculas**, a la tabla `miembros` |
| "Sin conexión · se guardará al reconectar" | No cierres la ventana; en cuanto vuelve el internet se guarda solo |
| No ves los cambios de otra persona | Recarga la página. Si sigue igual, vuelve a ejecutar el paso 7 de `schema.sql` (Realtime) |
| La página de GitHub muestra error 404 | Espera 2 minutos y revisa **Settings → Pages**: rama `main`, carpeta `/ (root)` |

## Actualizar el programa

Si después recibes una versión nueva de `index.html`, súbela en GitHub con **Add file → Upload files** reemplazando la anterior. Tus datos no se tocan, porque viven en Supabase.
