### Actividad 5: Deshaciendo Cambios - Nivel 4

## Ejercicio Guiado - Rescatando el Historial

### 🎯 Objetivo
El objetivo de esta actividad es que practiques cómo deshacer cambios en Git utilizando los comandos clave del Nivel 4: `git reset`, `git restore`, `git revert` y `git stash`. Aprenderás cuándo usar cada uno para corregir errores sin perder trabajo valioso.

---

### 🚀 Instrucciones paso a paso

**Paso 1: Preparar el entorno**

1.  **Crea una nueva rama** llamada `estilos` y cámbiate a ella.

    ```bash
    git switch -c estilos
    ```

2.  **Crea y agrega** un archivo `style.css` y un archivo `bug.js`.

    ```bash
    echo "h1 { color: red; }" > style.css
    echo "function bug() { alert('Esto es un bug'); }" > bug.js
    ```

3.  **Prepara los archivos** para el commit.

    ```bash
    git add .
    ```

**Paso 2: Deshacer cambios locales con `git restore` y `git reset`**

1.  **Elimina un archivo** del área de preparación sin perderlo. Utiliza el comando que se encarga de "restaurar" el área de preparación.

    ```bash
    git restore --staged bug.js
    ```

2.  **Verifica el estado** para confirmar que `bug.js` ya no está preparado. Observa que el archivo aún existe en tu directorio de trabajo.

    ```bash
    git status
    ```

3.  **Realiza un commit** solo con el archivo `style.css`.

    ```bash
    git commit -m "fix: estilos basicos"
    ```

4.  **Haz un cambio local** en `style.css` y luego usa el comando para descartar ese cambio en el archivo de trabajo.

    ```bash
    echo "h1 { color: blue; }" >> style.css
    git restore style.css
    ```

**Paso 3: Usar `git stash` para guardar cambios temporalmente**

1.  **Modifica el archivo `style.css`** nuevamente.

    ```bash
    echo "h1 { color: green; }" >> style.css
    ```

2.  **Guarda estos cambios temporalmente** para poder cambiar de rama sin hacer un commit.

    ```bash
    git stash
    ```

3.  **Vuelve a la rama `main`** para simular que necesitas trabajar en otra cosa.

    ```bash
    git switch main
    ```

4.  **Vuelve a la rama `estilos`** y **aplica los cambios guardados** en el paso anterior.

    ```bash
    git switch estilos
    git stash pop
    ```

**Paso 4: Deshacer un commit con `git revert`**

1.  **Haz un commit** con los cambios de `style.css`.

    ```bash
    git add style.css
    git commit -m "fix: nuevo color verde"
    ```

2.  **Verifica el historial** con el comando `git log --oneline`. Copia el hash del commit que acabas de hacer.
3.  **Usa `git revert`** para crear un nuevo commit que deshace los cambios del commit anterior.

    ```bash
    git revert <hash-del-commit-anterior>
    ```

4.  **Verifica el historial** nuevamente. Observa cómo hay un nuevo commit que revierte el cambio, manteniendo el historial completo.

---