# Documentaci-n-de-Software-TGCD
# Calculadora de IMC

Este proyecto es una aplicación de calculadora de Índice de Masa Corporal (IMC) desarrollada en Python utilizando la biblioteca `tkinter` para la interfaz gráfica y `PIL` para el manejo de imágenes.

## Requisitos
- Python 3.x
- Biblioteca `tkinter`
- Biblioteca `PIL` (Python Imaging Library)
- Biblioteca `csv`

## Instalación
1. Asegúrate de tener Python 3.x instalado en tu sistema.
2. Instala la biblioteca `PIL` utilizando pip:

## Estructura del Proyecto
- `main.py`: Contiene el código principal de la aplicación.
- `bascula.jpg`: Imagen utilizada como fondo de la aplicación.

## Funcionalidades
1. **Ingresar Datos del Usuario**: 
- Nombre
- Peso (kg)
- Altura (m)
- Edad
- Sexo (Hombre o Mujer)

2. **Calcular IMC**:
- Utiliza los datos ingresados para calcular el IMC y determina la categoría del IMC (Bajo peso, Peso normal, Sobrepeso, Obesidad grado I, Obesidad grado II, Obesidad grado III).

3. **Mostrar Resultado del IMC**:
- Muestra el IMC calculado junto con su categoría.

4. **Guardar Datos en Archivo CSV** (opcional):
- Guarda los datos ingresados y el resultado del IMC en un archivo CSV con el nombre del usuario.

## Uso
1. Ejecuta el archivo `main.py`:

2. Se abrirá la ventana principal de la aplicación. Ingresa los datos requeridos y haz clic en el botón "Calcular IMC" para ver el resultado.

3. Si deseas guardar los datos en un archivo CSV, haz clic en el botón "Guardar Datos".

## Código Principal
```python
import os
import tkinter as tk
from tkinter import Canvas
from PIL import Image, ImageTk
import csv

# Función para calcular el Índice de Masa Corporal (IMC)
def calcular_imc():
 # Obtener los valores ingresados por el usuario
 peso = float(entry_peso.get())
 altura = float(entry_altura.get())
 edad = int(entry_edad.get())
 sexo = var_sexo.get()

 # Ajuste del factor de sexo
 if (sexo == "Hombre"):
     ks = 1.0
 else:
     ks = 1.1

 # Ajuste del factor de edad
 ka = 1 + 0.01 * (edad - 25)

 # Calcular el IMC
 imc = peso / (altura ** 2) * ks * ka

 # Determinar la categoría del IMC
 if imc < 18.5:
     categoria = "Bajo peso"
 elif 18.5 <= imc < 25.0:
     categoria = "Peso normal"
 elif 25.0 <= imc < 30.0:
     categoria = "Sobrepeso"
 elif 30.0 <= imc < 35.0:
     categoria = "Obesidad grado I"
 elif 35.0 <= imc < 40.0:
     categoria = "Obesidad grado II"
 else:
     categoria = "Obesidad grado III"

 # Mostrar el resultado del IMC en la interfaz
 label_resultado.config(text=f"IMC: {imc:.2f} ({categoria})")

 return imc, categoria

# Función para guardar los datos del usuario en un archivo CSV
def guardar_datos():
 # Obtener los valores ingresados por el usuario
 nombre = entry_nombre.get()
 peso = entry_peso.get()
 altura = entry_altura.get()
 edad = entry_edad.get()
 sexo = var_sexo.get()

 # Calcular el IMC y obtener la categoría
 imc, categoria = calcular_imc()

 # Datos a guardar en el archivo CSV
 datos = [nombre, peso, altura, edad, sexo, imc, categoria]

 # Guardar los datos en un archivo CSV con el nombre del usuario
 with open(f"{nombre}.csv", mode="w", newline="") as file:
     writer = csv.writer(file)
     writer.writerow(["Nombre", "Peso (kg)", "Altura (m)", "Edad", "Sexo", "IMC", "Categoría"])
     writer.writerow(datos)

# Crear la ventana principal de la aplicación
ventana_principal = tk.Tk()
ventana_principal.title("Calculadora de IMC")
ventana_principal.geometry("400x400")

# Ruta de la imagen de fondo
imagen_path = os.path.join(os.path.dirname(__file__), "bascula.jpg")

# Cargar y redimensionar la imagen de fondo
imagen = Image.open(imagen_path)
width, height = 400, 400
resized_image = imagen.resize((width, height), Image.LANCZOS)
imagen_tk = ImageTk.PhotoImage(resized_image)

# Crear un lienzo (canvas) para la imagen de fondo
canvas = Canvas(ventana_principal, width=width, height=height)
canvas.pack(fill="both", expand=True)
canvas.create_image(0, 0, image=imagen_tk, anchor="nw")

# Configuración de la fuente y color del texto
fuente = ("Arial", 14)
color_texto = "black"

# Crear y posicionar los widgets en la interfaz

# Nombre
label_nombre = tk.Label(ventana_principal, text="Nombre:", font=fuente, fg=color_texto, bg="white")
entry_nombre = tk.Entry(ventana_principal, font=fuente, fg=color_texto)
canvas.create_window(120, 50, window=label_nombre, anchor="nw")
canvas.create_window(200, 50, window=entry_nombre, anchor="nw")

# Peso
label_peso = tk.Label(ventana_principal, text="Peso (kg):", font=fuente, fg=color_texto, bg="white")
entry_peso = tk.Entry(ventana_principal, font=fuente, fg=color_texto)
canvas.create_window(120, 90, window=label_peso, anchor="nw")
canvas.create_window(200, 90, window=entry_peso, anchor="nw")

# Altura
label_altura = tk.Label(ventana_principal, text="Altura (m):", font=fuente, fg=color_texto, bg="white")
entry_altura = tk.Entry(ventana_principal, font=fuente, fg=color_texto)
canvas.create_window(120, 130, window=label_altura, anchor="nw")
canvas.create_window(200, 130, window=entry_altura, anchor="nw")

# Edad
label_edad = tk.Label(ventana_principal, text="Edad:", font=fuente, fg=color_texto, bg="white")
entry_edad = tk.Entry(ventana_principal, font=fuente, fg=color_texto)
canvas.create_window(120, 170, window=label_edad, anchor="nw")
canvas.create_window(200, 170, window=entry_edad, anchor="nw")

# Sexo
label_sexo = tk.Label(ventana_principal, text="Sexo:", font=fuente, fg=color_texto, bg="white")
var_sexo = tk.StringVar(value="Hombre")
radio_hombre = tk.Radiobutton(ventana_principal, text="Hombre", variable=var_sexo, value="Hombre", font=fuente, fg=color_texto, bg="white")
radio_mujer = tk.Radiobutton(ventana_principal, text="Mujer", variable=var_sexo, value="Mujer", font=fuente, fg=color_texto, bg="white")
canvas.create_window(120, 210, window=label_sexo, anchor="nw")
canvas.create_window(200, 210, window=radio_hombre, anchor="nw")
canvas.create_window(270, 210, window=radio_mujer, anchor="nw")

# Botón para calcular el IMC
boton_calcular = tk.Button(ventana_principal, text="Calcular IMC", command=calcular_imc, font=fuente, fg=color_texto, cursor="hand2")
canvas.create_window(150, 250, window=boton_calcular, anchor="nw")

# Etiqueta para mostrar el resultado del IMC
label_resultado = tk.Label(ventana_principal, text="IMC:", font=fuente, fg=color_texto, bg="white")
canvas.create_window(150, 290, window=label_resultado, anchor="nw")

# Botón para guardar los datos
boton_guardar = tk.Button(ventana_principal, text="Guardar Datos", command=guardar_datos, font=fuente, fg=color_texto, cursor="hand2")
canvas.create_window(150, 330, window=boton_guardar, anchor="nw")

# Iniciar la ventana principal
ventana_principal.mainloop()
Creado por Torres Gutierrez Cristian David
