### Actividad 6: Deshaciendo Cambios Avanzados

#### Ejercicio Guiado - Historial de Rescate y Restauración de Archivos

#### 🎯 Objetivo
En esta actividad, profundizarás en el uso de los comandos de Nivel 4 para recuperar archivos perdidos, entender el historial interno de Git (`reflog`) y practicar la restauración de commits eliminados.

---

#### 🚀 Pasos de la Actividad

**Paso 1: Simulación de un error fatal**

1.  Asegúrate de estar en una rama de prueba (`estilos`) y que el historial de commits sea visible (al menos con dos o tres commits).
2.  Haz un cambio en uno de los archivos existentes y realiza un commit.
3.  Utiliza el comando de deshacer que borra commits del historial de forma permanente, volviendo al commit anterior al que acabas de hacer.

**Paso 2: Rescatando un commit perdido**

1.  Usa el comando para ver el historial interno de referencias de tu repositorio. Observa que el commit que acabas de "borrar" aún está presente en este historial.
2.  Copia el identificador del commit que eliminaste.
3.  Crea una nueva rama a partir del commit que creías perdido.
4.  Cambia a esta nueva rama y verifica que el commit "eliminado" y sus cambios están de vuelta.

**Paso 3: Restauración de archivos individuales**

1.  Regresa a la rama principal.
2.  Modifica el archivo `style.css` y realiza un commit con el mensaje "bug: color incorrecto".
3.  Sin deshacer el commit completo, utiliza el comando de restauración para traer una versión anterior de `style.css` desde un commit anterior. Puedes usar el hash del commit donde tenías el color correcto.
4.  Verifica el estado de tus archivos para ver que `style.css` está en el área de preparación con la versión anterior.
5.  Realiza un commit para guardar esta corrección.

**Paso 4: Limpiando el directorio de trabajo**

1.  Crea un nuevo archivo temporal, por ejemplo `temp.txt`.
2.  Usa el comando de estado para ver que el archivo está presente pero no rastreado.
3.  Utiliza el comando para eliminar permanentemente este archivo del directorio de trabajo, ya que no lo necesitas.

---