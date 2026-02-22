# Escenario-Procedural
CODIGO
Constryuccion de un escenario 3d
import bpy
import random

def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0)
    return mat

def generar_escenario():
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))

    largo_pasillo = 10
    ancho_pasillo = 4

    for i in range(largo_pasillo):
        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo, i * 2, 1))
        pared_izq = bpy.context.active_object

        if i % 2 == 0:
            pared_izq.data.materials.append(mat_pared_a)
        else:
            pared_izq.data.materials.append(mat_pared_b)
            pared_izq.scale.z = 1.5

        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo, i * 2, 1))
        pared_der = bpy.context.active_object
        pared_der.data.materials.append(mat_pared_a)

    bpy.ops.mesh.primitive_plane_add(size=1, location=(0, (largo_pasillo - 1), 0))
    suelo = bpy.context.active_object
    suelo.scale.x = ancho_pasillo + 1
    suelo.scale.y = largo_pasillo + 1
    
DESCRIPCION
Le voy a explicar cómo desarrollé este código para Blender:
 
Primero, comencé importando la librería  bpy , que es la que permite controlar Blender con Python. Después, creé la función  crear_material  porque quería poder generar materiales de forma organizada y reutilizable – le pido un nombre y un color RGB, y la función se encarga de crear el material en Blender con esa configuración, incluyendo el canal alfa para que quede opaco.
 
Luego definí la función principal  generar_escenario . Lo primero que hice ahí fue limpiar la escena: seleccioné y eliminé todos los objetos existentes para empezar de cero y evitar confusiones. Después creé dos materiales específicos: uno gris oscuro llamado 'ParedOscura' y otro naranja rojizo llamado 'ParedDetalle'.
 
También establecí variables para el largo y ancho del pasillo, así es fácil modificar esas dimensiones después sin tener que cambiar código en varios lugares.
 
Para las paredes, usé un ciclo  for  que se repite según el largo del pasillo. En cada vuelta, creo un cubo para la pared izquierda: lo coloco en la posición correcta, y con una condición  if i % 2 == 0  hago que alternen los materiales – cuando es par, usa el gris oscuro, y cuando es impar, usa el naranja y le aumento la altura con  scale.z . La pared derecha la hice igual pero siempre con el material gris para mantener consistencia.
 
Por último, agregué un plano para el suelo, lo ubiqué al final del pasillo y lo escale para que cubriera todo el área del mismo. Al llamar la función  generar_escenario() , se ejecuta todo el proceso y se crea el pasillo completo en la escena de Blender.
 
Lo que logré hacer es automatizar la creación de un escenario estructurado, donde puedo cambiar fácilmente sus medidas o colores solo modificando algunos valores, en lugar de tener que crear cada objeto manualmente uno por uno
generar_escenario()
