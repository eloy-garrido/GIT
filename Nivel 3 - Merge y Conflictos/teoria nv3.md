# Nivel 3: Merge y Conflictos - Uniendo el Trabajo

## Introducción

En este tercer nivel aprenderemos uno de los aspectos más importantes del trabajo colaborativo con Git: cómo combinar el trabajo de diferentes ramas usando merge, cómo manejar conflictos cuando Git no puede fusionar automáticamente los cambios, y qué es el estado DETACHED HEAD y cómo manejarlo.

## Prerrequisitos

Antes de comenzar este nivel, debes dominar:
- Comandos básicos: `git init`, `git add`, `git commit`, `git log`
- Manejo de ramas: `git branch`, `git checkout`, `git switch`
- Navegación entre ramas y creación de contenido en diferentes ramas

## Conceptos a Cubrir

- **git merge** - Fusionar ramas
- **Conflictos de merge** - Resolución manual cuando Git no puede fusionar automáticamente
- **DETACHED HEAD** - Estado especial de Git y cómo manejarlo
- **Estrategias de merge** - Fast-forward, recursive, y otras estrategias

---

## ¿Qué es un merge?

Un **merge** (fusión) es el proceso de combinar cambios de una rama con otra. Típicamente, se fusionan ramas de funcionalidades (feature branches) de vuelta a la rama principal (main o develop).

### Visualización conceptual

**Antes del merge:**
```
main:     A---B---C
               \
feature:        D---E
```

**Después del merge:**
```
main:     A---B---C---F
               \     /
feature:        D---E
```

### ¿Por qué hacer merge?

1. **Integrar funcionalidades**: Combinar trabajo de diferentes desarrolladores
2. **Mantener historial**: Preservar el contexto de desarrollo
3. **Sincronización**: Actualizar ramas con cambios recientes
4. **Colaboración**: Permitir que el equipo trabaje en paralelo

---

## 1. git merge

### ¿Qué hace?
`git merge` combina los cambios de una rama especificada con la rama actual. Git intentará fusionar automáticamente los cambios, pero si hay conflictos, requerirá intervención manual.

### Sintaxis básica
```bash
git merge <rama-a-fusionar>
git merge <rama> --no-ff
git merge <rama> --squash
```

### Tipos de merge

#### 1. Fast-Forward Merge
Ocurre cuando la rama de destino no tiene commits nuevos desde que se creó la rama que se va a fusionar.

```
Antes:
main:     A---B
               \
feature:        C---D

Después (fast-forward):
main:     A---B---C---D
```

#### 2. Three-Way Merge (Recursive)
Ocurre cuando ambas ramas tienen commits nuevos. Git crea un commit de merge.

```
Antes:
main:     A---B---C
               \
feature:        D---E

Después (three-way):
main:     A---B---C---F
               \     /
feature:        D---E
```

### Opciones de merge

#### Merge básico
```bash
# Fusionar rama feature con la rama actual
git merge nueva-funcionalidad
```

#### Merge sin fast-forward
```bash
# Forzar creación de commit de merge
git merge --no-ff nueva-funcionalidad
```

#### Squash merge
```bash
# Combinar todos los commits de la rama en uno solo
git merge --squash nueva-funcionalidad
```

#### Merge con mensaje personalizado
```bash
# Especificar mensaje del commit de merge
git merge -m "Merge feature: agregar login system" login
```

### Ejemplos prácticos

**Ejemplo 1: Fast-Forward Merge**
```bash
# Crear y trabajar en rama feature
git switch -c readme-update
echo "# Proyecto Actualizado" > README.md
git add README.md
git commit -m "docs: actualizar README"

# Volver a main y hacer merge
git switch main
git merge readme-update

# Ver historial (será linear)
git log  --graph
```

**Ejemplo 2: Three-Way Merge**
```bash
# En main, hacer un cambio
git switch main
echo "Cambio en main" >> archivo.txt
git add archivo.txt
git commit -m "feat: cambio en main"

# En feature, hacer otro cambio
git switch nueva-funcionalidad
echo "Cambio en feature" >> otro-archivo.txt
git add otro-archivo.txt
git commit -m "feat: cambio en feature"

# Volver a main y merge
git switch main
git merge nueva-funcionalidad

# Ver historial (con rama visible)
git log  --graph --all
```

### Recomendaciones para merge
- Siempre estar en la rama de destino antes de hacer merge
- Verificar que ambas ramas estén actualizadas
- Usar `--no-ff` para preservar historial de ramas en proyectos importantes
- Probar la aplicación después del merge
- Eliminar ramas feature después de merge exitoso

---

## 2. Conflictos de Merge

### ¿Qué es un conflicto?

Un **conflicto de merge** ocurre cuando Git no puede fusionar automáticamente los cambios porque:
1. Ambas ramas modificaron las mismas líneas de un archivo
2. Una rama eliminó un archivo que la otra modificó
3. Ambas ramas crearon archivos con el mismo nombre pero contenido diferente

### Identificar conflictos

Cuando hay conflictos, Git te mostrará:
```bash
git merge conflictiva
Auto-merging archivo.txt
CONFLICT (content): Merge conflict in archivo.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### Anatomía de un conflicto

Git marca los conflictos en el archivo de esta manera:
```
<<<<<<< HEAD
Contenido de la rama actual (main)
=======
Contenido de la rama que se está fusionando (feature)
>>>>>>> nombre-rama
```

**Ejemplo de conflicto real:**
```python
def saludar():
<<<<<<< HEAD
    print("¡Hola desde main!")
    return "Saludo oficial"
=======
    print("¡Hola desde feature!")
    return "Saludo personalizado"
>>>>>>> saludo-personalizado
```

### Proceso de resolución de conflictos

#### Paso 1: Identificar archivos en conflicto
```bash
git status
# On branch main
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#
#	both modified:   archivo.txt
```

#### Paso 2: Abrir y editar archivos
```bash
# Abrir archivo en conflicto
nano archivo.txt
# o usar tu editor preferido
code archivo.txt
```

#### Paso 3: Resolver el conflicto
Tienes tres opciones:
1. **Mantener cambios de HEAD** (rama actual)
2. **Mantener cambios de la rama que se fusiona**
3. **Combinar ambos cambios** (solución personalizada)

**Ejemplo de resolución:**
```python
# Antes (conflicto):
def saludar():
<<<<<<< HEAD
    print("¡Hola desde main!")
    return "Saludo oficial"
=======
    print("¡Hola desde feature!")
    return "Saludo personalizado"
>>>>>>> saludo-personalizado

# Después (resuelto):
def saludar():
    print("¡Hola desde la aplicación!")
    return "Saludo mejorado"
```

#### Paso 4: Marcar como resuelto y completar merge
```bash
# Agregar archivo resuelto
git add archivo.txt

# Verificar estado
git status

# Completar el merge
git commit
# Git abrirá editor con mensaje de merge predeterminado
```

### Herramientas para resolución de conflictos

#### Usando git mergetool
```bash
# Configurar herramienta de merge (una vez)
git config --global merge.tool vimdiff
# o usar otras: meld, kdiff3, vscode

# Usar herramienta para resolver conflictos
git mergetool
```

#### Usando VS Code
VS Code detecta automáticamente conflictos y muestra opciones:
- "Accept Current Change"
- "Accept Incoming Change"
- "Accept Both Changes"
- "Compare Changes"

#### Comandos útiles durante conflictos
```bash
# Ver archivos en conflicto
git diff --name-only --diff-filter=U

# Ver conflictos de un archivo específico
git diff archivo.txt

# Abortar merge si no puedes resolverlo
git merge --abort

# Ver estado del merge
git status
```

### Ejemplos prácticos de conflictos

**Ejemplo 1: Conflicto simple**
```bash
# En main
git switch main
echo "Versión main" > config.txt
git add config.txt
git commit -m "feat: configuración desde main"

# En feature
git switch -c config
echo "Versión feature" > config.txt
git add config.txt
git commit -m "feat: configuración desde feature"

# Intentar merge (generará conflicto)
git switch main
git merge config

# Resolver conflicto editando config.txt
echo "Versión combinada" > config.txt
git add config.txt
git commit
```

**Ejemplo 2: Conflicto complejo en código**
```bash
# Simular conflicto en código Python
# En main: agregar función
echo 'def calcular(a, b):
    return a + b

def main():
    resultado = calcular(5, 3)
    print(f"Resultado: {resultado}")' > calculadora.py

git add calculadora.py
git commit -m "feat: agregar función calcular"

# En feature: modificar la misma función
git switch -c multiplicacion
echo 'def calcular(a, b):
    return a * b

def main():
    resultado = calcular(5, 3)
    print(f"Producto: {resultado}")' > calculadora.py

git add calculadora.py
git commit -m "feat: cambiar a multiplicación"

# Merge con conflicto
git switch main
git merge multiplicacion

# Resolver creando ambas funciones
echo 'def sumar(a, b):
    return a + b

def multiplicar(a, b):
    return a * b

def main():
    suma = sumar(5, 3)
    producto = multiplicar(5, 3)
    print(f"Suma: {suma}, Producto: {producto}")' > calculadora.py

git add calculadora.py
git commit
```

### Prevenir conflictos

1. **Comunicación del equipo**: Coordinar quién trabaja en qué archivos
2. **Pulls frecuentes**: Mantener ramas actualizadas
3. **Ramas pequeñas**: Hacer merges frecuentes de ramas pequeñas
4. **Separación de responsabilidades**: Diferentes desarrolladores en diferentes módulos

---

## 3. DETACHED HEAD

### ¿Qué es DETACHED HEAD?

**DETACHED HEAD** es un estado en Git donde HEAD (el puntero que indica dónde estás) apunta directamente a un commit específico en lugar de apuntar a una rama.

### ¿Cuándo ocurre?

1. **Checkout a commit específico**:
   ```bash
   git checkout abc1234
   ```

2. **Checkout a tag**:
   ```bash
   git checkout v1.0.0
   ```

3. **Durante rebase interactivo** (veremos en niveles avanzados)

4. **Al explorar historial**:
   ```bash
   git checkout HEAD~3  # 3 commits atrás
   ```

### Visualización del estado

**Estado normal:**
```
HEAD -> main -> commit_abc
```

**Estado DETACHED HEAD:**
```
HEAD -> commit_abc
main -> commit_xyz
```

### Identificar DETACHED HEAD

Git te avisa cuando entras en este estado:
```bash
git checkout abc1234
# Note: switching to 'abc1234'.
# 
# You are in 'detached HEAD' state. You can look around, make experimental
# changes and commit them, and you can discard any commits you make in this
# state without impacting any branches by switching back to a branch.
```

También puedes verificar:
```bash
git branch
# * (HEAD detached at abc1234)
#   main
#   algo

git status
# HEAD detached at abc1234
```

### ¿Por qué es problemático?

1. **Commits "huérfanos"**: Los commits hechos en DETACHED HEAD pueden perderse
2. **Sin referencia**: No hay una rama que apunte a tus cambios
3. **Garbage collection**: Git puede eliminar commits sin referencia

### Trabajar en DETACHED HEAD

#### Explorar (seguro)
```bash
# Ver código en commit específico
git checkout abc1234
ls
cat archivo.txt
git log  -5

# Volver a rama
git switch main
```

#### Hacer cambios (riesgoso)
```bash
# En DETACHED HEAD
git checkout abc1234
echo "Cambio experimental" > experimento.txt
git add experimento.txt
git commit -m "Experimento en detached HEAD"

# El commit existe pero sin rama que lo referencie
git log  -1
# def5678 Experimento en detached HEAD

# Si cambias de rama, el commit se "pierde"
git switch main
git log  -5  # No verás def5678
```

### Resolver DETACHED HEAD

#### Opción 1: Crear rama desde el estado actual
```bash
# Estando en DETACHED HEAD con cambios
git switch -c nueva-rama-experimental

# Ahora tus cambios están en una rama
git branch
# * nueva-rama-experimental
#   main
```

#### Opción 2: Volver a rama sin guardar cambios
```bash
# Descartar todo el trabajo en DETACHED HEAD
git switch main
# Warning: you are leaving 1 commit behind
```

#### Opción 3: Guardar cambios específicos
```bash
# En DETACHED HEAD, encontrar hash del commit
git log  -1
# def5678 Mi trabajo importante

# Volver a rama
git switch main

# Crear rama desde ese commit
git branch recuperar-trabajo def5678

# Cambiar a la rama creada
git switch recuperar-trabajo
```

### Ejemplos prácticos

**Ejemplo 1: Exploración segura**
```bash
# Ver historial
git log 
# abc1234 Último commit
# def5678 Commit anterior
# ghi9012 Commit más viejo

# Explorar commit anterior
git checkout def5678
# HEAD detached at def5678

# Ver archivos en ese punto
ls
cat README.md

# Volver a main
git switch main
```

**Ejemplo 2: Recuperar trabajo perdido**
```bash
# Accidentally trabajar en DETACHED HEAD
git checkout HEAD~2
echo "Trabajo importante" > importante.txt
git add importante.txt
git commit -m "Trabajo que no quiero perder"

# Guardar hash antes de cambiar
git log  -1
# xyz9876 Trabajo que no quiero perder

# Volver a main
git switch main

# Crear rama desde el trabajo perdido
git branch rescatar-trabajo xyz9876
git switch rescatar-trabajo

# Verificar que el trabajo está ahí
ls importante.txt
```

### Comandos útiles para DETACHED HEAD

```bash
# Ver dónde estás
git branch
git status

# Ver commits recientes
git log  -5

# Ver todos los commits (incluyendo "perdidos")
git reflog

# Crear rama desde estado actual
git switch -c nombre-nueva-rama

# Volver a rama segura
git switch main
```

### Recomendaciones

1. **Evita hacer commits en DETACHED HEAD** a menos que sepas lo que haces
2. **Usa DETACHED HEAD para exploración** de versiones anteriores
3. **Siempre crea una rama** si necesitas hacer cambios desde un commit específico
4. **Usa git reflog** para recuperar commits "perdidos"
5. **Entiende las advertencias** que Git te da

---

## Estrategias de merge avanzadas

### 1. Merge vs Rebase

#### Merge (lo que hemos visto)
- Preserva historial completo
- Crea commits de merge
- Historial no linear pero completo

#### Rebase (tema avanzado)
- Historial linear
- No crea commits de merge
- Reescribe historial

### 2. Squash Merge

Combina todos los commits de una rama en un solo commit:
```bash
git merge --squash muchos-commits
git commit -m "feat: implementar funcionalidad completa"
```

**Cuándo usar:**
- Feature branch con muchos commits pequeños
- Quieres historial limpio en main
- Commits de la feature branch no son importantes individualmente

### 3. Merge sin Fast-Forward

Fuerza creación de commit de merge incluso cuando es posible fast-forward:
```bash
git merge --no-ff pequeña-funcionalidad
```

**Cuándo usar:**
- Quieres preservar que existió una rama
- Facilitar rollback de funcionalidades completas
- Política del proyecto requiere commits de merge

### 4. Abortar Merge

Si algo sale mal durante el merge:
```bash
# Abortar merge en progreso
git merge --abort

# Volver al estado anterior al merge
git reset --hard HEAD

# Ver estado actual
git status
```

---

## Flujo de trabajo completo con merge

### Flujo típico de desarrollo

1. **Partir desde main actualizada**
   ```bash
   git switch main
   git pull  # Si trabajas con repositorio remoto
   ```

2. **Crear rama de funcionalidad**
   ```bash
   git switch -c nueva-funcionalidad
   ```

3. **Desarrollar funcionalidad**
   ```bash
   # Hacer cambios
   git add .
   git commit -m "feat: implementar parte 1"
   
   # Más cambios
   git add .
   git commit -m "feat: implementar parte 2"
   ```

4. **Actualizar con main (opcional pero recomendado)**
   ```bash
   git switch main
   git pull
   git switch nueva-funcionalidad
   git merge main  # o git rebase main
   ```

5. **Merge a main**
   ```bash
   git switch main
   git merge nueva-funcionalidad
   ```

6. **Limpiar**
   ```bash
   git branch -d nueva-funcionalidad
   ```

### Checklist antes de hacer merge

- [ ] Los tests pasan
- [ ] El código está revisado
- [ ] La rama está actualizada con main
- [ ] No hay conflictos
- [ ] La funcionalidad está completa
- [ ] La documentación está actualizada

---

## Herramientas y comandos útiles

### Comandos de diagnóstico
```bash
# Ver estado de merge
git status

# Ver diferencias antes de merge
git diff main..rama

# Ver commits únicos de la rama
git log main..rama 

# Ver gráfico de ramas
git log --graph  --all

# Ver qué ramas están merged
git branch --merged
git branch --no-merged
```

### Configuraciones útiles
```bash
# Configurar editor para mensajes de merge
git config --global core.editor "code --wait"

# Configurar herramienta de merge
git config --global merge.tool vscode

# Evitar fast-forward por defecto
git config --global merge.ff false
```

### Aliases útiles
```bash
# Crear aliases para comandos comunes
git config --global alias.lg "log --graph  --all"
git config --global alias.st "status"
git config --global alias.sw "switch"
git config --global alias.mg "merge --no-ff"
```

---

## Problemas comunes y soluciones

### Problema 1: "I'm in the middle of a merge"
```bash
# Error: no puedes cambiar de rama durante merge
git status  # Ver qué archivos están en conflicto
git add .   # Resolver conflictos y agregar
git commit  # Completar merge
# o
git merge --abort  # Cancelar merge
```

### Problema 2: Commits perdidos en DETACHED HEAD
```bash
# Ver historial completo (incluyendo commits "perdidos")
git reflog

# Encontrar el commit perdido
git reflog | grep "commit message"

# Crear rama desde commit perdido
git branch recuperar abc1234
```

### Problema 3: Merge con demasiados conflictos
```bash
# Abortar y probar estrategia diferente
git merge --abort

# Opción 1: Actualizar feature branch primero
git switch problematica
git merge main  # Resolver conflictos aquí
git switch main
git merge problematica  # Ahora debería ser limpio

# Opción 2: Usar squash merge
git merge --squash problematica
```

---

## Resumen

En este nivel hemos aprendido sobre merge y manejo de conflictos:

### Conceptos clave
- **git merge**: Fusiona cambios de una rama a otra
- **Conflictos**: Cuando Git no puede fusionar automáticamente
- **DETACHED HEAD**: Estado donde HEAD apunta a commit específico
- **Tipos de merge**: Fast-forward, three-way, squash

### Flujo de resolución de conflictos
1. Identificar archivos en conflicto con `git status`
2. Editar archivos para resolver conflictos manualmente
3. Marcar como resuelto con `git add`
4. Completar merge con `git commit`

### Mejores prácticas
- Hacer merges pequeños y frecuentes
- Comunicar cambios importantes al equipo
- Probar código después de cada merge
- Usar herramientas visuales para conflictos complejos
- Mantener historial limpio con estrategias apropiadas

En el siguiente nivel aprenderemos sobre herramientas para revertir cambios y deshacer trabajo.
