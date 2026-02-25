# formulario3.py
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

