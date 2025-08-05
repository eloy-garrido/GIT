# Actividad 1 - Parte 1: Mi Primer Repositorio

## Ejercicio Guiado - Flujo Básico de Git

### 🎯 Objetivo

Familiarizarse con los comandos básicos de Git creando y gestionando tu primer repositorio. El objetivo es dominar el flujo fundamental: **`init` → `add` → `commit` → `log`**.

### ⏱️ Tiempo estimado

20-30 minutos

### 📋 Prerrequisitos

* Git instalado y configurado
* IDE instalado

---

## Instrucciones paso a paso

### Paso 1: Preparar el entorno de trabajo

1.  **Crear directorio del proyecto**
    Si aún no tienes la carpeta `Proyectos-Git`, créala primero:
    
    ```bash
    mkdir -p ~/Documentos/Proyectos-Git
    cd ~/Documentos/Proyectos-Git
    ```
    
    Luego, crea y entra en el directorio de la actividad:
    
    ```bash
    mkdir mi-primer-repositorio
    cd mi-primer-repositorio
    ```
    
    **¿Qué esperas ver?** Un directorio vacío.

### Paso 2: Inicializar el repositorio

1.  **Inicializar Git**
    
    ```bash
    git init
    ```
    
    **Explicación necesaria:** ¿Qué archivos/carpetas se crearon? (Pista: usa `ls -a` para ver los archivos ocultos). ¿Qué significa el mensaje de salida?

2.  **Verificar la inicialización**
    
    ```bash
    git status
    ls -a
    ```
    
    **¿Qué observas?** Debe aparecer la carpeta oculta `.git` y un mensaje sobre la rama inicial.

3.  **Ver información del repositorio**
    
    ```bash
    git log
    ```
    
    **¿Qué pasa con `git log`?** Documenta el mensaje de error y explica por qué ocurre.

### Paso 3: Crear tu primer archivo

1.  **Crear archivo README**
    
    ```bash
    echo "# Mi Primer Repositorio Git" > README.md
    ```
    
2.  **Verificar el archivo creado**
    
    ```bash
    ls
    git status
    ```
    
    **Analiza la salida de `git status`:** ¿Qué significa "Untracked files"? Git detecta que hay un nuevo archivo, pero aún no lo está rastreando.

### Paso 4: Preparar cambios (Staging)

1.  **Agregar archivo al staging area**
    
    ```bash
    git add README.md
    ```
    
2.  **Verificar el cambio en staging**
    
    ```bash
    git status
    ```
    
    **¿Qué cambió?** El archivo ahora debe aparecer como "Changes to be committed", lo que significa que está listo para ser guardado.

3.  **Ver diferencias**
    
    ```bash
    git diff --staged
    ```
    
    **Explicación:** La diferencia se muestra con `--staged` porque el archivo ya está en el área de preparación (staging area), esperando ser confirmado.

### Paso 5: Crear tu primer commit

1.  **Hacer el primer commit**
    
    ```bash
    git commit -m "Initial commit: agregar README del proyecto"
    ```
    
    **Analiza la salida:** ¿Qué información te da Git sobre el commit creado?

2.  **Verificar el estado y el historial**
    
    ```bash
    git status
    git log
    ```
    
    **¿Cómo cambió la salida de estos comandos comparado con antes?** Ahora `git log` debería mostrar tu primer commit.

---

## Verificación y autoevaluación

### Preguntas de comprensión

Responde estas preguntas en tu entrega:

1.  **¿Cuál es la diferencia entre un archivo `untracked` y uno en el `staging area`?**
2.  **¿Qué información te da `git log` después de hacer un commit?**
3.  **¿Qué hace `git init` en un directorio?**

---

## Recursos adicionales

### Comandos útiles para recordar

```bash
git status          # Ver estado actual del repositorio
git add archivo     # Mover archivo del área de trabajo al staging area
git commit -m "msg" # Guardar los cambios del staging area en el repositorio
git log             # Ver el historial de commits