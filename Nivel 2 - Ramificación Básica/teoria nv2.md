# Nivel 2: Ramificación Básica - git branch, git checkout, git switch

## Introducción

En este segundo nivel aprenderemos sobre uno de los conceptos más poderosos de Git: las ramas (branches). Las ramas nos permiten trabajar en diferentes funcionalidades de forma paralela sin afectar el código principal, facilitando el desarrollo colaborativo y la experimentación segura.

## Prerrequisitos

Antes de comenzar este nivel, debes dominar:
- `git init` - Inicializar repositorios
- `git add` - Preparar cambios
- `git commit` - Confirmar cambios
- `git log` - Ver historial

## Comandos a Cubrir

- `git branch` - Gestionar ramas
- `git checkout` - Cambiar entre ramas (comando clásico)
- `git switch` - Cambiar entre ramas (comando moderno)

---

## Concepto: ¿Qué es una rama?

Una **rama** en Git es una línea independiente de desarrollo. Imagina que el código principal (rama `main`) es como el tronco de un árbol, y las ramas son como las ramas del árbol que crecen desde el tronco.

### ¿Por qué usar ramas?

1. **Desarrollo paralelo**: Varios desarrolladores pueden trabajar en funcionalidades diferentes
2. **Experimentación segura**: Puedes probar cambios sin afectar el código principal
3. **Organización**: Cada funcionalidad o corrección puede tener su propia rama
4. **Colaboración**: Facilita el trabajo en equipo y la revisión de código

### Visualización conceptual
```
main:     A---B---C---F---G
               \       /
feature:        D---E
```

---

## 1. git branch

### ¿Qué hace?
El comando `git branch` se usa para listar, crear, eliminar y gestionar ramas en el repositorio.

### Sintaxis básica
```bash
git branch                    # Listar ramas
git branch <nombre-rama>      # Crear rama
git branch -d <nombre-rama>   # Eliminar rama
```

### Variaciones y opciones

#### Listar ramas
```bash
# Listar ramas locales
git branch

# Listar todas las ramas (locales y remotas)
git branch -a

# Listar ramas remotas
git branch -r

# Listar con información adicional
git branch -v
git branch -vv  # Con información de tracking
```

#### Crear ramas
```bash
# Crear nueva rama desde el commit actual
git branch nueva-funcionalidad

# Crear rama desde un commit específico
git branch hotfix abc123

# Crear rama desde otra rama
git branch login main
```

#### Eliminar ramas
```bash
# Eliminar rama (solo si está merged)
git branch -d nombre-rama

# Forzar eliminación de rama (aunque no esté merged)
git branch -D nombre-rama

# Eliminar múltiples ramas
git branch -d rama1 rama2 rama3
```

#### Renombrar ramas
```bash
# Renombrar rama actual
git branch -m nuevo-nombre

# Renombrar rama específica
git branch -m nombre-viejo nombre-nuevo
```

#### Información sobre ramas
```bash
# Ver rama actual
git branch --show-current

# Ver ramas merged
git branch --merged

# Ver ramas no merged
git branch --no-merged
```

### Ejemplos prácticos

**Ejemplo 1: Crear y listar ramas**
```bash
# Ver ramas actuales
git branch

# Crear nueva rama
git branch calculadora

# Ver ramas después de crear
git branch

# Ver información detallada
git branch -v
```

**Ejemplo 2: Gestión de ramas**
```bash
# Crear varias ramas
git branch login
git branch registro
git branch hotfix/bug-crítico

# Listar todas las ramas
git branch

# Eliminar rama no necesaria
git branch -d hotfix/bug-crítico
```

### Recomendaciones
- Usa nombres descriptivos para las ramas: `login`, `fix/bug-123`, `docs/readme`
- Mantén las ramas actualizadas con la rama principal
- Elimina ramas que ya no necesites para mantener el repositorio limpio
- Usa `git branch -v` para ver información útil sobre tus ramas

---

## 2. git checkout

### ¿Qué hace?
`git checkout` es un comando multipropósito que tradicionalmente se usa para cambiar entre ramas, crear nuevas ramas, y restaurar archivos. Es el comando clásico de Git.

**Nota**: A partir de Git 2.23, se introdujeron `git switch` y `git restore` para separar las funcionalidades de `checkout` y hacerlo menos confuso.

### Sintaxis básica
```bash
git checkout <rama>           # Cambiar a rama
git checkout -b <nueva-rama>  # Crear y cambiar a nueva rama
git checkout <archivo>        # Restaurar archivo
```

### Variaciones y opciones

#### Cambiar entre ramas
```bash
# Cambiar a rama existente
git checkout main
git checkout login

# Cambiar a rama remota
git checkout origin/main
```

#### Crear y cambiar a nueva rama
```bash
# Crear rama y cambiar a ella
git checkout -b nueva-funcionalidad

# Crear rama desde commit específico
git checkout -b hotfix abc123

# Crear rama desde otra rama
git checkout -b api main
```

#### Restaurar archivos
```bash
# Restaurar archivo específico
git checkout archivo.txt

# Restaurar archivo desde otra rama
git checkout main -- archivo.txt

# Restaurar todos los archivos
git checkout .
```

#### Cambiar a commit específico
```bash
# Cambiar a commit específico (modo DETACHED HEAD)
git checkout abc123

# Crear rama desde commit específico
git checkout -b nueva-rama abc123
```

### Ejemplos prácticos

**Ejemplo 1: Flujo básico de ramas**
```bash
# Estar en main
git checkout main

# Crear y cambiar a nueva rama
git checkout -b calculadora

# Hacer algunos cambios y commits
echo "print('Nueva función')" > nueva_funcion.py
git add nueva_funcion.py
git commit -m "feat: agregar nueva función"

# Volver a main
git checkout main

# Ver que el archivo no está en main
ls nueva_funcion.py  # No existe en main
```

**Ejemplo 2: Trabajo con múltiples ramas**
```bash
# Crear rama de funcionalidad
git checkout -b login

# Hacer trabajo en esta rama
echo "def login():" > login.py
git add login.py
git commit -m "feat: implementar función login"

# Crear otra rama desde main
git checkout main
git checkout -b registro

# Hacer trabajo en esta rama
echo "def registro():" > registro.py
git add registro.py
git commit -m "feat: implementar función registro"

# Cambiar entre ramas para ver diferencias
git checkout login
ls  # Solo veremos login.py

git checkout registro
ls  # Solo veremos registro.py
```

### Problemas comunes con checkout

#### DETACHED HEAD
Cuando haces `git checkout` a un commit específico:
```bash
git checkout abc123
```
Entras en estado "DETACHED HEAD". Esto significa que no estás en ninguna rama. Si haces commits aquí, pueden perderse.

**Solución**: Crear una rama
```bash
git checkout -b nueva-rama-desde-commit
```

#### Cambios no guardados
Si tienes cambios no guardados, checkout puede fallar:
```bash
# Error: tienes cambios no guardados
git checkout otra-rama
```

**Soluciones**:
```bash
# Opción 1: Guardar cambios
git add .
git commit -m "wip: trabajo en progreso"

# Opción 2: Descartar cambios
git checkout .

# Opción 3: Stash (veremos en nivel 4)
git stash
git checkout otra-rama
```

### Recomendaciones
- Usa `git checkout -b` para crear y cambiar a nueva rama en un paso
- Siempre verifica en qué rama estás con `git branch` o `git status`
- Evita usar checkout en commits específicos a menos que necesites crear una rama desde ahí
- Considera usar `git switch` (más moderno) en lugar de checkout para cambiar ramas

---

## 3. git switch

### ¿Qué hace?
`git switch` es el comando moderno introducido en Git 2.23 para cambiar entre ramas de forma más clara y segura. Se diseñó para reemplazar la funcionalidad de cambio de ramas de `git checkout`.

### ¿Por qué git switch?

1. **Más claro**: Se enfoca solo en cambiar ramas
2. **Más seguro**: Menos propenso a errores
3. **Mejor UX**: Mensajes de error más claros
4. **Separación de responsabilidades**: No mezcla cambio de ramas con restauración de archivos

### Sintaxis básica
```bash
git switch <rama>           # Cambiar a rama
git switch -c <nueva-rama>  # Crear y cambiar a nueva rama
git switch -               # Cambiar a rama anterior
```

### Variaciones y opciones

#### Cambiar entre ramas
```bash
# Cambiar a rama existente
git switch main
git switch login

# Cambiar a rama anterior (como cd -)
git switch -
```

#### Crear y cambiar a nueva rama
```bash
# Crear rama y cambiar a ella
git switch -c nueva-funcionalidad

# Crear rama desde otra rama
git switch -c api main

# Crear rama desde commit específico
git switch -c hotfix abc123
```

#### Opciones adicionales
```bash
# Cambiar rama descartando cambios locales
git switch --discard-changes rama

# Crear rama aunque ya exista (forzar)
git switch -C rama-existente

# Crear rama orphan (sin historial)
git switch --orphan nueva-rama-limpia
```

### Comparación: checkout vs switch

| Acción | git checkout | git switch |
|--------|-------------|------------|
| Cambiar a rama | `git checkout rama` | `git switch rama` |
| Crear y cambiar | `git checkout -b nueva` | `git switch -c nueva` |
| Rama anterior | No disponible | `git switch -` |
| Seguridad | Menos seguro | Más seguro |
| Claridad | Multipropósito | Específico |

### Ejemplos prácticos

**Ejemplo 1: Flujo básico con switch**
```bash
# Ver rama actual
git branch

# Cambiar a main
git switch main

# Crear y cambiar a nueva rama
git switch -c dashboard

# Hacer trabajo
echo "Dashboard HTML" > dashboard.html
git add dashboard.html
git commit -m "feat: crear dashboard inicial"

# Cambiar a main
git switch main

# Volver rápidamente a la rama anterior
git switch -
```

**Ejemplo 2: Gestión avanzada de ramas**
```bash
# Crear múltiples ramas para diferentes funcionalidades
git switch -c auth
echo "Sistema de autenticación" > auth.py
git add auth.py
git commit -m "feat: base del sistema de auth"

# Crear otra rama desde main
git switch main
git switch -c database
echo "Conexión a base de datos" > db.py
git add db.py
git commit -m "feat: configurar conexión DB"

# Cambiar entre ramas rápidamente
git switch auth
git switch database
git switch main

# Ver todas las ramas
git branch -v
```

### Ventajas de git switch

1. **Mensajes de error más claros**
```bash
# Con checkout (confuso)
git checkout rama-inexistente
# error: pathspec 'rama-inexistente' did not match any file(s) known to git

# Con switch (claro)
git switch rama-inexistente
# fatal: invalid reference: rama-inexistente
```

2. **Protección contra errores**
```bash
# switch es más estricto con cambios no guardados
git switch otra-rama
# error: Your local changes would be overwritten by checkout.
# Please commit your changes or stash them before you switch branches.
```

3. **Funcionalidad específica**
```bash
# switch solo cambia ramas, no restaura archivos
git switch archivo.txt
# fatal: a branch is expected, got 'archivo.txt'
```

### Recomendaciones
- **Usa `git switch` en lugar de `git checkout`** para cambiar ramas (es más moderno y seguro)
- Usa `git switch -c` para crear y cambiar a nueva rama
- Aprovecha `git switch -` para alternar rápidamente entre dos ramas
- Si usas Git 2.23 o superior, adopta `git switch` como tu comando principal

---

## Flujo de trabajo con ramas

### Flujo típico de desarrollo

1. **Partir desde main actualizada**
   ```bash
   git switch main
   ```

2. **Crear rama de funcionalidad**
   ```bash
   git switch -c nueva-funcionalidad
   ```

3. **Desarrollar en la rama**
   ```bash
   # Hacer cambios
   git add .
   git commit -m "feat: implementar nueva funcionalidad"
   ```

4. **Cambiar a main cuando termines**
   ```bash
   git switch main
   ```

5. **Integrar cambios** (veremos en Nivel 3)
   ```bash
   git merge nueva-funcionalidad
   ```

6. **Limpiar rama**
   ```bash
   git branch -d nueva-funcionalidad
   ```

### Convenciones de nombres de ramas

#### Prefijos recomendados
- `` - Nuevas funcionalidades
- `fix/` o `` - Corrección de bugs
- `hotfix/` - Correcciones urgentes
- `docs/` - Cambios en documentación
- `style/` - Cambios de formato/estilo
- `refactor/` - Refactorización de código
- `test/` - Agregar o modificar tests

#### Ejemplos de nombres descriptivos
```bash
user-authentication
fix/login-redirect-bug
hotfix/security-vulnerability
docs/api-documentation
style/navbar-responsive
refactor/database-connection
test/user-registration
```

### Comandos útiles para ramas

#### Ver información sobre ramas
```bash
# Ver todas las ramas con info
git branch -a -v

# Ver última actividad en ramas
git for-each-ref --sort=-committerdate refs/heads/

# Ver ramas merged/no-merged
git branch --merged
git branch --no-merged
```

#### Limpieza de ramas
```bash
# Eliminar ramas merged
git branch --merged | grep -v main | xargs git branch -d

# Ver ramas que se pueden eliminar
git branch --merged
```

---

## Conceptos importantes

### HEAD
**HEAD** es un puntero que indica dónde estás actualmente en el repositorio. Normalmente apunta a la rama actual.

```bash
# Ver dónde apunta HEAD
cat .git/HEAD

# HEAD apunta a rama
ref: refs/heads/main

# En modo DETACHED HEAD apunta directamente a commit
abc123456789...
```

### Ramas como punteros
Las ramas en Git son simplemente **punteros móviles** a commits específicos. Cuando haces un commit en una rama, el puntero se mueve al nuevo commit.

```
A---B---C  (main)
     \
      D---E  (feature)
```

### Working Directory vs Staging vs Repository
Cuando cambias de rama, Git actualiza tres áreas:
1. **Working Directory**: Los archivos que ves
2. **Staging Area**: Se limpia
3. **Repository**: HEAD apunta a la nueva rama

---

## Resolución de problemas comunes

### Problema 1: No puedo cambiar de rama
```bash
git switch otra-rama
# error: Your local changes would be overwritten
```

**Soluciones**:
```bash
# Opción 1: Commit cambios
git add .
git commit -m "wip: guardar progreso"

# Opción 2: Stash cambios (Nivel 4)
git stash
git switch otra-rama

# Opción 3: Descartar cambios
git restore .
git switch otra-rama
```

### Problema 2: DETACHED HEAD
```bash
git switch abc123
# You are in 'detached HEAD' state
```

**Solución**:
```bash
# Crear rama desde aquí
git switch -c nueva-rama-desde-commit

# O volver a una rama
git switch main
```

### Problema 3: Rama ya existe
```bash
git switch -c login
# fatal: A branch named 'login' already exists
```

**Soluciones**:
```bash
# Cambiar a rama existente
git switch login

# Forzar creación (elimina rama existente)
git switch -C login
```

---

## Comandos de diagnóstico útiles

```bash
# Ver estado actual
git status

# Ver en qué rama estás
git branch --show-current

# Ver historial con ramas
git log  --graph --all

# Ver diferencias entre ramas
git diff main..login

# Ver commits únicos de una rama
git log main..login 
```

---

## Resumen

En este nivel hemos aprendido sobre ramificación básica:

### Comandos principales
- **git branch**: Listar, crear y eliminar ramas
- **git checkout**: Comando clásico para cambiar ramas (y más)
- **git switch**: Comando moderno y seguro para cambiar ramas

### Conceptos clave
- Las ramas permiten desarrollo paralelo
- HEAD indica dónde estás actualmente
- Las ramas son punteros móviles a commits
- `git switch` es más seguro que `git checkout` para cambiar ramas

### Flujo de trabajo
1. Crear rama desde main: `git switch -c nueva`
2. Desarrollar en la rama: commits normales
3. Cambiar a main: `git switch main`
4. Integrar cambios: merge (Nivel 3)
5. Limpiar: `git branch -d nueva`

En el siguiente nivel aprenderemos sobre merge, conflictos y el estado DETACHED HEAD en detalle.
