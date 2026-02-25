# Proyecto integrador
Formulario de datos personales de un alumno hecho con Python y las herramientas de Flet

<img width="613" height="484" alt="image" src="https://github.com/user-attachments/assets/5e51dc20-9370-476b-9ba0-651369be3750" />

Formulario 1

import flet as ft

def main(page: ft.Page):
    # Configuración de la página según el diseño [cite: 18]
    page.title = "Ejercicio Unidad 1 - Registro de Datos"
    page.padding = 30
    page.bgcolor = "#FDFBE3" 
    
    # --- CONTROLES (Subtema 1.4)  ---
    
    # En versiones nuevas, si 'label' o 'text' fallan, se usa 'value' o 'content'
    txt_nombre = ft.TextField(label="Nombre", border_color="#4D2A32")
    
    txt_telefono = ft.TextField(label="Teléfono", expand=True, border_color="#4D2A32")
    
    txt_cp = ft.TextField(label="C.P.", expand=True, border_color="#4D2A32")
    
    dd_estado = ft.Dropdown(
        label="Estado",
        expand=True,
        border_color="#4D2A32",
        options=[
            ft.dropdown.Option("Morelos"),
            ft.dropdown.Option("Puebla"),
            ft.dropdown.Option("Estado de México"),
        ]
    )
    
    # FORMA UNIVERSAL PARA BOTONES EN VERSIONES NUEVAS:
    # Si 'text' falla, usamos un control Text dentro de 'content'
    btn_enviar = ft.ElevatedButton(
        content=ft.Text("Enviar", color="black"),
        bgcolor="grey",
        expand=True,
        style=ft.ButtonStyle(
            shape=ft.RoundedRectangleBorder(radius=0),
        )
    )

    # --- ESTRUCTURA (Tema 1.1)  ---
    page.add(
        ft.Column([
            txt_nombre,
            ft.Row([txt_telefono, txt_cp]),
            ft.Row([dd_estado, btn_enviar])
        ], spacing=20)
    )

# Ejecución para Web Browser [cite: 10]
ft.app(target=main, view=ft.AppView.WEB_BROWSER)



Formulario 2

import flet as ft

def main(page: ft.Page):
    # Configuración de página para entorno Web/Pyodide
    page.title = "Registro de Estudiantes - Tópicos Avanzados"
    page.bgcolor = "#FDFBE3"  # Fondo crema de la imagen
    page.padding = 30
    page.theme_mode = ft.ThemeMode.LIGHT

    # --- CONTROLES DE ENTRADA (Subtema 1.4) ---
    txt_nombre = ft.TextField(label="Nombre", border_color="#4D2A32",expand=True)
    txt_control = ft.TextField(label="Numero de control", border_color="#4D2A32", expand=True)
    txt_email = ft.TextField(label="Email", border_color="#4D2A32")

    dd_carrera = ft.Dropdown(
        label="Carrera",
        expand=True,
        border_color="#4D2A32",
        options=[
            ft.dropdown.Option("Ingeniería en Sistemas"),
            ft.dropdown.Option("Ingeniería Civil"),
            ft.dropdown.Option("Ingeniería Industrial"),
        ]
    )

    dd_semestre = ft.Dropdown(
        label="Semestre",
        expand=True,
        border_color="#4D2A32",
        options=[ft.dropdown.Option(str(i)) for i in range(1, 7)]
    )
    
    # Campos que se integrarán como Dropdowns posteriormente
    txt_carrera = ft.TextField(label="Carrera", expand=True, border_color="#4D2A32")
    txt_semestre = ft.TextField(label="Semestre", expand=True, border_color="#4D2A32")

    # Contenedor para Genero (Se integrará Radio posteriormente)
    # Por ahora mantenemos la estructura visual con Texto
    row_genero = ft.Row([
        ft.Text("Genero:", color="#4D2A32", weight=ft.FontWeight.BOLD),
        ft.Text("Macho / Hembra / Otro (Espacio para Radio)")
    ], alignment=ft.MainAxisAlignment.START)

    # Botón Enviar adaptado a versión 0.80.6.dev (usando content)
    btn_enviar = ft.ElevatedButton(
        content=ft.Text("Enviar", color="black", size=16),
        bgcolor=ft.Colors.GREY_500,
        width=page.width, # Ocupa el ancho disponible
        style=ft.ButtonStyle(
            shape=ft.RoundedRectangleBorder(radius=0),
        )
    )

    # --- CONSTRUCCIÓN DE LA INTERFAZ (Subtema 1.1) ---
    page.add(
        ft.Column([
            txt_nombre,
            txt_control,
            txt_email,
            # Fila para Carrera y Semestre
            ft.Row([
                dd_carrera,
                dd_semestre
            ], spacing=10),
            # Espacio para el Genero
            row_genero,
            # Botón final
            btn_enviar
        ], spacing=15)
    )

# Ejecución específica para visualización en Navegador
ft.app(target=main, view=ft.AppView.WEB_BROWSER)


1. Estructura Principal y Configuración de Página
import flet as ft: Importa la librería. Usar el alias ft es el estándar para acceder a todos los controles (widgets) de forma rápida.
def main(page: ft.Page):: Esta es la función de entrada. El objeto page representa la ventana del navegador o de la aplicación. Es "el lienzo" donde pondremos todo.
page.title, page.bgcolor, page.padding: Son propiedades visuales.
padding = 30: Crea un margen interno para que los elementos no toquen los bordes de la ventana.
theme_mode = ft.ThemeMode.LIGHT: Fuerza a la aplicación a usar colores claros, ignorando la configuración del sistema operativo.

2. El "Overlay" y Mensajes Dinámicos
Python
def mostrar_mensaje(texto, color):
    snack = ft.SnackBar(content=ft.Text(texto), bgcolor=color)
    page.overlay.append(snack)
    snack.open = True
    page.update()

ft.SnackBar: Es una pequeña barra informativa que aparece en la parte inferior.
page.overlay.append(snack): En Flet, ciertos elementos (como diálogos o snackbars) no se añaden al cuerpo de la página, sino a una "capa superior" llamada overlay.
page.update(): Crucial. En Flet, los cambios en el código no se ven en la pantalla automáticamente hasta que llamas a update(). Es como "refrescar" el estado visual.

3. Controles de Entrada de Datos (Inputs)
Los controles se definen primero para poder extraer sus valores después:
ft.TextField: Una caja de texto.
expand=True: Hace que el control ocupe todo el espacio disponible en su fila o columna.
ft.Dropdown: Un menú desplegable.
options: Una lista de objetos ft.dropdown.Option. En el código usas una comprensión de listas para generar los semestres del 1 al 6: [ft.dropdown.Option(str(i)) for i in range(1, 7)].
ft.RadioGroup: Agrupa varias opciones de selección única (ft.Radio). A diferencia de los otros, el valor seleccionado se guarda en el contenedor padre (radio_genero).

4. Lógica de Validación y Eventos
Python
def enviar_datos(e):
    nombre = txt_nombre.value
    # ... validaciones ...

El parámetro e: Representa el "Evento". Aunque no lo uses dentro de la función, Flet lo envía automáticamente cuando haces clic en un botón.
.value: Es la propiedad que extrae lo que el usuario escribió o seleccionó en el control.
Validaciones:
.isalpha(): Verifica que el texto contenga solo letras.
.isdigit(): Verifica que el texto contenga solo números.
if not carrera: Verifica si el valor está vacío (el usuario no eligió nada).

5. Organización del Layout (Diseño)
Flet usa un sistema de cajas basado en Flexbox:
ft.Column: Organiza los elementos uno debajo del otro (vertical).
ft.Row: Organiza los elementos uno al lado del otro (horizontal).
spacing=10: El espacio en píxeles entre los elementos dentro de la fila/columna.
alignment=ft.MainAxisAlignment.START: Alinea el contenido al inicio de la fila.

6. Ejecución del Programa
ft.app(target=main, view=ft.AppView.WEB_BROWSER):
target=main: Indica qué función debe ejecutar al iniciar.
view=ft.AppView.WEB_BROWSER: Abre la aplicación directamente en tu navegador en lugar de una ventana nativa de escritorio.

Resumen de Herramientas de Flet utilizadas
Herramienta
Función
Page
Contenedor raíz de toda la app.
TextField
Captura de texto del usuario.
Dropdown
Selección de una lista cerrada de opciones.
RadioGroup
Selección única mediante botones circulares.
ElevatedButton
Botón con sombra que dispara la acción de guardar.
SnackBar
Notificación emergente de éxito o error.
Column/Row
Estructuras para maquetar la interfaz.


Validación de espacios vasios
1. La obtención del Valor (.value)
Cada control que creaste (como txt_nombre o dd_carrera) es un objeto. Cuando el usuario interactúa con ellos, Flet almacena lo que se escribe o selecciona en la propiedad .value.

Al presionar el botón "Enviar", lo primero que hace tu función es capturar esos estados actuales:

Python
nombre = txt_nombre.value  # Esto obtiene el string actual del campo
carrera = dd_carrera.value # Esto obtiene la opción seleccionada o None
2. El uso de la lógica "Truthy/Falsy" de Python
Flet aprovecha que en Python, los strings vacíos ("") y los valores nulos (None) se evalúan como False en un contexto booleano.

La validación que usas en tu código funciona así:

Python
if not nombre:
    # Si 'nombre' está vacío (""), 'not nombre' es True.
    mostrar_mensaje("Error...", "red")
    return # Detiene la ejecución para que no se guarden datos vacíos
3. Diferencias según el tipo de Control
Flet maneja el "vacío" de formas ligeramente distintas dependiendo del control:

TextField (Nombre, Email): Si el usuario no escribe nada, .value devuelve una cadena vacía ("").

Dropdown (Carrera, Semestre): Si el usuario no ha desplegado y elegido una opción, .value suele ser None o una cadena vacía. Por eso usas if not carrera:.

RadioGroup (Género): Si no se marca ninguna opción, el valor es None.

4. Limpieza de espacios en blanco (Trim)
Un error común en formularios es que el usuario presione la barra espaciadora y deje espacios. Tu código maneja esto de forma inteligente en el nombre:

Python
if not nombre or not nombre.replace(" ", "").isalpha():
Aquí, replace(" ", "") elimina temporalmente los espacios para verificar si realmente hay contenido o si solo son caracteres invisibles.


Validación de correos electrónicos
1. Extracción del valor
Flet toma el contenido del control txt_email a través de su propiedad .value. Esta propiedad siempre devuelve un String.

Si el campo está vacío, devuelve "".

Si el usuario escribió algo, devuelve el texto exacto.

2. Triple comprobación lógica
El condicional if utiliza tres reglas conectadas por el operador or (que significa que si cualquiera de estas se cumple, el correo se considera inválido):

not email: Verifica si el campo está vacío. Si no hay texto, la validación falla.

"@" not in email: Busca el símbolo de la arroba. Es el requisito mínimo de cualquier correo electrónico. Si el usuario escribe "usuario.com", esta regla lo detecta como error.

"." not in email: Busca la existencia de un punto (que representaría el dominio como .com, .org, etc.).

3. Retroalimentación visual
Si la validación falla, se ejecutan dos acciones de Flet:

Bloqueo: El return detiene la función, impidiendo que el código llegue a la parte de "Datos guardados".

Notificación: Se llama a la función mostrar_mensaje, que abre el ft.SnackBar en la parte inferior de la pantalla.

Una forma más avanzada: Expresiones Regulares (Regex)
La validación actual es básica (deja pasar cosas como @.). En proyectos más profesionales de Flet, solemos usar el módulo re de Python para una validación exhaustiva.

Aquí te muestro cómo se vería esa herramienta integrada en tu código:

Python
import re

def validar_email_pro(email):
    # Expresión regular para un formato de email estándar
    patron = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(patron, email)

# Dentro de enviar_datos:
if not validar_email_pro(txt_email.value):
    mostrar_mensaje("Formato de correo no permitido", "red")
    return



1. Anatomía de un Dropdown en Flet
Un Dropdown se compone de dos partes fundamentales:

El contenedor (ft.Dropdown): Define el diseño, la etiqueta y guarda el valor seleccionado.

Las opciones (ft.dropdown.Option): Son los elementos individuales dentro del menú.

2. Implementación en tu código
En tu script, utilizaste dos Dropdowns con enfoques ligeramente distintos para las opciones:

A. Definición Manual (Carrera)
Para el campo de carrera, escribiste cada opción una por una. Esto es útil cuando las opciones son pocas y estáticas.

Python
dd_carrera = ft.Dropdown(
    label="Carrera",
    expand=True,
    options=[
        ft.dropdown.Option("Ingeniería en Sistemas"),
        ft.dropdown.Option("Ingeniería Civil"),
        ft.dropdown.Option("Ingeniería Industrial"),
    ]
)
B. Generación Dinámica (Semestre)
Para el semestre, utilizaste una comprensión de listas de Python. Esto es mucho más eficiente que escribir 6 líneas de código idénticas.

Python
options=[ft.dropdown.Option(str(i)) for i in range(1, 7)]
range(1, 7) genera números del 1 al 6.

str(i) convierte el número en texto (Flet requiere que el valor de la opción sea un String).

Se crea un objeto ft.dropdown.Option para cada número automáticamente.

3. Propiedades clave que utilizaste
label: Es el texto flotante que indica al usuario qué debe seleccionar.

expand=True: Como el Dropdown está dentro de una ft.Row (Fila), esta propiedad hace que el control se estire para ocupar todo el espacio disponible, permitiendo que "Carrera" y "Semestre" compartan la fila proporcionalmente.

border_color: Define el color del borde para que coincida con la estética de tu app (#4D2A32).

4. Cómo se captura la selección
Cuando el usuario hace clic en una opción, el texto de esa opción se almacena en la propiedad .value del objeto Dropdown.

En tu función de validación lo extraes así:

Python
carrera = dd_carrera.value # Si eligió Sistemas, carrera vale "Ingeniería en Sistemas"

if not carrera:
    # Si el valor es None (no eligió nada), lanza el error
    mostrar_mensaje("Seleccione una carrera.", "red")
5. El flujo de renderizado
Cuando Flet dibuja el Dropdown:

Crea el campo visual con el borde y la etiqueta.

Prepara una "ventana emergente" (popover) que contiene las Options.

Al seleccionar una, Flet actualiza internamente el estado del control y cierra el menú.



'''

import flet as ft

def main(page: ft.Page):
    page.title = "Registro de Estudiantes - Tópicos Avanzados"
    page.bgcolor = "#FDFBE3"
    page.padding = 30
    page.theme_mode = ft.ThemeMode.LIGHT

    # -------- FUNCION PARA MOSTRAR MENSAJE --------
    def mostrar_mensaje(texto, color):
        snack = ft.SnackBar(
            content=ft.Text(texto),
            bgcolor=color
        )
        page.overlay.append(snack)
        snack.open = True
        page.update()

    # -------- VALIDACION --------
    def enviar_datos(e):
        nombre = txt_nombre.value
        control = txt_control.value
        email = txt_email.value
        carrera = dd_carrera.value
        semestre = dd_semestre.value

        # Validar nombre (solo letras)
        if not nombre or not nombre.replace(" ", "").isalpha():
            mostrar_mensaje("El nombre solo debe contener letras.", "red")
            return
        
        # Validar número de control (solo números)
        if not control or not control.isdigit():
            mostrar_mensaje("El número de control solo debe contener números.", "red")
            return
        
        # Validar email
        if not email or "@" not in email or "." not in email:
            mostrar_mensaje("Ingrese un email válido.", "red")
            return
        
        # Validar carrera seleccionada
        if not carrera:
            mostrar_mensaje("Seleccione una carrera.", "red")
            return
        
        # Validar semestre seleccionado
        if not semestre:
            mostrar_mensaje("Seleccione un semestre.", "red")
            return
        
        # Si todo está correcto
        mostrar_mensaje("Datos guardados correctamente.", "green")

    # -------- CONTROLES --------
    txt_nombre = ft.TextField(label="Nombre", border_color="#4D2A32", expand=True)
    txt_control = ft.TextField(label="Numero de control", border_color="#4D2A32", expand=True)
    txt_email = ft.TextField(label="Email", border_color="#4D2A32")

    dd_carrera = ft.Dropdown(
        label="Carrera",
        expand=True,
        border_color="#4D2A32",
        options=[
            ft.dropdown.Option("Ingeniería en Sistemas"),
            ft.dropdown.Option("Ingeniería Civil"),
            ft.dropdown.Option("Ingeniería Industrial"),
        ]
    )

    dd_semestre = ft.Dropdown(
        label="Semestre",
        expand=True,
        border_color="#4D2A32",
        options=[ft.dropdown.Option(str(i)) for i in range(1, 7)]
    )

    # -------- RADIO BUTTON GENERO --------
    radio_genero = ft.RadioGroup(
        content=ft.Row([
            ft.Radio(value="Macho", label="Macho"),
            ft.Radio(value="Hembra", label="Hembra"),
            ft.Radio(value="Otro", label="Otro"),
        ])
    )

    row_genero = ft.Row([
        ft.Text("Genero:", color="#4D2A32", weight=ft.FontWeight.BOLD),
        radio_genero
    ], alignment=ft.MainAxisAlignment.START)

    btn_enviar = ft.ElevatedButton(
        content=ft.Text("Enviar", color="black", size=16),
        bgcolor=ft.Colors.GREY_500,
        on_click=enviar_datos,
        style=ft.ButtonStyle(
            shape=ft.RoundedRectangleBorder(radius=0),
        )
    )

    # -------- INTERFAZ --------
    page.add(
        ft.Column([
            txt_nombre,
            txt_control,
            txt_email,
            ft.Row([
                dd_carrera,
                dd_semestre
            ], spacing=10),
            row_genero,
            btn_enviar
        ], spacing=15)
    )

ft.app(target=main, view=ft.AppView.WEB_BROWSER)


'''

