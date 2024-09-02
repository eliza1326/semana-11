# semana-11
Creacion de un programa para un local de venta de materias primas, productos terminado con composiciones quimicas y envases de difrentes materiales.



class Producto:
    def __init__(self, id_producto, nombre, cantidad, precio):
        self.id_producto = id_producto
        self.nombre = nombre
        self.cantidad = cantidad
        self.precio = precio

    def __str__(self):
        return f"ID: {self.id_producto}, Nombre: {self.nombre}, Cantidad: {self.cantidad}, Precio: {self.precio}"

    def actualizar_cantidad(self, cantidad):
        self.cantidad = cantidad

    def actualizar_precio(self, precio):
        self.precio = precio


        import json

class Inventario:
    def __init__(self):
        self.productos = {}

    def añadir_producto(self, producto):
        if producto.id_producto in self.productos:
            print("Error: El producto con ese ID ya existe.")
        else:
            self.productos[producto.id_producto] = producto
            print(f"Producto {producto.nombre} añadido al inventario.")

    def eliminar_producto(self, id_producto):
        if id_producto in self.productos:
            del self.productos[id_producto]
            print(f"Producto con ID {id_producto} eliminado del inventario.")
        else:
            print("Error: Producto no encontrado.")

    def actualizar_producto(self, id_producto, cantidad=None, precio=None):
        if id_producto in self.productos:
            if cantidad is not None:
                self.productos[id_producto].actualizar_cantidad(cantidad)
            if precio is not None:
                self.productos[id_producto].actualizar_precio(precio)
            print(f"Producto con ID {id_producto} actualizado.")
        else:
            print("Error: Producto no encontrado.")

    def buscar_producto_por_nombre(self, nombre):
        for producto in self.productos.values():
            if producto.nombre == nombre:
                print(producto)
                return producto
        print("Producto no encontrado.")
        return None

    def mostrar_productos(self):
        if self.productos:
            for producto in self.productos.values():
                print(producto)
        else:
            print("El inventario está vacío.")

    def guardar_inventario(self, archivo):
        with open(archivo, 'w') as f:
            productos_serializados = {id_prod: vars(prod) for id_prod, prod in self.productos.items()}
            json.dump(productos_serializados, f)
            print(f"Inventario guardado en {archivo}.")

    def cargar_inventario(self, archivo):
        try:
            with open(archivo, 'r') as f:
                productos_serializados = json.load(f)
                self.productos = {id_prod: Producto(**datos) for id_prod, datos in productos_serializados.items()}
                print(f"Inventario cargado desde {archivo}.")
        except FileNotFoundError:
            print("Archivo no encontrado.")
        except json.JSONDecodeError:
            print("Error al leer el archivo.")


def menu():
    inventario = Inventario()
    archivo_inventario = "inventario.json"
    
    while True:
        print("\n--- Menú de Gestión de Inventario ---")
        print("1. Añadir Producto")
        print("2. Eliminar Producto")
        print("3. Actualizar Producto")
        print("4. Buscar Producto por Nombre")
        print("5. Mostrar Todos los Productos")
        print("6. Guardar Inventario en Archivo")
        print("7. Cargar Inventario desde Archivo")
        print("8. Salir")
        
        opcion = input("Selecciona una opción: ")

        if opcion == "1":
            id_producto = input("ID del Producto: ")
            nombre = input("Nombre del Producto: ")
            cantidad = int(input("Cantidad: "))
            precio = float(input("Precio: "))
            producto = Producto(id_producto, nombre, cantidad, precio)
            inventario.añadir_producto(producto)
        
        elif opcion == "2":
            id_producto = input("ID del Producto a eliminar: ")
            inventario.eliminar_producto(id_producto)
        
        elif opcion == "3":
            id_producto = input("ID del Producto a actualizar: ")
            cantidad = input("Nueva cantidad (deja en blanco para no cambiar): ")
            precio = input("Nuevo precio (deja en blanco para no cambiar): ")
            cantidad = int(cantidad) if cantidad else None
            precio = float(precio) if precio else None
            inventario.actualizar_producto(id_producto, cantidad, precio)
        
        elif opcion == "4":
            nombre = input("Nombre del Producto a buscar: ")
            inventario.buscar_producto_por_nombre(nombre)
        
        elif opcion == "5":
            inventario.mostrar_productos()
        
        elif opcion == "6":
            inventario.guardar_inventario(archivo_inventario)
        
        elif opcion == "7":
            inventario.cargar_inventario(archivo_inventario)
        
        elif opcion == "8":
            print("Saliendo del sistema...")
            break
        
        else:
            print("Opción no válida. Intenta de nuevo.")

if __name__ == "__main__":
    menu()
