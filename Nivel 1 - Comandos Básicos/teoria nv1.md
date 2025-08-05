# Nivel 1: Comandos Básicos de Git

## Introducción

En este primer nivel aprenderemos los comandos fundamentales de Git que todo desarrollador debe conocer. Estos comandos forman la base de cualquier flujo de trabajo con Git y son esenciales para comenzar a versionar nuestro código.

## Comandos a Cubrir

- `git init` - Inicializar un repositorio
- `git add` - Preparar cambios para commit
- `git commit` - Confirmar cambios
- `git log` - Ver historial de commits

---

## 1. git init

### ¿Qué hace?
El comando `git init` inicializa un nuevo repositorio Git en el directorio actual. Crea una carpeta oculta `.git` que contiene toda la estructura necesaria para el control de versiones.

### Sintaxis básica
```bash
git init
git init <nombre-directorio>
```

### Variaciones y opciones

#### Inicializar en directorio actual
```bash
git init
```

#### Inicializar en un nuevo directorio
```bash
git init mi-proyecto
```

#### Especificar rama inicial
```bash
git init --initial-branch=main
git init -b main
```

### Ejemplos prácticos

**Ejemplo 1: Crear un nuevo proyecto**
```bash
# Crear directorio del proyecto
mkdir mi-primer-proyecto
cd mi-primer-proyecto

# Inicializar repositorio Git
git init

# Verificar que se creó el repositorio
ls -la  # Veremos la carpeta .git
```

**Ejemplo 2: Inicializar con rama main**
```bash
git init -b main
```

### Recomendaciones
- Siempre usa `git init -b main` para establecer 'main' como rama principal
- Verifica que el repositorio se inicializó correctamente con `git status`
- No inicialices un repositorio dentro de otro repositorio existente

---

## 2. git add

### ¿Qué hace?
El comando `git add` mueve cambios del área de trabajo (working directory) al área de preparación (staging area). Es el paso previo antes de hacer un commit.

### Sintaxis básica
```bash
git add <archivo>
git add <directorio>
git add .
git add -A
```

### Variaciones y opciones

#### Agregar archivo específico
```bash
git add archivo.txt
```

#### Agregar múltiples archivos
```bash
git add archivo1.txt archivo2.txt
git add *.txt  # Todos los archivos .txt
```

#### Agregar todo el directorio actual
```bash
git add .
```

#### Agregar todos los cambios (incluidos archivos eliminados)
```bash
git add -A
git add --all
```

### Ejemplos prácticos

**Ejemplo 1: Preparar archivos específicos**
```bash
# Crear algunos archivos
echo "Hola mundo" > saludo.txt
echo "Función principal" > main.py

# Agregar archivo específico
git add saludo.txt

# Ver estado
git status
```

**Ejemplo 2: Agregar todos los cambios**
```bash
# Modificar varios archivos
echo "Contenido nuevo" > archivo1.txt
echo "Más contenido" > archivo2.txt

# Agregar todos los cambios
git add .

# Verificar qué se agregó
git status
```

**Ejemplo 3: Agregar interactivamente**
```bash
# Modificar archivo
echo -e "Línea 1\nLínea 2\nLínea 3" > ejemplo.txt

# Agregar por parches (permite seleccionar qué cambios agregar)
git add -p ejemplo.txt
```

### Recomendaciones
- Usa `git add .` para agregar todos los cambios en el directorio actual
- Usa `git add -A` para agregar todos los cambios incluyendo archivos eliminados
- Para proyectos grandes, considera usar `git add -p` para revisar cambios antes de agregarlos
- Siempre revisa `git status` después de `git add` para confirmar qué se agregó

---

## 3. git commit

### ¿Qué hace?
El comando `git commit` toma todos los cambios en el área de preparación y los guarda permanentemente en el historial del repositorio con un mensaje descriptivo.

### Sintaxis básica
```bash
git commit -m "mensaje"
```

### Variaciones y opciones

#### Commit con mensaje en línea
```bash
git commit -m "Agregar función de login"
```

#### Commit abriendo editor para mensaje
```bash
git commit
```

#### Commit con mensaje multilínea
```bash
git commit -m "Título del commit

Descripción detallada del cambio
- Cambio 1
- Cambio 2"
```

#### Commit saltándose el staging (add + commit)
```bash
git commit -a -m "mensaje"
git commit --all -m "mensaje"
```

#### Modificar el último commit
```bash
git commit --amend -m "nuevo mensaje"
```

#### Commit con fecha específica
```bash
git commit --date="2024-01-01 10:00:00" -m "mensaje"
```

### Ejemplos prácticos

**Ejemplo 1: Commit básico**
```bash
# Crear y agregar archivo
echo "print('Hola mundo')" > programa.py
git add programa.py

# Hacer commit
git commit -m "Agregar programa inicial"
```

**Ejemplo 2: Commit con add automático**
```bash
# Modificar archivo existente
echo "print('Hola Git')" >> programa.py

# Add y commit en un paso (solo para archivos ya trackeados)
git commit -a -m "Actualizar mensaje del programa"
```

**Ejemplo 3: Modificar último commit**
```bash
# Hacer commit
git commit -m "Agregar fucnión de calculo"

# Corregir mensaje del commit anterior
git commit --amend -m "Agregar función de cálculo"
```

### Buenas prácticas para mensajes de commit

#### Estructura recomendada
```
Tipo: Descripción breve (máximo 50 caracteres)

Descripción detallada si es necesaria (máximo 72 caracteres por línea)

- Punto específico 1
- Punto específico 2
```

#### Tipos comunes
- `feat:` Nueva funcionalidad
- `fix:` Corrección de bug
- `docs:` Cambios en documentación
- `style:` Cambios de formato (espacios, puntos y comas, etc.)
- `refactor:` Refactorización de código
- `test:` Agregar o modificar tests

### Recomendaciones
- Usa mensajes descriptivos y claros
- Haz commits pequeños y frecuentes
- Un commit debe representar un cambio lógico completo
- Usa el tiempo presente en los mensajes: "Agregar" en lugar de "Agregado"
- Si el commit soluciona un issue, referéncialo: "fix: corregir bug #123"

---

## 4. git log

### ¿Qué hace?
El comando `git log` muestra el historial de commits del repositorio, incluyendo información como autor, fecha, hash del commit y mensaje.

### Sintaxis básica
```bash
git log
git log 
git log --graph
git log -n <número>
```

### Variaciones y opciones

#### Log básico
```bash
git log
```

#### Log condensado (una línea por commit)
```bash
git log 
```

#### Log con gráfico de ramas
```bash
git log --graph
git log --graph 
```

**Ejemplo 5: Log personalizado**
```bash
# Formato: hash corto, autor, fecha relativa, mensaje
git log --pretty=format:"%h - %an, %ar : %s"
```

### Opciones de formato útiles

#### Variables de formato más comunes
- `%H` - Hash completo del commit
- `%h` - Hash corto del commit
- `%an` - Nombre del autor
- `%ae` - Email del autor
- `%ad` - Fecha del autor
- `%ar` - Fecha relativa del autor
- `%s` - Mensaje del commit

#### Ejemplo de formato personalizado
```bash
git log --pretty=format:"%C(yellow)%h%C(reset) %C(blue)%an%C(reset) %C(green)%ar%C(reset) %s"
```

### Recomendaciones
- Usa `git log ` para ver rápidamente el historial
- Combina `--graph ` para visualizar ramificaciones
- Usa `git log -p` para ver los cambios exactos en cada commit
- Para buscar commits específicos, usa filtros por autor, fecha o archivo
- Crea aliases para comandos de log que uses frecuentemente

---

## Flujo de trabajo básico

El flujo típico con estos comandos es:

1. **Inicializar repositorio** (solo una vez)
   ```bash
   git init -b main
   ```

2. **Hacer cambios** en los archivos

3. **Preparar cambios**
   ```bash
   git add .
   ```

4. **Confirmar cambios**
   ```bash
   git commit -m "Descripción del cambio"
   ```

5. **Ver historial**
   ```bash
   git log 
   ```

6. **Repetir pasos 2-5** según sea necesario

## Comandos útiles adicionales

### Verificar estado del repositorio
```bash
git status
```

### Ver diferencias antes de hacer commit
```bash
git diff  # Cambios no preparados
git diff --staged  # Cambios preparados
```

### Configurar usuario (hacer antes del primer commit)
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"
```

## Resumen

En este nivel hemos aprendido los cuatro comandos fundamentales de Git:

- **git init**: Inicializa un repositorio Git
- **git add**: Prepara cambios para commit
- **git commit**: Guarda cambios permanentemente
- **git log**: Muestra el historial de commits

Estos comandos forman la base de cualquier flujo de trabajo con Git. En el siguiente nivel aprenderemos sobre ramificación y navegación entre ramas.
