### Actividad 1: Merge y Resolución de Conflictos

## Ejercicio Guiado - Uniendo Cambios y Manejo de Conflictos

### 🎯 Objetivo

Aprenderás a fusionar los cambios de una rama a otra utilizando `git merge` y cómo resolver manualmente los conflictos que surgen cuando Git no puede fusionar automáticamente los cambios.

### 📋 Prerrequisitos

* Haber completado las actividades 1 y 2.
* Dominar los comandos de los niveles 1 y 2: `git init`, `git add`, `git commit`, `git log`, `git branch`, y `git switch`.

---

## 🚀 Instrucciones paso a paso

### Paso 1: Configuración inicial de las ramas

1.  **Posiciónate en la rama `main`** para asegurar que partes del punto inicial correcto.

    ```bash
    git switch main
    ```

2.  **Crea un archivo base** llamado `index.html` con un título y un encabezado.

    ```bash
    echo "<h1>Hola desde la rama main</h1>" > index.html
    ```

3.  **Realiza un commit** de este archivo inicial.

    ```bash
    git add .
    git commit -m "feat: agregar pagina de inicio"
    ```

### Paso 2: Desarrollo de la primera funcionalidad

1.  **Crea una nueva rama** llamada `login` y cámbiate a ella.

    ```bash
    git switch -c login
    ```

2.  **Modifica la página de inicio** agregando un formulario de inicio de sesión.

    ```bash
    echo -e "<h1>Hola desde la rama main</h1>\n<form>Login</form>" > index.html
    ```

3.  **Realiza un commit** con los cambios.

    ```bash
    git add index.html
    git commit -m "feat: agregar formulario de login"
    ```

### Paso 3: Desarrollo de la segunda funcionalidad (que causará un conflicto)

1.  **Vuelve a la rama `main`**.

    ```bash
    git switch main
    ```

2.  **Crea una segunda rama** llamada `home` y cámbiate a ella.

    ```bash
    git switch -c home
    ```

3.  **Modifica el mismo archivo** `index.html` pero de una forma diferente a la rama `login`, por ejemplo, agregando un enlace de navegación.

    ```bash
    echo -e "<h1>Hola desde la rama main</h1>\n<nav>Navegacion</nav>" > index.html
    ```

4.  **Realiza un commit** de estos cambios.

    ```bash
    git add index.html
    git commit -m "feat: agregar navegacion a la pagina"
    ```

### Paso 4: Intento de merge y resolución de conflictos

1.  **Vuelve a la rama `main`**.

    ```bash
    git switch main
    ```

2.  **Intenta fusionar la rama `login`**. Observa que Git realizará un **fast-forward merge** porque `main` no ha tenido nuevos commits.

    ```bash
    git merge login
    ```

3.  **Verifica el historial** con un gráfico para ver la fusión.

    ```bash
    git log --graph --oneline
    ```

4.  **Ahora, intenta fusionar la rama `home`** en `main`. Aquí es donde ocurrirá un conflicto, ya que ambas ramas modificaron la misma parte del archivo `index.html`.

    ```bash
    git merge home
    ```

    **Analiza el mensaje de error:** Git te informará que el merge automático falló y que debes resolver el conflicto manualmente.

5.  **Abre el archivo `index.html`** con tu editor de texto. Verás las marcas de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`).

    ```html
    <<<<<<< HEAD
    <h1>Hola desde la rama main</h1>
    <form>Login</form>
    =======
    <h1>Hola desde la rama main</h1>
    <nav>Navegacion</nav>
    >>>>>>> home
    ```

6.  **Edita el archivo** para que refleje la versión final que quieres. Por ejemplo, mantén ambos elementos y elimina las marcas de conflicto.

    ```html
    <h1>Hola desde la rama main</h1>
    <form>Login</form>
    <nav>Navegacion</nav>
    ```

7.  **Marca el archivo como resuelto** con `git add`.

    ```bash
    git add index.html
    ```

8.  **Completa el merge** haciendo un commit. Git te sugerirá un mensaje de commit que puedes usar.

    ```bash
    git commit -m "Merge branch 'home'"
    ```

9.  **Verifica el historial** nuevamente con un gráfico para ver el commit de merge que se acaba de crear.

    ```bash
    git log --graph --oneline
    ```

---