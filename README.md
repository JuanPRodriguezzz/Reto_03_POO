# Reto_03_POO
### Juan Pablo Rodríguez Cruz

## 1. **Create a repo with the class exercise**

1. Create class Line.

 - *length*, *slope*, start, end: Instance attributes, two of them being points (so a line is composed at least of two points).
 - compute_length(): should return the line´s length
 - compute_slope(): should return the slope of the line from tje horizontal in deg.
 - compute_horizontal_cross(): should return if exists the intersection with x-axis
 - compute_vertical_cross(): should return if exists the intersection with y-axis

2. Redefine the class Rectangle, adding a new method of initialization using 4 Lines (composition at its best, a rectangle is compose of 4 lines).

3. **Optional:** Define a method called discretize_line() that creates an array on *n* equally spaced points in the line and assigned as a instance attribute.


``` python
from math import atan, degrees
import os
import numpy as np


def limpiar_pantalla():
    """Limpia la consola."""
    os.system("cls" if os.name == "nt" else "clear")


def continuar():
    """Pausa la ejecución hasta que el usuario presione Enter."""
    input("\nPresiona Enter para continuar...")


class Point:
    """Clase que representa un punto en un plano bidimensional."""
    def __init__(self, x: float = 0, y: float = 0):
        self.x = x
        self.y = y


class Line:
    """Clase que representa una línea en un plano bidimensional."""
    def __init__(self, start: Point, end: Point):
        self.start = start
        self.end = end
        self.length = self.compute_length()
        self.slope = self.compute_slope()

    def compute_length(self) -> float:
        """Calcula la longitud de la línea."""
        return ((self.end.x - self.start.x)**2 + (self.end.y - self.start.y)**2)**0.5

    def compute_slope(self):
        """Calcula la pendiente de la línea en grados desde la horizontal."""
        x_displacement = self.end.x - self.start.x
        y_displacement = self.end.y - self.start.y
        if x_displacement == 0:
            return None  # Pendiente indefinida para una línea vertical
        return degrees(atan(y_displacement / x_displacement))

    def compute_horizontal_cross(self) -> bool:
        """Verifica si la línea cruza el eje X."""
        return self.start.y * self.end.y <= 0

    def compute_vertical_cross(self) -> bool:
        """Verifica si la línea cruza el eje Y."""
        return self.start.x * self.end.x <= 0

    def discretize_line(self, n: int) -> np.ndarray:
        """Divide la línea en n puntos igualmente espaciados."""
        x_points = np.linspace(self.start.x, self.end.x, n)
        y_points = np.linspace(self.start.y, self.end.y, n)
        return np.array(list(zip(x_points, y_points)))


class Rectangle:
    """Clase que representa un rectángulo."""

    def __init__(self, width: float = None, height: float = None, center: Point = None, bottom_left: Point = None, top_right: Point = None):
        if bottom_left and top_right:
            self.width = top_right.x - bottom_left.x
            self.height = top_right.y - bottom_left.y
            self.center = Point(bottom_left.x + self.width / 2, bottom_left.y + self.height / 2)
        elif bottom_left and width and height:
            self.width = width
            self.height = height
            self.center = Point(bottom_left.x + width / 2, bottom_left.y + height / 2)
        elif center and width and height:
            self.width = width
            self.height = height
            self.center = center
        else:
            raise ValueError("Debe proporcionar argumentos válidos para inicializar el rectángulo.")

    def compute_area(self) -> float:
        """Calcula el área del rectángulo."""
        return self.width * self.height

    def compute_perimeter(self) -> float:
        """Calcula el perímetro del rectángulo."""
        return 2 * (self.width + self.height)


# Ejemplo de uso
if __name__ == "__main__":
    limpiar_pantalla()

    # Inicialización por esquina inferior izquierda
    bottom_left = Point(0, 0)
    rect1 = Rectangle(bottom_left=bottom_left, width=4, height=3)
    print(f"Área del rectángulo 1: {rect1.compute_area()}")
    print(f"Perímetro del rectángulo 1: {rect1.compute_perimeter()}")

    # Inicialización por el centro
    center = Point(2, 1.5)
    rect2 = Rectangle(center=center, width=4, height=3)
    print(f"Área del rectángulo 2: {rect2.compute_area()}")
    print(f"Perímetro del rectángulo 2: {rect2.compute_perimeter()}")

    # Inicialización por esquinas opuestas
    top_right = Point(4, 3)
    rect3 = Rectangle(bottom_left=bottom_left, top_right=top_right)
    print(f"Área del rectángulo 3: {rect3.compute_area()}")
    print(f"Perímetro del rectángulo 3: {rect3.compute_perimeter()}")

    continuar()

```

## 2. 
**Restaurant scenario:** You want to design a program to calculate the bill for a customer's order in a restaurant.
- Define a base class *MenuItem*: This class should have attributes like name, price, and a method to calculate the total price.
- Create subclasses for different types of menu items: Inherit from *MenuItem* and define properties specific to each type (e.g., Beverage, Appetizer, MainCourse). 
- Define an Order class: This class should have a list of *MenuItem* objects and methods to add items, calculate the total bill amount, and potentially apply specific discounts based on the order composition.

## Diagrama

```mermaid
classDiagram
    class MenuItem {
        + String Name
        + Float Price
        + calculate_price(Price)
    }

    class Beverage {
        + Bool is_alcoholic
    }
    MenuItem <|-- Beverage

    class Appetizer {
    }
    MenuItem <|-- Appetizer

    class MainCourse {
        + Bool is_vegetarian
    }
    MenuItem <|-- MainCourse

    class Order {
        - items: List[MenuItem]
        + add_item(item: MenuItem)
        + calculate_total(float)
        + apply_discount(discount_percentage: float)
        + calculate_auto_discount(float) 
        + show_order_summary(List)
    }

    MenuItem *-- Order : 
```

## Código

```python
import os

class MenuItem:
    """Clase base para un elemento del menú."""

    def __init__(self, name, price):
        self.name = name
        self.price = price

    def calculate_price(self):
        return self.price


class Beverage(MenuItem):
    """Subclase para bebidas."""

    def __init__(self, name, price, is_alcoholic):
        super().__init__(name, price)
        self.is_alcoholic = is_alcoholic


class Appetizer(MenuItem):
    """Subclase para aperitivos."""

    def __init__(self, name, price):
        super().__init__(name, price)
      


class MainCourse(MenuItem):
    """Subclase para platos principales."""

    def __init__(self, name, price, is_vegetarian):
        super().__init__(name, price)
        self.is_vegetarian = is_vegetarian


class Order:
    """Clase que representa el pedido de un cliente."""

    def __init__(self):
        self.items = []

    def add_item(self, item):
        if isinstance(item, MenuItem):
            self.items.append(item)
        else:
            raise ValueError("El elemento debe ser un objeto de tipo MenuItem")

    def calculate_total(self):
        return sum(item.calculate_price() for item in self.items)

    def apply_discount(self, discount_percentage):
        total = self.calculate_total()
        discount = total * (discount_percentage / 100)
        return total - discount

    def calculate_auto_discount(self):
        combos = {
            ("Mojarra", "Ceviche", "Limonada de Coco"): 12,
            ("Mojarra", "Empanadas de camarón(4und)", "Limonada de Coco"): 10,
            ("Chunchullo", "Bandeja Paisa"): 8,
            ("Churrasco", "Empanadas de carne(4und)"): 8,
            ("Medio Churrasco", "Empanadas de carne(4und)"): 5
        }
        
        applied_discount = 0
        for combo, discount in combos.items():
            if all(any(item.name == combo_item for item in self.items) for combo_item in combo):
                applied_discount = max(applied_discount, discount)
        return applied_discount

    def show_order_summary(self):
        print("Resumen del pedido:")
        for item in self.items:
            print(f"{item.name}: ${item.price:.2f}")
        print(f"Total: ${self.calculate_total():.2f}")

    @staticmethod
    def limpiar_pantalla():
        """Limpia la consola."""
        os.system("cls" if os.name == "nt" else "clear")

    @staticmethod
    def continuar():
        """Pausa la ejecución hasta que el usuario presione Enter."""
        input("\nPresiona Enter para continuar...")
        Order.limpiar_pantalla()


def menu_principal():
    menu = [
        Beverage("Gaseosa 600 mL", 7000, False),
        Beverage("Cerveza artesanal", 15000, True),
        Beverage("Cerveza Heineken botella", 10000, True),
        Beverage("Limonada de coco", 10000, False),
        Beverage("Agua", 5000, False),
        Appetizer("Empanadas de pollo(4und)", 10000),
        Appetizer("Empanadas de carne(4und)", 10000),
        Appetizer("Empanadas de camarón(4und)", 12000),
        Appetizer("Chunchullo", 18000),
        Appetizer("Ceviche", 15000),
        MainCourse("Churrasco", 40000, False),
        MainCourse("Medio Churrasco", 28000, False),
        MainCourse("Bandeja Paisa", 38000, False),
        MainCourse("Hamburguesa", 33000, False),
        MainCourse("Hamburguesa vegetariana", 36000, True),
        MainCourse("Mojarra", 38000, False)
    ]

    order = Order()

    while True:
        Order.limpiar_pantalla()
        print("Menú del Restaurante:")
        print("\n--- Bebidas ---")
        for i, item in enumerate(menu, start=1):
            if isinstance(item, Beverage):
                print(f"{i}. {item.name} - ${item.price:.2f}")
        print("\n--- Aperitivos ---")
        for i, item in enumerate(menu, start=1):
            if isinstance(item, Appetizer):
                print(f"{i}. {item.name} - ${item.price:.2f}")
        print("\n--- Platos principales ---")
        for i, item in enumerate(menu, start=1):
            if isinstance(item, MainCourse):
                print(f"{i}. {item.name} - ${item.price:.2f}")

        try:
            choices = input("\nIngrese los números de los items separados por comas: ").strip()

            choices = [int(choice) - 1 for choice in choices.split(",") if choice.strip().isdigit()]
            for choice in choices:
                if 0 <= choice < len(menu):
                    order.add_item(menu[choice])
                    print(f"{menu[choice].name} agregado al pedido.")
                else:
                    print(f"El número {choice + 1} no es válido y se ignoró.")
        except ValueError:
            print("\nEntrada inválida. Por favor, ingrese solo números separados por comas.")

        confirm = input("\n¿Desea agregar más items? (s/n): ").strip().lower()
        if confirm != 's':
            break

    Order.limpiar_pantalla()
    print("\nResumen Final del Pedido:")
    order.show_order_summary()

    confirm = input("\n¿Confirma su pedido? (s/n): ").strip().lower()
    if confirm == 's':
        auto_discount = order.calculate_auto_discount()
        if auto_discount > 0:
            print(f"\nSe ha aplicado un descuento automático del {auto_discount}%.")
            total_with_discount = order.apply_discount(auto_discount)
            print(f"Total después del descuento: ${total_with_discount:.2f}")
        else:
            print("\nNo se aplicó ningún descuento automático.")

        print("\nGracias por su pedido. Que tenga un buen día.")
    else:
        print("\nEl pedido fue cancelado.")


if __name__ == "__main__":
    menu_principal()
```
