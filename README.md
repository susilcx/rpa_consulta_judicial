# \# PRUEBA TÉCNICA TCC RPA #

# \## Herramienta: Community Edition Automation Anywhere. 

# \* Versionamiento del BotAgent 22.240.39

# 

# Se utiliza Automation Anywhere para el desarrollo de este bot debido a la experiencia y habilidad manejando la herramienta.

# 

# \## Configuración del bot

# \*  Instalación del bot agent:

# 

# Primero, inicia sesión en tu instancia de Automation Anywhere desde el navegador. Una vez dentro del portal, selecciona la opción para conectar el dispositivo local.

# 

# !\[Vista del instalador del bot agent](/Imagenes/Imagen1.png)

# 

# Al seleccionar “Connect to my computer”, el sistema descargará automáticamente el instalador AutomationAnywhereBotAgent.exe.

# Una vez descargado el archivo:

# 

# 1\. Ejecuta el instalador como administrador.

# 2\. Espera a que se abra el asistente de instalación.

# 3\. Selecciona el tipo de instalación.

# 

# Se recomienda utilizar la opción:

# 

# Anyone who uses this computer (all users)

# 

# !\[Seleccionar Anyone who uses this computer](/Imagenes/Imagen2.png)

# 

# El asistente instalará automáticamente los componentes necesarios del Bot Agent. Cuando el proceso termine, aparecerá el mensaje de instalación completada. Haz clic en Finish para finalizar.

# 

# !\[Finalización de la instalación](/Imagenes/Imagen3.png)

# 

# !\[Bot Agent conectado exitosamente](/Imagenes/Imagen4.png)

# 

# \----

# 

# \*  Instalación de la extensión de Automation Anywhere:

# 

# Para la ejecución del bot es importante tener la extensión de Automation 360, pues esta nos ayudará a reconocer elementos que se encuentren dentro de Chrome.

# 

# !\[Extensión Automation 360](/Imagenes/Imagen5.png)

# 

# \---

# \*  Configuración de carpetas: 

# 

# Para la correcta ejecución del bot, se debe tener en cuenta la siguiente estructura de carpetas. Esto permitirá una mejor organización del proyecto, facilitará el mantenimiento del flujo y ayudará a mantener una estructura más clara y predecible.

# 

# !\[Configuración de carpetas](/Imagenes/Imagen6.png)

# 

# \---

# \*  Diccionario de configuración:

# 

# Las rutas deben configurarse de acuerdo con la ubicación en la que se encuentre la estructura de carpetas definida previamente. Asimismo, desde este apartado es posible modificar distintas variables y parámetros de configuración según las necesidades del proceso o del entorno de ejecución.

# 

# !\[Archivo de configuración inicial](/Imagenes/Imagen7.png)

# 

# 

# Recuerda modificar la ruta del diccionario en la siguiente variable en el main del bot:

# 

# !\[Variables a modificar](/Imagenes/Imagen8.png)

# 

# 

# 

# \## Descripción del flujo

# 

# \### Flujo General del Bot

# 

# El bot inicia su ejecución desde la tarea principal (`Main`). Durante esta etapa se realiza la configuración inicial, en la cual se extraen todas las rutas y parámetros necesarios desde el diccionario de configuración. Adicionalmente, se inicializa el sistema de logs, creando un archivo `.txt` que permite registrar la trazabilidad completa de la ejecución del bot.

# 

# Los logs se clasifican en dos tipos:

# 

# \- \*\*Información:\*\* permite identificar el estado actual y el paso en el que se encuentra el bot durante la ejecución.

# \- \*\*Errores:\*\* registra cualquier excepción o inconveniente presentado durante el recorrido del flujo.

# 

# Asimismo, cada registro incluye el nombre de la tarea ejecutada, facilitando la identificación del punto exacto del proceso.

# 

# \---

# 

# \* Conexión y Validación de Correos

# 

# Una vez finalizada la configuración inicial, el bot realiza la conexión a la bandeja de correos y busca mensajes cuyo asunto sea \*\*"CONSULTA JUDICIAL"\*\*.

# 

# Si existe un correo con este asunto, el bot accede a su contenido y extrae el nombre configurado dentro de la parametrización establecida.

# 

# Si el campo del nombre contiene información válida, el bot inicia la navegación web accediendo al portal de consulta de procesos de la Rama Judicial:

# 

# https://consultaprocesos.ramajudicial.gov.co/Procesos/NombreRazonSocial

# 

# Dentro del portal se realizan las siguientes acciones:

# 

# 1\. Selección de la opción \*\*"Todos los procesos"\*\*.

# 2\. Selección del tipo de persona \*\*"Natural"\*\*.

# 3\. Ingreso del nombre extraído desde el cuerpo del correo.

# 4\. Ejecución de la consulta mediante el botón correspondiente.

# 

# \---

# 

# \### Validación de Resultados

# 

# \* Sin registros encontrados

# 

# Si no se encuentran registros asociados a la búsqueda, el bot enviará un correo notificando al usuario que no existen procesos relacionados con la persona consultada.

# 

# \* Registros encontrados

# 

# En caso de encontrar resultados, el bot identificará el registro número 6. Si dicho registro existe, realizará el clic correspondiente, esperará la carga de la pantalla de detalle del proceso y posteriormente seleccionará la opción de descarga de documentos.

# 

# \- Si no existe un archivo disponible para descarga, el bot notificará al usuario mediante correo electrónico.

# \- Si la descarga se realiza correctamente, el archivo será enviado automáticamente al usuario final como adjunto en un correo electrónico.

# 

# \---

# 

# \* Gestión del Archivo Descargado

# 

# El archivo es descargado inicialmente en la carpeta de descargas del sistema. Posteriormente, se realizan una serie de validaciones y comparaciones para identificar correctamente el archivo descargado y moverlo a la carpeta `Output`, donde es renombrado antes de ser enviado al usuario final. 

# 

# \---

# 

# \* Registro de Ejecución

# 

# Finalmente, el bot registra el resultado de la ejecución en el archivo de control interno (`ACI`), permitiendo al programador llevar un seguimiento de los diferentes estados y resultados finales de cada ejecución del proceso.

# 

# \---

# 

# !\[Flujo TO-BE](/Imagenes/Imagen9.png)

# 

# \## Decisiones técnicas claves

# \* Estrategia para identificar el sexto resultado:

# 

# Para identificar la existencia del 6 resultado se utiliza la ubicación en la tabla, es decir en que index y row de la tabla debería estar el registro.

# 

# !\[Identificación del registro 6 en la tabla](/Imagenes/Imagen10.png)

# 

# Para poder darle clic y generar la pantalla de detalle del proceso, se realizo una serie de interacciones con el DOMXPATH y PATH, ya que estos cambiaban en algunas ubicaciones según la tabla. 

# 

# !\[DOMXPATH y PATH](/Imagenes/Imagen11.png)

# 

# \---

# \* Como se extrae el nombre del correo: 

# 

# Lo ideal es que al momento de enviar el correo se envie bajo la plantilla de "NOMBRE A CONSULTAR: Nombres y Apellidos."  (Como se muetra en la imagen) 

# 

# 

# !\[Correo de muestra](/Imagenes/Imagen12.png)

# 

# Con esto  el bot mediante la función Extract text, extraera todo lo que se encuentre entre los dos puntos (:) y el punto (.)

# 

# !\[Task Extract Text](/Imagenes/Imagen13.png)

# 

# \---

# \* Como se maneja la descarga del archivo:

# 

# El archivo es descargado inicialmente en la carpeta de descargas del sistema. Posteriormente, se realizan una serie de validaciones y comparaciones para identificar correctamente el archivo descargado y moverlo a la carpeta Output, donde es renombrado antes de ser enviado al usuario final.

# 

# !\[Descarga del archivo](/Imagenes/Imagen14.png)

# 

# 

# \## Limitaciones y trabajo pendiente

# Debido a que el desarrollo fue realizado utilizando la Community Edition de Automation Anywhere, se presentaron ciertas limitaciones técnicas y funcionales. Por esta razón, se contemplan futuras mejoras y refactorizaciones en el flujo, así como la implementación de funcionalidades adicionales orientadas a optimizar la estabilidad, escalabilidad y mantenibilidad del bot.

# 

# Posibles mejoras a implementar

# \* Migración del archivo de control interno a base de datos:

# 

# Se planea reemplazar el archivo de control interno por una base de datos, con el fin de mejorar la administración, trazabilidad y escalabilidad de la información.

# 

# \* Implementación de triggers para monitoreo de correos:

# 

# Se buscará implementar triggers que permitan mantener un monitoreo constante y automatizado sobre la llegada de nuevos correos electrónicos.

# 

# \* Optimización de la extracción de nombres de usuario:

# 

# Se mejorará la plantilla utilizada para la extracción del nombre del usuario encargado de realizar la búsqueda, aumentando la precisión del proceso.

# 

# \* Mejora en el manejo y captura de errores:

# 

# Se fortalecerá la gestión de excepciones y captura de errores para garantizar una mayor estabilidad y confiabilidad del bot durante la ejecución.

# 

# \* Implementación de nuevas alternativas para la obtención de archivos:

# 

# Se evaluará el uso de APIs u otros mecanismos de integración para optimizar la obtención y procesamiento de archivos.

