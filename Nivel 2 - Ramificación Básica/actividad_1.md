# 📝 Actividad Práctica: Nivel 2 - Ramificación y Flujo de Trabajo

Esta actividad te guiará a través de un flujo de trabajo típico de Git, aplicando los comandos del Nivel 1 (`git init`, `git add`, `git commit`, `git log`) y los conceptos de ramificación del Nivel 2 (`git branch`, `git switch`).

## 🎯 Objetivo

El objetivo es simular el desarrollo de una nueva funcionalidad (`feature`) en una rama separada, para luego volver a la rama principal (`main`) sin afectar el código base.

---

## 🚀 Pasos de la Actividad

### Paso 1: Configuración Inicial (Nivel 1)

1.  **Crea un nuevo directorio** para tu proyecto y navega hasta él.
    ```bash
    mkdir proyecto-actividad
    cd proyecto-actividad
    ```
2.  **Inicializa un nuevo repositorio Git** con la rama principal llamada `main`.
    ```bash
    git init -b main
    ```
3.  **Crea el archivo inicial** del proyecto llamado `README.md` y agrégale algún contenido.
    ```bash
    echo "# Proyecto de Actividad" > README.md
    ```
4.  **Prepara y confirma** el archivo inicial.
    ```bash
    git add .
    git commit -m "feat: commit inicial del proyecto"
    ```
5.  **Verifica el historial** para confirmar que el commit se realizó correctamente.
    ```bash
    git log 
    ```

### Paso 2: Creación de la Rama de Funcionalidad (Nivel 2)

1.  **Crea una nueva rama** llamada `user-profile` y cámbiate a ella en un solo paso. Utiliza el comando moderno `git switch`.
    ```bash
    git switch -c user-profile
    ```
2.  **Verifica que estás en la rama correcta**.
    ```bash
    git branch --show-current
    ```

### Paso 3: Desarrollo en la Rama de Funcionalidad (Nivel 1)

1.  **Crea un nuevo archivo** llamado `profile.js` para la nueva funcionalidad.
    ```bash
    echo "function getUserProfile() { /* ... */ }" > profile.js
    ```
2.  **Agrega el nuevo archivo** al área de preparación.
    ```bash
    git add profile.js
    ```
3.  **Realiza un commit** con un mensaje descriptivo para este cambio.
    ```bash
    git commit -m "feat: agregar función para perfil de usuario"
    ```
4.  **Verifica el historial** de commits en esta rama. Observa que el historial de `user-profile` contiene tanto el commit inicial como el nuevo.
    ```bash
    git log 
    ```

### Paso 4: Navegación entre Ramas (Nivel 2)

1.  **Cambia de nuevo a la rama `main`**.
    ```bash
    git switch main
    ```
2.  **Verifica el estado del directorio de trabajo**. ¿Ves el archivo `profile.js`? No debería estar allí.
    ```bash
    ls
    ```
3.  **Verifica el historial** de commits en la rama `main`. ¿Contiene el commit de la nueva funcionalidad? No, el historial de `main` es independiente.
    ```bash
    git log 
    ```

---

## ✅ Verificación y Reflexión

1.  Vuelve a la rama `user-profile` y verifica que el archivo `profile.js` reaparece.
    ```bash
    git switch user-profile
    ls
    ```
2.  ¿Qué pasaría si intentas cambiar a la rama `main` sin haber hecho commit de un cambio en `user-profile`? Intenta modificar un archivo en `user-profile` y luego cambia a `main` para ver el error que te muestra Git.
3.  Utiliza `git branch -v` para ver información útil sobre tus ramas, como el último commit de cada una.
4.  Utiliza `git log  --graph --all` para ver una representación visual del historial de ambas ramas.

Al finalizar esta actividad, habrás practicado el flujo fundamental de Git: inicializar un repositorio, hacer commits, crear una rama para el desarrollo y navegar entre la rama de trabajo y la rama principal de forma segura.