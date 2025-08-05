### Actividad 4: Flujo de Trabajo con `rebase`

#### Ejercicio Guiado - Refactorización del Historial y Fusión Limpia

#### 🎯 Objetivo
El objetivo de esta actividad es que practiques cómo usar `git rebase` para integrar cambios de una rama a otra, manteniendo un historial de commits lineal y limpio. Esto es una alternativa a `git merge` que ayuda a evitar commits de fusión innecesarios.

---

#### 🚀 Pasos de la Actividad

**Paso 1: Configuración de ramas**

1.  Asegúrate de estar en la rama `main`. Si no es así, muévete a ella.
2.  Crea una nueva rama llamada `refactor` y cámbiate a ella.
3.  En la rama `refactor`, haz un cambio simple en el archivo `index.html` que creaste en la actividad anterior. Por ejemplo, agrega un comentario.
4.  Realiza un commit con el mensaje "refactor: agregar comentario al index".

**Paso 2: Simulación de nuevos cambios en `main`**

1.  Regresa a la rama `main`.
2.  Agrega un nuevo archivo llamado `footer.html` y un texto de pie de página.
3.  Realiza un commit con el mensaje "feat: agregar pie de pagina".

**Paso 3: Sincronización de ramas con `rebase`**

1.  Vuelve a la rama `refactor`.
2.  Usa el comando para aplicar los cambios de la rama `main` sobre tu rama actual (`refactor`).
3.  Observa cómo Git te mostrará que está re-aplicando tu commit ("refactor: agregar comentario al index") después de los commits de `main`.

**Paso 4: Fusión final**

1.  Regresa a la rama `main`.
2.  Usa el comando para fusionar la rama `refactor` en `main`. Observa que Git realizará un "fast-forward merge".
3.  Usa el comando para ver el historial de commits con un gráfico y confirma que el historial es completamente lineal, sin commits de fusión.

---