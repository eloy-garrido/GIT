# Nivel 4: Revertir Cambios - git reset, git restore, git revert, git stash

## Introducción

En este nivel final aprenderemos las herramientas más importantes para deshacer cambios en Git. Cada comando tiene propósitos específicos y es crucial entender cuándo usar cada uno. Estos comandos son fundamentales para recuperarse de errores y mantener un historial limpio.

## Prerrequisitos

Antes de comenzar este nivel, debes dominar:
- Comandos básicos: `git init`, `git add`, `git commit`, `git log`
- Manejo de ramas: `git branch`, `git checkout`, `git switch`
- Merge y resolución de conflictos

## Comandos a Cubrir

- **git reset** - Resetear el estado del repositorio
- **git restore** - Restaurar archivos (comando moderno)
- **git revert** - Crear commits que deshacen cambios anteriores
- **git stash** - Guardar temporalmente cambios sin hacer commit

---

## Conceptos fundamentales

### Las tres áreas de Git

Para entender estos comandos, es crucial comprender las tres áreas principales:

1. **Working Directory** (Directorio de trabajo): Los archivos que ves y editas
2. **Staging Area** (Área de preparación): Cambios preparados para commit
3. **Repository** (Repositorio): Commits confirmados en el historial

```
Working Directory  -->  Staging Area  -->  Repository
     (git add)              (git commit)
```

### Estados de los archivos

- **Untracked**: Archivos nuevos que Git no rastrea
- **Modified**: Archivos rastreados que han cambiado
- **Staged**: Archivos preparados para commit
- **Committed**: Archivos guardados en el repositorio

---

## 1. git reset

### ¿Qué hace?
`git reset` mueve el HEAD y puede cambiar el estado del staging area y working directory. Es una herramienta poderosa pero potencialmente destructiva.

### Sintaxis básica
```bash
git reset <modo> <commit>
git reset <archivo>
git reset HEAD~<número>
```

### Tipos de reset

#### 1. --soft: Solo mueve HEAD
```bash
git reset --soft HEAD~1
```
- Mueve HEAD al commit anterior
- Mantiene staging area intacta
- Mantiene working directory intacto
- **Uso**: Deshacer commit pero mantener cambios preparados

#### 2. --mixed (por defecto): Mueve HEAD y limpia staging
```bash
git reset HEAD~1
git reset --mixed HEAD~1  # Equivalente
```
- Mueve HEAD al commit anterior
- Limpia staging area
- Mantiene working directory intacto
- **Uso**: Deshacer commit y deshacer add, pero mantener cambios en archivos

#### 3. --hard: Resetea todo
```bash
git reset --hard HEAD~1
```
- Mueve HEAD al commit anterior
- Limpia staging area
- **DESTRUYE** cambios en working directory
- **Uso**: Eliminar completamente cambios (¡CUIDADO!)

### Ejemplos prácticos

**Ejemplo 1: Deshacer último commit (mantener cambios)**
```bash
# Situación: hiciste commit pero quieres modificar algo
git log  -3
# abc1234 Último commit (quiero deshacer este)
# def5678 Commit anterior
# ghi9012 Commit más viejo

# Deshacer commit pero mantener cambios
git reset --soft HEAD~1

# Verificar estado
git status
# Changes to be committed: (tus archivos están aquí)

# Ahora puedes modificar y hacer commit de nuevo
```

**Ejemplo 2: Deshacer add (unstage archivos)**
```bash
# Agregaste archivos por error
git add archivo1.txt archivo2.txt archivo_incorrecto.txt
git status

# Quitar archivo específico del staging
git reset archivo_incorrecto.txt

# O quitar todos los archivos del staging
git reset
```

**Ejemplo 3: Reset destructivo (CUIDADO)**
```bash
# Eliminar completamente los últimos 2 commits
git reset --hard HEAD~2

# ¡Los cambios se pierden permanentemente!
# Solo usar si estás completamente seguro
```

**Ejemplo 4: Reset a commit específico**
```bash
# Ver historial
git log 
# abc1234 Commit reciente
# def5678 Commit que quiero mantener
# ghi9012 Commit más viejo

# Reset a commit específico
git reset --mixed def5678

# Ahora HEAD apunta a def5678
# Los cambios de abc1234 están en working directory
```

### Cuándo usar cada tipo

| Situación | Comando | Resultado |
|-----------|---------|-----------|
| Deshacer commit, rehacer mejor | `git reset --soft HEAD~1` | Cambios en staging |
| Deshacer commit y add | `git reset HEAD~1` | Cambios en working dir |
| Eliminar cambios completamente | `git reset --hard HEAD~1` | Todo eliminado |
| Quitar archivo del staging | `git reset archivo.txt` | Solo unstage archivo |

### ⚠️ Advertencias importantes

1. **--hard es destructivo**: Los cambios se pierden permanentemente
2. **Solo en repositorios locales**: No uses reset en commits ya pushed
3. **Usa reflog para recuperar**: `git reflog` puede ayudar a recuperar commits "perdidos"

---

## 2. git restore

### ¿Qué hace?
`git restore` es el comando moderno (Git 2.23+) para restaurar archivos. Reemplaza algunas funcionalidades de `git checkout` y `git reset` de forma más clara y segura.

### Sintaxis básica
```bash
git restore <archivo>                    # Restaurar working directory
git restore --staged <archivo>          # Restaurar staging area
git restore --source=<commit> <archivo> # Restaurar desde commit específico
```

### Opciones principales

#### Restaurar working directory (deshacer cambios locales)
```bash
# Deshacer cambios en archivo específico
git restore archivo.txt

# Deshacer cambios en todos los archivos
git restore .

# Deshacer cambios en directorio
git restore src/
```

#### Restaurar staging area (unstage)
```bash
# Quitar archivo del staging
git restore --staged archivo.txt

# Quitar todos los archivos del staging
git restore --staged .
```

#### Restaurar ambos (staging y working directory)
```bash
# Restaurar completamente (equivale a checkout)
git restore --staged --worktree archivo.txt

# Forma corta
git restore -SW archivo.txt
```

#### Restaurar desde commit específico
```bash
# Restaurar archivo desde commit anterior
git restore --source=HEAD~1 archivo.txt

# Restaurar desde commit específico
git restore --source=abc1234 archivo.txt

# Restaurar desde otra rama
git restore --source=main archivo.txt
```

### Ejemplos prácticos

**Ejemplo 1: Deshacer cambios locales**
```bash
# Modificar archivo
echo "Cambio que no quiero" >> importante.txt
git status
# modified: importante.txt

# Deshacer cambios
git restore importante.txt

# Verificar que se deshizo
git status
# nothing to commit, working tree clean
```

**Ejemplo 2: Unstage archivos**
```bash
# Agregar archivos al staging
git add archivo1.txt archivo2.txt
git status
# Changes to be committed:
#   new file: archivo1.txt
#   new file: archivo2.txt

# Quitar uno del staging
git restore --staged archivo2.txt

# Verificar estado
git status
# Changes to be committed:
#   new file: archivo1.txt
# Untracked files:
#   archivo2.txt
```

**Ejemplo 3: Restaurar desde versión anterior**
```bash
# Ver historial del archivo
git log  -- config.json

# Restaurar archivo desde 2 commits atrás
git restore --source=HEAD~2 config.json

# El archivo ahora tiene el contenido de hace 2 commits
```

**Ejemplo 4: Operaciones combinadas**
```bash
# Situación: archivo modificado y en staging
echo "Cambio 1" > archivo.txt
git add archivo.txt
echo "Cambio 2" >> archivo.txt

git status
# Changes to be committed:
#   modified: archivo.txt
# Changes not staged:
#   modified: archivo.txt

# Restaurar solo working directory
git restore archivo.txt
# Ahora solo queda el cambio en staging

# Restaurar también staging
git restore --staged archivo.txt
# Ahora todo está como en el último commit
```

### Ventajas de git restore

1. **Más claro**: Nombres de opciones más descriptivos
2. **Más seguro**: Separación clara entre staging y working directory
3. **Menos confuso**: No mezcla funcionalidades como `git checkout`
4. **Mejor UX**: Mensajes de error más claros

### Comparación: checkout vs restore

| Acción | git checkout (antiguo) | git restore (moderno) |
|--------|----------------------|---------------------|
| Deshacer cambios locales | `git checkout archivo.txt` | `git restore archivo.txt` |
| Unstage archivo | `git reset archivo.txt` | `git restore --staged archivo.txt` |
| Restaurar desde commit | `git checkout HEAD~1 -- archivo.txt` | `git restore --source=HEAD~1 archivo.txt` |

---

## 3. git revert

### ¿Qué hace?
`git revert` crea un nuevo commit que deshace los cambios de un commit anterior. Es la forma "segura" de deshacer cambios porque no reescribe el historial.

### ¿Por qué usar revert?

1. **Historial preservado**: No elimina commits existentes
2. **Seguro para repositorios compartidos**: No afecta a otros desarrolladores
3. **Auditable**: Queda registro de qué se deshizo y por qué
4. **Reversible**: Puedes revertir un revert

### Sintaxis básica
```bash
git revert <commit>
git revert <commit-range>
git revert --no-commit <commit>
```

### Opciones principales

#### Revert básico
```bash
# Revertir commit específico
git revert abc1234

# Revertir último commit
git revert HEAD

# Revertir commit anterior
git revert HEAD~1
```

#### Revert sin commit automático
```bash
# Aplicar cambios pero no hacer commit
git revert --no-commit abc1234

# Puedes revisar y modificar antes de commit
git status
git commit -m "Revert: deshacer cambios problemáticos"
```

#### Revert de merge commits
```bash
# Revertir merge commit (especificar parent)
git revert -m 1 merge-commit-hash

# -m 1 indica revertir a primer parent (main)
# -m 2 indica revertir a segundo parent (feature branch)
```

#### Revert de rango de commits
```bash
# Revertir múltiples commits
git revert HEAD~3..HEAD

# Revertir rango específico
git revert abc1234..def5678
```

### Ejemplos prácticos

**Ejemplo 1: Revert simple**
```bash
# Ver historial
git log  -5
# abc1234 Cambio problemático
# def5678 Cambio bueno
# ghi9012 Otro cambio bueno

# Revertir el cambio problemático
git revert abc1234

# Git abrirá editor con mensaje como:
# "Revert "Cambio problemático""
# 
# This reverts commit abc1234.

# Verificar historial después
git log  -3
# xyz7890 Revert "Cambio problemático"
# abc1234 Cambio problemático
# def5678 Cambio bueno
```

**Ejemplo 2: Revert de merge**
```bash
# Situación: merge que causó problemas
git log  --graph -5
# abc1234 Merge branch 'problematica'
# def5678 | feat: funcionalidad problemática
# ghi9012 | feat: otra funcionalidad
# jkl3456 |/
# mno7890 Último commit estable

# Revertir el merge (volver a main)
git revert -m 1 abc1234

# Mensaje sugerido:
# "Revert "Merge branch 'problematica'"
# 
# This reverts commit abc1234, reversing
# changes made to jkl3456."
```

**Ejemplo 3: Revert múltiple sin commits automáticos**
```bash
# Revertir varios commits pero en un solo commit final
git revert --no-commit HEAD~2
git revert --no-commit HEAD~1

# Revisar qué cambios se aplicarán
git status
git diff --staged

# Hacer commit manual con mensaje personalizado
git commit -m "Revert: eliminar funcionalidades inestables

- Revertir implementación de API v2
- Revertir cambios en base de datos
- Mantener funcionalidad básica estable"
```

**Ejemplo 4: Revert de revert (undo del undo)**
```bash
# Situación: revertiste algo pero luego necesitas restaurarlo
git log  -3
# abc1234 Revert "Implementar nueva funcionalidad"
# def5678 Implementar nueva funcionalidad
# ghi9012 Commit anterior

# Revertir el revert (restaurar funcionalidad)
git revert abc1234

# Mensaje sugerido:
# "Revert "Revert "Implementar nueva funcionalidad""
# 
# This reverts commit abc1234."
```

### Cuándo usar git revert

✅ **Usar revert cuando:**
- El commit ya fue pushed a repositorio compartido
- Quieres mantener historial completo
- Necesitas documentar por qué se deshizo algo
- Trabajas en equipo y otros pueden haber basado trabajo en ese commit

❌ **No usar revert cuando:**
- Es trabajo local que nadie más ha visto
- Prefieres historial limpio sin "ruido"
- Los commits son muy recientes y no críticos

---

## 4. git stash

### ¿Qué hace?
`git stash` guarda temporalmente cambios del working directory y staging area en una "pila" especial, permitiendo cambiar de rama o hacer otras operaciones con un directorio limpio.

### ¿Por qué usar stash?

1. **Cambio de contexto rápido**: Cambiar de rama sin hacer commit
2. **Trabajo temporal**: Guardar cambios que no están listos para commit
3. **Experimentos**: Probar algo rápidamente y volver al estado anterior
4. **Pulls limpios**: Hacer pull sin conflictos con cambios locales

### Sintaxis básica
```bash
git stash                    # Guardar cambios
git stash pop               # Aplicar y eliminar último stash
git stash apply             # Aplicar sin eliminar stash
git stash list              # Ver lista de stashes
git stash drop              # Eliminar stash específico
```

### Comandos principales

#### Guardar cambios (stash)
```bash
# Stash básico
git stash

# Stash con mensaje descriptivo
git stash push -m "WIP: trabajando en nueva funcionalidad"

# Stash incluyendo archivos untracked
git stash -u
git stash --include-untracked

# Stash incluyendo archivos ignored
git stash -a
git stash --all

# Stash solo archivos específicos
git stash push -m "mensaje" archivo1.txt archivo2.txt
```

#### Recuperar cambios
```bash
# Aplicar último stash y eliminarlo
git stash pop

# Aplicar stash específico
git stash pop stash@{2}

# Aplicar sin eliminar (para usar en múltiples ramas)
git stash apply
git stash apply stash@{1}
```

#### Gestionar stashes
```bash
# Ver lista de stashes
git stash list

# Ver contenido de stash específico
git stash show stash@{0}
git stash show -p stash@{0}  # Con diff completo

# Eliminar stash específico
git stash drop stash@{1}

# Eliminar todos los stashes
git stash clear
```

#### Crear rama desde stash
```bash
# Crear rama y aplicar stash en un comando
git stash branch nueva-rama stash@{0}
```

### Ejemplos prácticos

**Ejemplo 1: Cambio rápido de rama**
```bash
# Trabajando en feature branch
git switch nueva-funcionalidad
echo "Trabajo en progreso" > trabajo.txt
git add trabajo.txt

# Necesitas cambiar a main urgentemente
git status
# Changes to be committed:
#   new file: trabajo.txt

# Guardar trabajo temporalmente
git stash push -m "WIP: función de login a medias"

# Ahora puedes cambiar de rama
git switch main
# ... hacer trabajo urgente en main

# Volver a tu trabajo
git switch nueva-funcionalidad
git stash pop

# Continuar donde dejaste
git status
# Changes to be committed:
#   new file: trabajo.txt
```

**Ejemplo 2: Experimento temporal**
```bash
# Tienes código funcionando
git status
# nothing to commit, working tree clean

# Hacer experimento
echo "Código experimental" > experimento.py
git add experimento.py
echo "Modificación experimental" >> archivo-existente.txt

# Guardar experimento
git stash push -m "Experimento con nueva librería"

# Código vuelve al estado original
ls experimento.py  # No existe
git diff archivo-existente.txt  # Sin cambios

# Si el experimento funcionó, recuperarlo
git stash pop

# Si no funcionó, descartarlo
git stash drop
```

**Ejemplo 3: Gestión múltiple de stashes**
```bash
# Crear varios stashes
echo "Cambio 1" > archivo1.txt
git stash push -m "Trabajo en funcionalidad A"

echo "Cambio 2" > archivo2.txt
git stash push -m "Trabajo en funcionalidad B"

echo "Cambio 3" > archivo3.txt
git stash push -m "Experimento fallido"

# Ver lista de stashes
git stash list
# stash@{0}: On main: Experimento fallido
# stash@{1}: On main: Trabajo en funcionalidad B
# stash@{2}: On main: Trabajo en funcionalidad A

# Aplicar stash específico (funcionalidad A)
git stash apply stash@{2}

# Eliminar experimento fallido
git stash drop stash@{0}

# Ver qué queda
git stash list
# stash@{0}: On main: Trabajo en funcionalidad B
# stash@{1}: On main: Trabajo en funcionalidad A
```

**Ejemplo 4: Stash parcial**
```bash
# Múltiples archivos modificados
echo "Cambio importante" > importante.txt
echo "Cambio temporal" > temporal.txt
git add importante.txt temporal.txt

# Solo stash el archivo temporal
git stash push -m "Solo cambios temporales" temporal.txt

# importante.txt sigue en staging
git status
# Changes to be committed:
#   modified: importante.txt

# temporal.txt está guardado en stash
git stash show -p
```

### Casos de uso avanzados

#### Stash con conflictos
```bash
# Si aplicar stash genera conflictos
git stash pop
# Auto-merging archivo.txt
# CONFLICT (content): Merge conflict in archivo.txt

# Resolver conflictos como en merge normal
# Editar archivo.txt
git add archivo.txt

# El stash se elimina automáticamente después de resolver
```

#### Aplicar stash en otra rama
```bash
# Crear stash en una rama
git switch A
echo "Trabajo útil" > util.txt
git stash push -m "Trabajo que sirve para otra rama"

# Aplicar en otra rama
git switch B
git stash apply  # Mantiene el stash para usar en más ramas

# O crear nueva rama directamente
git stash branch C
```

### Cuándo usar git stash

✅ **Usar stash cuando:**
- Necesitas cambiar de rama rápidamente
- Tienes trabajo a medias que no está listo para commit
- Quieres probar algo temporalmente
- Necesitas hacer pull pero tienes cambios locales

❌ **No usar stash para:**
- Almacenamiento permanente (usa branches)
- Backup de código (usa commits)
- Compartir código con otros (usa branches)

---

## Resumen comparativo de comandos

### Tabla de decisión rápida

| Situación | Comando recomendado | Resultado |
|-----------|-------------------|-----------|
| Deshacer último commit (local) | `git reset --soft HEAD~1` | Cambios en staging |
| Eliminar cambios locales | `git restore archivo.txt` | Archivo restaurado |
| Deshacer commit público | `git revert commit-hash` | Nuevo commit que deshace |
| Cambiar rama con trabajo a medias | `git stash` | Cambios guardados temporalmente |
| Quitar archivo del staging | `git restore --staged archivo.txt` | Archivo unstaged |
| Eliminar completamente cambios | `git reset --hard HEAD~1` | ⚠️ Cambios perdidos |
| Guardar experimento temporal | `git stash push -m "experimento"` | Cambios guardados con nombre |

### Flujo de trabajo común

```bash
# 1. Trabajando en funcionalidad
git switch nueva
# ... editando archivos

# 2. Necesitas cambiar de contexto urgentemente
git stash push -m "WIP: nueva funcionalidad"

# 3. Atender urgencia
git switch main
git pull
# ... trabajo urgente
git add .
git commit -m "hotfix: corregir bug crítico"

# 4. Volver a tu trabajo
git switch nueva
git stash pop

# 5. Continuar desarrollo
# ... más cambios
git add .
git commit -m "feat: completar nueva funcionalidad"
```

### Comandos más utilizados (recomendaciones)

#### Uso diario (muy frecuente)
1. `git stash` / `git stash pop` - Para cambios de contexto
2. `git restore archivo.txt` - Para deshacer cambios locales
3. `git restore --staged archivo.txt` - Para unstage
4. `git reset HEAD~1` - Para deshacer último commit (local)

#### Uso semanal (frecuente)
1. `git revert commit-hash` - Para deshacer commits públicos
2. `git stash list` / `git stash apply` - Gestión de stashes
3. `git reset --hard HEAD~1` - Eliminar cambios (con cuidado)

#### Uso ocasional (menos frecuente)
1. `git reset --soft HEAD~3` - Combinar múltiples commits
2. `git revert -m 1 merge-hash` - Revertir merges
3. `git stash branch nueva-rama` - Crear rama desde stash

### Comandos de emergencia

```bash
# Si algo sale mal, estos comandos pueden salvarte

# Ver qué hiciste recientemente
git reflog

# Recuperar commit "perdido"
git branch recuperar-commit hash-del-reflog

# Ver diferencias antes de aplicar cambios destructivos
git diff HEAD~1

# Abortar operación en curso
git reset --abort    # Si estás en medio de reset
git merge --abort    # Si estás en medio de merge
git revert --abort   # Si estás en medio de revert

# Limpiar completamente directorio de trabajo
git clean -fd        # Elimina archivos no rastreados
git reset --hard HEAD # Elimina cambios rastreados
```

En este nivel hemos cubierto las herramientas esenciales para manejar errores y deshacer cambios en Git. La clave está en elegir la herramienta correcta para cada situación y entender las consecuencias de cada comando.
