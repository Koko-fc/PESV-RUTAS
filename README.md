MANUAL DE INSTRUCCIONES
Sistema de Gestión de Rutas Frecuentes — PESV

Contiene: diagrama de flujo · diagrama de interfaz de usuario · manual de uso para administrador
Versión 1.0 — Julio de 2026
 
1. Introducción
Este manual documenta el funcionamiento del Sistema de Gestión de Rutas Frecuentes del Plan Estratégico de Seguridad Vial (PESV) de DELTA GLOBAL S.A.S. Está pensado para servir de referencia en futuras actualizaciones, mantenimiento y capacitación de nuevos administradores.
La aplicación reemplaza el manejo manual de fichas de ruta en Excel por una herramienta dinámica (desarrollada en Python con Streamlit) que permite:
●	Registrar y mantener actualizadas las rutas frecuentes de la empresa.
●	Mostrar a cada conductor los riesgos, puntos críticos y medidas preventivas de su ruta antes de salir.
●	Dejar evidencia (nombre, cédula, fecha y hora) de que el conductor consultó la ruta antes del desplazamiento.
●	Exportar la información a Excel o PDF cuando se requiera para auditorías o capacitaciones.
2. Diagrama de flujo de la aplicación
El siguiente diagrama resume el recorrido que sigue la información dentro de la app, desde que se abre hasta que se genera un registro de consulta o se actualiza una ruta.
 
Diagrama 1. Flujo general de la aplicación: acceso, modo conductor vs. administrador, y persistencia de datos.
Puntos clave del flujo:
●	Al abrir la app, por defecto cualquier persona está en modo conductor y solo ve la opción “Consultar ruta”.
●	Si se ingresa la contraseña correcta en “Acceso administrador” (barra lateral), se desbloquean los 4 menús adicionales.
●	Cada consulta de un conductor (nombre, cédula, ruta) se guarda automáticamente en data/consultas.json, sin necesidad de que el administrador haga nada.
●	Cualquier cambio en el listado de rutas (crear, editar, eliminar) se guarda automáticamente en data/rutas.json.
3. Diagrama de interfaz de usuario
Este esquema (wireframe) representa la distribución de la pantalla principal — la vista de “Consultar ruta” — tal como la ve un conductor, incluyendo la barra lateral con el acceso de administrador.
 
Diagrama 2. Distribución de la interfaz: barra lateral (menú + acceso admin) y panel principal (formulario de consulta + ficha resultante).
3.1 Elementos de la barra lateral
●	Acceso administrador: campo de contraseña que desbloquea el menú completo.
●	Menú: lista de secciones disponibles según el modo (conductor o administrador).
3.2 Elementos del panel principal
●	Formulario de consulta: nombre y cédula del conductor (obligatorios) + selector de ruta.
●	Resultado de la ficha: métricas rápidas (frecuencia, distancia, tiempo, nivel de riesgo), mapa embebido, lista de puntos críticos, medidas preventivas y botón de descarga en PDF.
 
4. Manual de uso — Administrador
4.1 Instalación (primera vez)
Requiere Python 3.9 o superior instalado (con la opción “Add Python to PATH” activada en Windows).
1.	Descomprimir la carpeta pesv_app en el equipo.
2.	Abrir una terminal (CMD) dentro de esa carpeta.
3.	Crear el entorno virtual:
python -m venv venv
4.	Activarlo:
venv\Scripts\activate
5.	Instalar las dependencias:
pip install -r requirements.txt
6.	Ejecutar la app:
streamlit run app.py
4.2 Cómo volver a abrir la app (uso diario)
1. No es necesario repetir la instalación completa. Cada vez que se quiera usar la app:
2. Ubicar dentro de CMD la carpeta: cd pesv_app-main
3. Después de ubicada la carpeta, se debe activar el programa: venv\Scripts\activate
4. Por último se corre el programa: streamlit run app.py
5. Para detener la app: volver a la terminal y presionar Ctrl + C.

Uso con PowerShell (opcional)
Se recomienda usar siempre CMD. En PowerShell el entorno virtual se activa con otro archivo (Activate.ps1) y Windows bloquea por defecto la ejecución de scripts. Si se prefiere PowerShell, ejecutar una sola vez:
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
y luego, cada vez que se quiera usar la app:
.\venv\Scripts\Activate.ps1
streamlit run app.py

4.3 Acceso de administrador
En la barra lateral, abrir el desplegable “Acceso administrador” e ingresar la contraseña configurada. Al validarse, aparecen los menús: Listado de rutas, Nueva ruta, Registro de consultas y Exportar / Respaldo.
Cambiar la contraseña de administrador
Abrir el archivo config.py con el Bloc de notas.
Editar la línea: ADMIN_PASSWORD = "PESV2026"
Reemplazar el valor entre comillas por la nueva clave.
Guardar el archivo y reiniciar la app (streamlit run app.py).
4.4 Gestión de rutas
Crear una ruta nueva
Menú “➕ Nueva ruta”. Diligenciar los campos generales (origen, destino, frecuencia, recorrido, distancia, tiempo, link del mapa de Google My Maps) y agregar los puntos críticos con su nivel de riesgo (Bajo / Medio / Alto). Guardar con el botón “Guardar ruta”.
Editar o eliminar una ruta
Menú “📋 Listado de rutas”. Expandir la ficha de la ruta deseada y usar los botones “✏️ Editar” o “🗑️ Eliminar”. También se puede buscar por nombre/origen/destino o filtrar por nivel de riesgo desde la parte superior de esta pantalla.
Mapa de Google My Maps
Solo se necesita pegar el enlace de “compartir” del mapa (el que contiene mid=...); la app lo convierte automáticamente al formato necesario para mostrarlo embebido dentro de la ficha.
4.5 Registro de consultas (evidencia PESV)
Menú “📝 Registro de consultas”. Muestra una tabla con cada consulta realizada por los conductores: nombre, cédula, ruta consultada, fecha y hora. Se puede filtrar por conductor o por ruta, y descargar el listado completo en CSV para incluirlo como evidencia en auditorías del PESV.
4.6 Exportar y respaldo
Menú “📊 Exportar / Respaldo”. Permite:
●	Descargar todas las rutas en un archivo Excel (una hoja por ruta, con el mismo formato de ficha original).
●	Descargar un respaldo en formato JSON de todas las rutas.
●	Cargar un respaldo JSON previamente descargado, para restaurar o migrar la información (esto reemplaza todas las rutas actuales, así que se recomienda hacerlo con cuidado).
4.7 Descarga de fichas en PDF
Tanto en la vista de consulta (conductor) como en el listado administrativo hay un botón para descargar la ficha completa de una ruta en PDF, con el mismo contenido de la pantalla (datos generales, puntos críticos coloreados por nivel y medidas preventivas).
4.8 Solución de problemas comunes
Problema	Causa probable	Solución
“python no se reconoce como comando”	Python no quedó en el PATH	Reinstalar Python marcando “Add Python to PATH”, o usar py en vez de python
“Could not open requirements file”	La terminal no está en la carpeta correcta (carpeta duplicada tras descomprimir)	Usar dir para verificar la ubicación; entrar con cd a la carpeta donde estén app.py y requirements.txt
El navegador no abre solo	Streamlit no pudo lanzar el navegador automáticamente	Copiar y pegar manualmente http://localhost:8501
El mapa no se ve en la ficha	El link de Google My Maps no es público o cambió de permisos	Verificar que el mapa esté compartido como “cualquiera con el enlace puede ver”
Olvidé la contraseña de administrador	—	Abrir config.py con el Bloc de notas y leer/cambiar el valor de ADMIN_PASSWORD
4.9 Estructura de archivos del proyecto
Archivo / carpeta	Función
app.py	Aplicación principal (ejecutar con streamlit run app.py)
config.py	Contraseña de administrador
utils_pesv.py	Funciones de apoyo: mapa embebido, generación de PDF, registro de consultas
migrar_excel.py	Script para importar un Excel existente al formato de la app
requirements.txt	Lista de librerías necesarias (se instalan con pip install -r requirements.txt)
data/rutas.json	Base de datos de rutas (se crea y actualiza sola)
data/consultas.json	Registro de consultas de conductores (se crea sola)
4.10 Recomendaciones para futuras actualizaciones
●	Antes de modificar app.py, hacer una copia de respaldo de la carpeta completa.
●	Después de cualquier cambio, probar localmente con streamlit run app.py antes de usarlo en el día a día.
●	Los mapas de Google My Maps deberían crearse desde una cuenta institucional de DELTA GLOBAL, no una cuenta personal, para no depender de una sola persona.
●	Si se agregan muchas rutas, considerar mover de data/rutas.json a una base de datos real (por ejemplo SQLite) para mejorar el rendimiento.
●	Este documento debe actualizarse cada vez que se agregue una función nueva a la aplicación.
# PESV-RUTAS
