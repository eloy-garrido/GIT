### Actividad 2: Profundizando en el Flujo de Trabajo

### Ejercicio Guiado - Gestión de Múltiples Archivos y Estado del Repositorio

#### 🎯 Objetivo
El objetivo de esta actividad es que te familiarices con la gestión de ramas (`branch`), los cambios entre ramas (`switch` / `checkout`) y la visualización de los cambios en los archivos.

---

#### 🚀 Pasos de la Actividad

**Paso 1: Configuración inicial**

1.  **Posiciónate en la rama `main`**.
2.  Agrega una línea de texto al archivo `README.md`.
3.  Usa el comando necesario para preparar y guardar los cambios con el mensaje "feat: actualizar README en main".

**Paso 2: Rama `secundaria` y creación de archivos**

1.  **Crea una nueva rama** llamada `secundaria` y muévete a ella.
2.  Crea un archivo llamado `index.html` con un título sencillo.
3.  Prepara y guarda los cambios de este nuevo archivo con el mensaje "feat: crear index.html".

**Paso 3: Rama `estilos`**

1.  **Crea una nueva rama** a partir de la rama actual (`secundaria`) y llámala `estilos`.
2.  Crea un nuevo archivo llamado `style.css` y agrega una regla CSS básica para cambiar el color de fondo de la página.
3.  En el archivo `index.html`, **enlaza el archivo `style.css`** para que los estilos se apliquen.
4.  Prepara y guarda los cambios con el mensaje "feat: agregar estilos basicos".

**Paso 4: Navegación y visualización de cambios**

1.  **Vuelve a la rama `secundaria`**. ¿Qué sucede con el archivo `style.css` y con el enlace en `index.html`? ¿Qué estado tiene tu proyecto ahora?
2.  **Regresa a la rama `main`**. ¿Qué archivos ves ahora? ¿Está `index.html` presente?
3.  **Vuelve a la rama `estilos`** y verifica que todos los archivos (README, index.html y style.css) están presentes y conectados.
4.  Utiliza el comando para **visualizar todas las ramas** con información detallada de los últimos commits para confirmar la estructura de tu proyecto.

---

#### ✅ Verificación y Autoevaluación

Responde a estas preguntas para confirmar tu comprensión:
1.  ¿Por qué al cambiar de rama el archivo `style.css` desaparece del directorio?
2.  ¿Qué pasaría si haces un cambio en `main` mientras tienes archivos sin guardar en la rama `secundaria`?
3.  ¿Cómo visualizas la estructura de tus ramas y los commits que hay en ellas?