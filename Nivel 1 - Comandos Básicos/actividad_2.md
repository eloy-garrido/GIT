# Actividad 2: Profundizando en el Flujo de Trabajo

## Ejercicio Guiado - Gestión de Múltiples Archivos y Estado del Repositorio

### 🎯 Objetivo

Aprender a gestionar cambios en varios archivos, diferenciar entre archivos rastreados y no rastreados, usar `git diff` para ver los cambios y explorar el historial de commits de forma más avanzada.

### ⏱️ Tiempo estimado

45-60 minutos

### 📋 Prerrequisitos

* Haber completado la Actividad 1 (Flujo Básico)

* Tener un repositorio con al menos un commit (el `README.md` de la actividad anterior)

## Instrucciones paso a paso

### Paso 1: Agregar contenido al proyecto

1. **Entrar al directorio del repositorio**

   ```
   cd ~/Documentos/Proyectos-Git/mi-primer-repositorio
   ```

2. **Expandir el archivo README**
   Agrega más contenido al archivo `README.md`.

   ```
   echo "" >> README.md
   echo "---" >> README.md
   echo "" >> README.md
   echo "## Aprendizajes" >> README.md
   echo "* Flujo básico de Git" >> README.md
   echo "* Crear commits descriptivos" >> README.md
   ```

3. **Crear un nuevo archivo HTML**
   Crea un archivo llamado `index.html` con algo de contenido.

   ```
   echo "<h1>Hola desde mi primera página web</h1>" > index.html
   ```

### Paso 2: Analizar el estado del repositorio

1. **Ver el estado actual**

   ```
   git status
   ```

   **Analiza la salida:** ¿Qué diferencia notas entre `README.md` y `index.html`? Uno aparece como **`modified`** (modificado) y el otro como **`untracked`** (no rastreado). Explica la diferencia en tus propias palabras.

### Paso 3: Entender los cambios con `git diff`

1. **Ver los cambios sin preparar**

   ```
   git diff
   ```

   **Analiza la salida:** ¿Qué te muestra este comando? ¿Por qué `index.html` no aparece? (Pista: el archivo no está rastreado aún).

2. **Preparar el archivo modificado**

   ```
   git add README.md
   ```

3. **Verificar el estado**

   ```
   git status
   ```

   **¿Qué observas ahora?** `README.md` está en el área de preparación (staged) y `index.html` sigue sin ser rastreado.

4. **Ver los cambios en el área de preparación**

   ```
   git diff --staged
   git diff
   ```

   **Compara las dos salidas:** ¿Qué diferencia hay entre `git diff` y `git diff --staged` en este momento? Esto es crucial para entender el flujo de trabajo de Git.

### Paso 4: Ignorar archivos con `.gitignore`

1. **Crear un archivo temporal**

   ```
   echo "Log del servidor" > server.log
   ```

2. **Verificar el estado**

   ```
   git status
   ```

   **¿Qué sucede?** Git lo detecta como un archivo no rastreado.

3. **Crear el archivo `.gitignore`**
   Crea este archivo y dile a Git que ignore los archivos de registro.

   ```
   echo "server.log" > .gitignore
   ```

4. **Verificar el estado de nuevo**

   ```
   git status
   ```

   **¿Qué cambió?** Git ya no detecta `server.log`. ¿Por qué `git status` muestra `.gitignore` como un archivo no rastreado?

### Paso 5: Hacer un segundo commit

1. **Preparar todos los cambios**
   Usa el siguiente comando para agregar el `index.html` y el `.gitignore`.

   ```
   git add .
   git status
   ```

   **Analiza la salida:** ¿Qué hace `git add .`? ¿Cómo se diferencia de `git add <archivo>`?

2. **Crear el segundo commit**

   ```
   git commit -m "feat: agregar página web básica y archivo .gitignore"
   ```

### Paso 6: Explorar el historial de commits

1. **Ver historial de commits**

   ```
   git log
   ```

2. **Ver historial resumido**

   ```
   git log 
   ```

   **Compara las dos salidas:** ¿Cuál es más útil para una vista rápida? ¿Qué información se omite en la versión ``?

3. **Ver las estadísticas de los cambios**

   ```
   git log --stat
   ```

   **¿Qué te muestra este comando?** ¿Puedes ver qué archivos fueron modificados en cada commit?

## Verificación y autoevaluación

### Preguntas de comprensión

Responde estas preguntas en tu entrega:

1. **¿Cuál es el propósito del archivo `.gitignore`?**

2. **Describe la diferencia en lo que muestran `git diff` y `git diff --staged`.**

3. **¿Cuándo usarías `git log` y cuándo `git log `?**

4. **¿Qué es el "área de preparación" y por qué es una etapa útil en el flujo de trabajo?**

## Recursos adicionales

### Comandos útiles en esta actividad

```
git diff          # Muestra cambios sin preparar
git diff --staged # Muestra cambios en el área de preparación
git add .         # Prepara todos los cambios en el directorio actual
git log  # Muestra el historial en una sola línea por commit
git log --stat    # Muestra las estadísticas de archivos en el historial
