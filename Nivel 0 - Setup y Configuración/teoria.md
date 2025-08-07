# Nivel 0: Setup y Configuración - Preparando el Entorno

## Introducción

Antes de aprender Git, necesitamos tener todo el entorno correctamente configurado. Este nivel te guiará paso a paso para instalar Git, crear tu cuenta de GitHub, configurar tu entorno local y realizar tu primera conexión entre tu computadora y GitHub.

## ¿Qué vamos a lograr?

Al final de este nivel tendrás:
- ✅ Git instalado en tu sistema operativo
- ✅ Cuenta de GitHub creada y configurada
- ✅ Git configurado con tu identidad
- ✅ Conexión SSH entre tu computadora y GitHub
- ✅ Tu primer repositorio creado y sincronizado
- ✅ Editor de código configurado para trabajar con Git

## Prerrequisitos

- Computadora con Windows, macOS o Linux
- Conexión a Internet
- Dirección de email válida
- Editor de código (recomendamos VS Code)

---

## 1. Instalación de Git

### En Windows

#### Opción 1: Descarga desde el sitio oficial (Recomendada)

1. **Descargar Git**
   - Ve a https://git-scm.com/downloads
   - Haz clic en "Download for Windows"
   - Se descargará automáticamente la versión más reciente

2. **Instalar Git**
   - Ejecuta el archivo descargado (Git-2.x.x-64-bit.exe)
   - **Configuraciones recomendadas durante la instalación:**
     - ✅ **Select Components**: Mantén las opciones por defecto
     - ✅ **Default editor**: Selecciona "Use Visual Studio Code as Git's default editor" (si tienes VS Code)
     - ✅ **PATH environment**: Selecciona "Git from the command line and also from 3rd-party software"
     - ✅ **HTTPS transport backend**: Selecciona "Use the OpenSSL library"
     - ✅ **Line ending conversions**: Selecciona "Checkout Windows-style, commit Unix-style line endings"
     - ✅ **Terminal emulator**: Selecciona "Use Windows' default console window"
     - ✅ **Git Credential Manager**: Mantén habilitado

3. **Verificar instalación**
   - Abre Command Prompt (cmd) o PowerShell
   - Ejecuta: `git --version`
   - Deberías ver algo como: `git version 2.43.0.windows.1`

---

## 2. Crear cuenta de GitHub

### Paso 1: Registro

1. **Ir a GitHub**
   - Ve a https://github.com
   - Haz clic en "Sign up"

2. **Completar formulario de registro**
   - **Username**: Elige un nombre de usuario profesional (será tu identidad en GitHub)
     - ❌ Evita: `xXCodeMaster123Xx`, `juan_random_2024`
     - ✅ Mejor: `juan-perez`, `maria-dev`, `alejandro-martinez`
   - **Email**: Usa tu email principal (lo necesitarás para verificación)
   - **Password**: Crea una contraseña segura (mínimo 8 caracteres)

3. **Verificar cuenta**
   - Completa el puzzle de verificación
   - Revisa tu email y haz clic en el enlace de verificación

### Paso 2: Configurar perfil

1. **Información básica**
   - Agrega una foto de perfil profesional
   - Completa tu nombre real
   - Agrega una bio breve (opcional pero recomendado)
   - Agrega tu ubicación y sitio web si tienes

2. **Configurar visibilidad**
   - Ve a Settings → Profile
   - Decide qué información quieres que sea pública

---

## 3. Configuración inicial de Git

### Configurar identidad (OBLIGATORIO)

Antes de hacer cualquier commit, Git necesita saber quién eres:

```bash
# Configurar nombre (usa tu nombre real)
git config --global user.name "Tu Nombre Completo"

# Configurar email (usa el mismo email de GitHub)
git config --global user.email "tu.email@ejemplo.com"
```

**Ejemplo:**
```bash
git config --global user.name "María García"
git config --global user.email "maria.garcia@gmail.com"
```

### Configurar editor por defecto

```bash
# Para VS Code (recomendado)
git config --global core.editor "code --wait"

# Para Sublime Text
git config --global core.editor "subl -n -w"

# Para Vim
git config --global core.editor "vim"

# Para Nano (más fácil para principiantes)
git config --global core.editor "nano"
```

### Configurar rama por defecto

```bash
# Usar 'main' como rama por defecto (estándar moderno)
git config --global init.defaultBranch main
```

### Configuraciones adicionales recomendadas

```bash
# Colorear output de Git (hace más fácil leer)
git config --global color.ui auto

# Configurar comportamiento de pull
git config --global pull.rebase false

# Configurar push por defecto
git config --global push.default simple

# Configurar autocrlf (importante en Windows)
# En Windows:
git config --global core.autocrlf true

# En macOS/Linux:
git config --global core.autocrlf input
```

### Verificar configuración

```bash
# Ver toda la configuración
git config --list

# Ver configuraciones específicas
git config user.name
git config user.email
git config core.editor
```

## 4. Configurar editor de código

### Visual Studio Code (Recomendado)

#### Instalación

1. **Descargar**: Ve a https://code.visualstudio.com/
2. **Instalar**: Ejecuta el instalador
3. **Configurar**: Durante la instalación, marca:
   - ✅ "Add to PATH"
   - ✅ "Register Code as an editor for supported file types"

#### Extensiones recomendadas para Git

Instala estas extensiones en VS Code:

1. **GitLens** (eamodio.gitlens)
   - Supercharge Git capabilities
   - Muestra información de Git en línea

2. **Git Graph** (mhutchie.git-graph)
   - Visualización gráfica del historial de Git

3. **Git History** (donjayamanne.githistory)
   - Ver historial de archivos y commits


## 6. Primera prueba completa

### Paso 1: Crear tu primer repositorio

1. **En GitHub**
   - Ve a https://github.com
   - Haz clic en el botón "+" → "New repository"
   - **Repository name**: `mi-primer-repositorio`
   - **Description**: "Mi primer repositorio para aprender Git"
   - ✅ **Public** (para practicar)
   - ✅ **Add a README file**
   - Haz clic en "Create repository"

### Paso 2: Clonar repositorio localmente

```bash
# Navegar a donde quieres guardar tus proyectos
cd Documents
mkdir Proyectos-Git
cd Proyectos-Git

# Clonar repositorio (usa SSH si lo configuraste)
git clone git@github.com:tu-username/mi-primer-repositorio.git

# O usa HTTPS si prefieres
git clone https://github.com/tu-username/mi-primer-repositorio.git

# Entrar al directorio
cd mi-primer-repositorio
```

### Paso 3: Hacer tu primer cambio

```bash
# Crear un archivo nuevo
echo "# Mi primer proyecto con Git" > mi-archivo.txt

# Ver estado
git status

# Agregar archivo
git add mi-archivo.txt

# Hacer commit
git commit -m "feat: agregar mi primer archivo"

# Subir cambios a GitHub
git push origin main
```

### Paso 4: Verificar en GitHub

1. Ve a tu repositorio en GitHub
2. Deberías ver tu nuevo archivo `mi-archivo.txt`
3. Deberías ver tu commit en el historial

---

## 7. Configuración del directorio de trabajo

### Crear estructura de trabajo

```bash
# Crear directorio principal para proyectos Git
mkdir -p ~/Documentos/Proyectos-Git
cd ~/Documentos/Proyectos-Git

# Crear subdirectorios organizados
mkdir aprendizaje
mkdir proyectos-personales
mkdir colaboraciones

# Estructura recomendada:
# ~/Documentos/Proyectos-Git/
# ├── aprendizaje/          # Para ejercicios y práctica
# ├── proyectos-personales/ # Para tus proyectos
# └── colaboraciones/       # Para proyectos de equipo
```

### Configurar .gitignore global

Crear archivo de ignorar global para archivos que nunca quieres en Git:

```bash
# Crear archivo gitignore global
touch ~/.gitignore_global

# Configurar Git para usarlo
git config --global core.excludesfile ~/.gitignore_global
```

**Contenido recomendado para .gitignore_global:**
```gitignore
# Archivos del sistema operativo
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Archivos de editores
*.swp
*.swo
*~
.vscode/
.idea/

# Archivos temporales
*.tmp
*.temp
*.log

# Archivos de configuración local
.env
.env.local
.env.*.local

# Node.js
node_modules/
npm-debug.log*

# Python
__pycache__/
*.py[cod]
*$py.class
.venv/
venv/

# Archivos de backup
*.backup
*.bak
*.orig
```

---

## 8. Verificación final del setup

### Checklist de verificación

Ejecuta estos comandos para verificar que todo está funcionando:

```bash
# 1. Verificar versión de Git
git --version

# 2. Verificar configuración de usuario
git config user.name
git config user.email

# 3. Verificar conexión con GitHub (SSH)
ssh -T git@github.com

# 4. Verificar editor configurado
git config core.editor

# 5. Verificar rama por defecto
git config init.defaultBranch

# 6. Ver toda la configuración
git config --list --global
```

### Esperado resultado

✅ **Git instalado**: Versión 2.40+ mostrada
✅ **Usuario configurado**: Tu nombre y email correctos
✅ **GitHub conectado**: Mensaje de autenticación exitosa
✅ **Editor configurado**: Tu editor preferido
✅ **Rama por defecto**: "main"

### Solución de problemas comunes

#### Problema: "git: command not found"
```bash
# Verificar que Git esté en PATH
echo $PATH

# En Windows, reiniciar terminal o computadora
# En macOS/Linux, verificar instalación:
which git
```

#### Problema: SSH no funciona
```bash
# Verificar que ssh-agent esté corriendo
ssh-add -l

# Verificar permisos de archivos SSH
ls -la ~/.ssh/
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

#### Problema: Push requiere username/password constantemente
```bash
# Verificar remote URL
git remote -v

# Cambiar a SSH si está usando HTTPS
git remote set-url origin git@github.com:username/repo.git
```

---

## 9. Recursos adicionales

### Documentación oficial
- **Git**: https://git-scm.com/doc
- **GitHub**: https://docs.github.com/

### Herramientas útiles
- **GitHub Desktop**: Interfaz gráfica para principiantes
- **SourceTree**: Cliente Git visual avanzado
- **GitKraken**: Cliente Git con interfaz moderna

### Configuraciones avanzadas (opcional)

#### Aliases útiles
```bash
# Shortcuts para comandos comunes
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

#### Configurar diff y merge tools
```bash
# Para VS Code
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

---

## Resumen

¡Felicidades! 🎉 Has completado la configuración básica de Git y GitHub. Ahora tienes:

- ✅ Git instalado y configurado
- ✅ Cuenta de GitHub creada
- ✅ Conexión SSH o token configurado
- ✅ Editor de código preparado
- ✅ Primer repositorio creado y sincronizado
- ✅ Entorno de trabajo organizado

### Próximos pasos

Ahora estás listo para comenzar con **Nivel 1: Comandos Básicos** donde aprenderás:
- `git init` - Inicializar repositorios
- `git add` - Preparar cambios
- `git commit` - Confirmar cambios
- `git log` - Ver historial

### Consejos para el éxito

1. **Practica regularmente**: Usa Git en todos tus proyectos
2. **No tengas miedo de experimentar**: Git es muy difícil de romper permanentemente
3. **Lee los mensajes de error**: Git da información útil sobre qué está pasando
4. **Haz commits frecuentes**: Es mejor hacer muchos commits pequeños que pocos grandes
5. **Usa mensajes descriptivos**: Tu yo del futuro te lo agradecerá

¡Estás listo para comenzar tu viaje con Git! 🚀
